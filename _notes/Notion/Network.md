# 네트워크 프로그래밍

- 개론
    
    식당 안내인 예시
    
    고객 입장
    
    1. 핸드폰 준비 (소켓 준비)
    
    2. 식당 번호로 문의 (서버 주소로 connect)
    
    대리인을 통해 식당측과 대화 가능 (소켓을 통해 서버와 패킷 송수신 가능)
    
    식당 입장
    
    1. 안내원 고용 (Listener 소켓 준비)
    2. 안내원 교육(대표 번호 배정) (Bind : 서버주소/Port를 소켓에 연동)
    3. 영업 시작 (Listen)
    4. 안내 (Accept)
    
    대리인을 통해 손님과 대화 가능 (클라 세션을 통해 손님과 통화 가능)
    
- 함수들
    - 클라-서버 공용
        - winsock 처음 쓸 때 : WSAStartup - WSACleanup
            - WSAStartup(MAKEWORD(2,2),&wsaData)
        - 전화기 만들기 : socket(int af, int type, int protocol)
            - af : AF_INET(IPv4)
            - type : **tcp** (안전) SOCK_STREAM / udp (퀵) SOCK_DGRAM
            - protocol : 0
            - return : descriptor
        
        ```cpp
        SOCKADDR_IN serverAddr; //IPv4
        ::memset(&serverAddr,0,sizeof(serverAddr));
        serverAddr.sin_family = AF_INET;
        ::inet_pton(AF_INET,"127.0.0.1",&serverAddr.sin_addr);
        // host to network short: host에서 network endian 방식을 맞춰주겠다는 말
        serverAddr.sin_port = ::htons(7777)
        ```
        
        - ::connect(clientSocket, (SOCKADDR*)&serverAddr,sizeof(serverAddr))
        - 소켓 리소스 반환 : ::closesocket(clientSocket);
    - 서버
        
        나의 주소는?
        
        ```cpp
        SOCKADDR_IN serverAddr; //IPv4
        ::memset(&serverAddr,0,sizeof(serverAddr));
        serverAddr.sin_family = AF_INET;
        // 니가 알아서 해줘
        serverAddr.sin_addr.s_addr = ::htonl(INADDR_ANY)
        // host to network short: host에서 network endian 방식을 맞춰주겠다는 말
        serverAddr.sin_port = ::htons(7777)
        ```
        
        - 안내원 폰 개통 : 식당의 대표 번호  ::bind(listenSocket,(SOCKADDR*)&serverAddr,sizeof(serverAddr))
            - 문지기
        - 영업 시작: ::listen(listenSocket,10);
            - 뒤에 숫자는 대기열로 보면 됨
        - SOCKET clientSocket = ::accept(listenSocket,(SOCKADDR*)&clientAddr,sizeof(clientAddr))
            - 실제로 손님과 통신하는 소켓
        - ip 알아보기 ::inet_ntop(AF_INET,&clientAddr.sin_addr,ipAddress,sizeof(ipAddress));
- TCP
    
    ```cpp
    char sendBuffer[100] = "Hello World!";
    ::send(clientSocket,sednBuffer,sizeof(sendBuffer),0);
    ```
    
    ```cpp
    char recvBuffer[100];
    ::recv(clientSocket,recvBuffer,sizeof(recvBuffer),0);
    ```
    
    위 둘은 blocking 함수 그러나 받아주는 (서버쪽) 부분 없음에도 클라의 send는 성공한다고 나옴
    
    - 소켓 입출력 버퍼
        
        client                                                          server
        
        ⬇️hello ⬆️send 완료!
        
        ——————————————————————————
        
        recvBuffer                                                recvBuffer
        
        sendBuffer(hello)                                     sendBuffer
        
        만약 sendBuffer가 꽉차 있는 상태라면?
        
        이럴 때는 send blocking 됨. 잠듦
        
        recvBuffer 비어있는데 recv 요청하면은 마찬가지로 block
        
        버퍼 방식이기 때문에 데이터들이 뭉쳐지거나 쪼개져서 보내질 수 있음
        
    
- UDP
    
    tcp와 다른점: 소켓 하나만 만들어주면 됨
    
    ```cpp
    SOCKET serverSocket = ::socket(AF_INET,SOCK_DGRAM,0);
    SOCKADDR_IN serverAddr; //IPv4
    ::memset(&serverAddr,0,sizeof(serverAddr));
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_addr.s_addr = htonl(INADDR_ANY);
    // host to network short: host에서 network endian 방식을 맞춰주겠다는 말
    serverAddr.sin_port = ::htons(7777)
    ```
    
    bind 하고
    
    ```cpp
    SOCKADDR_IN clientAddr;
    ::memset(&clientAddr,0,sizeof(clientAddr));
    int32 addrLen = sizeof(clientAddr);
    
    char recvBuffer[100];
    
    int32 recvLen = ::recvfrom(serverSocket,recvBuffer,sizeof(recvBuffer),0,(SOCKADDR*)&clientAddr,&addrLen);
    데이터를 보낼 때 포트 매핑 됨
    ::sendto(serverSocket,recvBuffer,recvLen,0,(SOCKADDR*)&clientAddr,sizeof(clientAddr));
    ```
    
    for문으로 10번 돌려서 메세지 보내고 서버 쪽에서 sleep_for(1s) 했을 때 tcp는 1000, udp는 100 → 데이터 경계
    
    connected UDP라는 것도 있는데 connect하고 send | recv 호출해주면 된다
    
- **TCP VS UDP**
    
    네트워크 : 택배 예시
    
    ![48AA17E9-210D-4EF3-8446-3D43B5876407.jpeg](/assets/Network/48AA17E9-210D-4EF3-8446-3D43B5876407.jpeg)
    
    - tcp: 안전한 트럭, 전화 연결 방식
        - 연결형 서비스
            - 연결을 위해 할당되는 논리적 경로가 있음
            - 전송 순서 보장됨
        - 신뢰성 Good, 속도 Bad
            - 분실이 일어나면 책임지고 다시 전송한다
            - 물건을 주고 받을 상황이 아니면 일부만 보냄(흐름/혼잡 제어)
            - 고려할 것이 많으니 속도가 bad
        - 컨베이어 벨트 (Hello World → HelloWor ld)
    - udp: 위험한 총알 배송, 이메일 전송방식
        - 비연결형 서비스
            - 연결 개념 없음
            - 전송 순서 보장 안됨
            - 경계의 개념이 있음
        - 신뢰성 Bad, 속도 Good
            - 분실에 대한 책임 없음
            - 일단 보내고 생각한다
            - 단순하기 때문에 속도가 good
        - 택배 포장 (Hello World → World Hello)
- 소켓 옵션
    
    옵션을 해석하고 처리할 주체?
    
    - 소켓 코드 → SOL_SOCKET
        - SO_KEEPALIVE : 주기적으로 연결 상태 확인 여부 (TCP Only)
            - 상대방이 소리소문 없이 연결 끊는다면? 주기적으로 tcp 연결 상태 확인, 끊어진 연결 감지
            
            ```cpp
            bool enable=true;
            setsocketopt(serverSocket,SOL_SOCKET,SO_KEEPALIVE,(char*)&enable,sizeof(enable));
            ```
            
        - SO_LINGER : 지연하다
            - 송신 버퍼에 있는 데이터를 보낼것인가 날릴것인가
            - onoff = 0 이면 closesocket() 바로 리턴. 아니면 linger 초만큼 대기
            
            ```cpp
            LINGER linger;
            linger.l_onoff=1;
            linger.l_linger=5;
            setsocketopt(serverSocket,SOL_SOCKET,SO_KEEPALIVE,(char*)&linger,sizeof(linger));
            ```
            
        - Half - Close
            - SD_SEND: send 막는다
            - ::shutdown(serverSocket,SD_SEND);
        - SO_SNDBUF | SO_RCVBUF
            - 송신 버퍼/ 수신 버퍼 크기
            - ::getsockopt(serverSocket,SOL_SOCKET,SO_SNDBUF,(char*)&sendBufferSize,&optionLen);
        - SO_REUSEADDR : ip주소 및 port 재사용
            - ::setsockopt(serverSocket,SOL_SOCKET,SO_REUSEADDR,(char*)&enable,sizeof(enable))
            - 클라 - 서버 연결 끊겼을때, (재로그인같은거 할때) 찌끄레기 남아있어서 다시 사용하려면 시간 걸릴 수도 있음. 이때 이거 사용해주면 됨
    - IPv4 → IPPROTO_IP
    - TCP 프로토콜 → IPPROTO_TCP
        - TCP_NODELAY = 네이글 알고리즘 작동 여부
            - 데이터가 충분히 크면 보내고, 그렇지 않으면 데이터가 충분히 쌓일때 까지 대기
                - 장점 : 작은 패킷이 불필요하게 많이 생성되는 일을 방지
                - 단점 : 반응 시간 손해
                
                ```cpp
                bool enable = true;
                //네이글 알고리즘 작동하게
                ::setsockopt(serverSocket,IPPROTO_TCP,TCP_NODELAY,(char*)&enable, sizeof(enable));
                ```
                
    
    getsocketopt()
    
    setsocketopt()
    
- 논블로킹 소켓
    - 블로킹
        - accept : 접속한 클라 있을 때
        - connect : 서버 접속 성공 했을 때
        - send, sendto: 요청 데이터를 송신 버퍼에 복사 했을 때
        - recv, recvfrom : 수신 버퍼에 도착한 데이터가 있고, 이를 유저레벨 버퍼에 복사했을 때
        
        쓰레드끼리의 컨텍스트 스위칭 비용이 커져서 블로킹이 아니라 언젠가는 논블로킹 쪽으로 넘어가야함
        
    - 논블로킹
        - u_long on = 1; **::ioctlsocket(listenSocket,FIONBIO&on)**
        
        ```cpp
        while(true)
        {
        	SOCKET clientSocket = :: accept(listenSocket,(SOCKADDR*)&clientAddr,&addrLen);
        	// 원래 블록했어야 하는데 너가 논블로킹으로 하라며?
        	if(::WSAGetLastError()==WSAEWOULDBLOCK)
        	{
        		continue;
        	}
        	break;
        }
        ```
        
        Recv도 마찬가지
        
        무한루프 도는게 상당히 비효율적 → 이걸 해결하면!
        
        기존 쓰던 블로킹은 갑질 메타였다
        
- Select 모델
    
    준비가 되지 않았는데 비동기 처리하는거는 오히려 비효율
    
    select 모델 = (select 함수가 핵심이 되는) 클라에서는 select. 서버는 iocp
    
    **소켓 함수 호출이 성공할 시점을 미리 알 수 있다.**
    
    문제 상황)
    
    - 수신 버퍼에 데이터가 없는데 read 한다거나
    - 송신 버퍼가 꽉 찼는데 write 한다거나
    
    블로킹 소켓 : 조건 만족 되지 않아서 블로킹 되는 상황 예방
    
    논블로킹 소켓 : 조건 만족 되지 않아서 불필요하게 반복체크하는 상황 예방
    
    socket set
    
    - 읽기[] 쓰기[] 예외(OutOfBand)[] 관찰 대상 등록
    - select(readSet,WriteSet,exceptSet);
    - 적어도 하나의 소켓이 준비되면 리턴 → 낙오자는 알아서 제거됨
    - 남은 소켓 체크해서 진행
    
    fd_set read;
    
    - FD_ZERO : 비운다
    - FD_SET : 소켓 s를 넣는다
    - FD_CLR : 소켓 s를 제거
    - FD_ISSET : 소켓 s가 set에 들어있으면 0이 아닌 값을 리턴
    
    ```cpp
    vector<Session> sessions;
    sessions.reserve(100);
    
    fd_set reads;
    fd_set writes;
    
    while(true)
    {
    	FD_ZERO(&reads);
    	FD_ZERO(&writes);
    
    	FD_SET(listenSocket,&reads);
    	//...
    
    	int32 retVal = ::select(0,&reads,&writes,nullptr,nullptr);
    	if(FD_ISSET(listenSocket,&reads))
    	{
    		//accept
    	}
    }
    ```
    
    장점: 무작정 accept .recv .send 호출하는 것이 아닌 진짜 그 함수가 호출될 준비가 되었는지 확인 후 호출
    
    단점: 소켓 배열 최대 크기가 64, 그렇다고 동접 64명만 처리할 수 있다는 뜻이 아님 + 매번 등록해줘야함
    

장점 : 윈도우/ 리눅스 공통

단점: 성능 최하(매번 등록비용), 64개 제한

- WSAEventSelect 모델
    
    WSAEventSelect 함수가 핵심
    
    소켓과 관련된 네트워크 이벤트를 [이벤트 객체]를 통해 감지 ,비동기
    
    - 함수들
        
        WSACreateEvent 생성
        
        WSACloseEvent 삭제
        
        WSAWaitForMultipleEvents 신호 상태 감지
        
        WSAEnumNetworkEvents 구체적인 네트워크 이벤트 알아내기
        
    
    소켓과 이벤트 객체를 연동
    
    - WSAEventSelect(socket,event,networkEvents)
        1. WSAEventSelect 함수를 호출하면 자동으로 넌블로킹 모드로 전환
        2. accpet()함수가 리턴하는 소켓은 listenSocket과 동일한 속성을 가짐
            1. clientSocket은 FD_READ,FD_WRITE 등을 다시 등록 필요
        3. 드물게 WSAEWOULDBLOCK 오류 뜰 수 있음
        4. **이벤트 발생시 적절한 소켓 함수 호출 필요**
            1. 아니면 다음번에는 동일 네트워크 이벤트가 발생 X
            2. ex) FD_READ 이벤트 떴으면 recv() 호출해야하고, 안하면 FD_READ 두번다시 X
    - 네트워크 이벤트
        
        FD_ACCEPT
        
        FD_READ
        
        FD_WRITE
        
        FD_CLOSE
        
        FD_CONNECT
        
        FD_OOB
        
    - WSAWaitForMultipleEvents
        1. count, event
        2. waitAll
        3. timeout
        4. false
        
        return : 완료된 첫번째 인덱스
        
    - WSAEnumNetworkEvents
        1. socket
        2. eventObject : 이벤트 객체 핸들 넘겨주면 이벤트 객체를 non signal로 바꿈
        3. networkEvent 네트워크 이벤트/ 오류 정보 저장
    - 순서도
        
        ```cpp
        vector<WSAEVENT> wsaEvents;
        
        //...
        
        WSAEVENT listenEvent = ::WSACreateEvent();
        wsaEvents.push_back(listenEvent);
        if(::WSAEventSelect(listenSocket,listenEvent,FD_ACCEPT|FD_CLOSE) == SOCKET_ERROR)
        	return 0;
        
        while(true)
        {
        	int32 index = ::WSAWaitForMultipleEvents(wsaEvents.size(),&wsaEvents[0],FALSE,WSA_INFINITE,FALSE);
        	//...
        	//::WSAResetEvent(wsaEvents[index]);
        	WSANETWORKEVENTS networkEvents;
        	if(::WSAEnumNetworkEvents(session[index].socket,wsaEvents[index],&networkEvents)==SOCKET_ERROR)
        		continue;
        	if(networkEvents.INetworkEvents & FD_ACCEPT)
        	{
        			//에러 체크
        			//...
        			SOCKADDR_IN clientAddr;
        			int32 addrLen = sizeof(clientAddr);
        			SOCKET clientSocket = ::accept(listenSocket,(SOCKADDR*)&clientAddr,&addrLen);
        			if(clientSocket != INVALID_SOCKET)
        			{
        					//다시 등록(read,write)
        					//...
        			}
        			//recv 인지 send인지 정해주고 FD_CLOSE 해주면 끝, 넌블로킹이기 때문에 EWOULDBLOCK 체크까지!
        		
        ```
        
    
    매번 전체 리셋을 할 필요가 없음
    
    EnumNetworkEvent로 리셋함
    
    갯수에 한계가 있음
    

장점: 비교적 뛰어난 성능

단점: 64개 제한

- Overlapped 모델
    
    ![Untitled](/assets/Network/Untitled.png)
    
    블로킹 / 논블로킹 : 함수를 호출하면 대기하는지? 바로 완료되는지?
    
    동기 / 비동기 : 동시에 일어나는 / 동시에 일어나지 않는(동시에 일어날 필요가 없는)
    
    지금까지 사용한 send,recv는 동기함수
    
    overlapped io : async - nonblocking
    
    1. overlapped 함수를 건다 WSARecv, WSASend
    2. overlapped 함수가 성공했는지 확인 후
    3. 성공했으면 결과 얻어서 처리
    4. 실패했으면 사유를 확인
    - WSASend | WSARecv
        1. 비동기 입출력 소켓
        2. WSABUF 배열의 시작 주소 + 개수 (Scatter-gather)
        3. 보내고/받은 바이트 수
        4. 상세 옵션인데 0
        5. WSAOverlapped 구조체 주소값
        6. 입출력이 완료되면 OS가 호출할 콜백 함수
    
    WSASend | WSARecv 할 때 buffer 함부로 바꾸면 안됨. 오염됨
    
    - event 기반
        1. 비동기 입출력 지원하는 소켓 생성 + 통지 받기 위한 이벤트 객체 생성
        2. 비동기 입출력 함수 호출 (1에서 만든 이벤트 객체를 같이 넘겨줌)
        3. 비동기 작업이 바로 완료되지 않으면, WSA_IO_PENDING 오류 코드
        운영체제는 이벤트 객체를 signaled 상태로 만들어서 완료상태 알려줌
        4. WSAWaitForMultipleEvents 함수 호출해서 이벤트 객체의 signal 판별
        5. WSAOverlappedResult 호출해서 비동기 입출력 결과 확인 및 데이터 처리
        - WSAOverlappedResult
            1. 비동기 소켓
            2. 넘겨준 overlapped 구조체
            3. 전송된 바이트 수
            4. 비동기 입출력 작업이 끝날 때 까지 대기할지?
            false
            5. 비동기 입출력 작업 관련 부가 정보. 거의 사용 안함
        
        ```cpp
        while(true)
        {
        	// connect client ...
        	
        	Session session = Session{clientSocket};
        	WSAEVENT wsaEvent = ::WSACreateEvent();
        	session.overlapped.hEvent = wsaEvent;
        	while(true)
        	{
        		WSABUF wsaBuf;
        		wsaBuf.buf = session.RecvBuffer;
        		wsaBuf.len = BUFSIZE;
        		
        		DWORD recvLen = 0;
        		DWORD flags = 0;
        		if(::WSARecv(clientSocket,&wsaBuf,1,&recvLen,&flags,&session.overlapped,nullptr)==SOCKET_ERROR)
        		{
        			if(::WSAGetLastError() ==WSA_IO_PENDING)
        			{
        				::WSAWaitForMultipleEvents(1,&wsaEvent,TRUE,WSA_INFINITE,FALSE)
        				::WSAGetOverlappedResult(session.socket,&session.overlapped,&recvLen,FALSE,&flags);
        			}
        			else
        			{
        				//문제 상황
        				break;
        			}
        		}
        }
        ```
        
    - 콜백 기반
        
        completion routine 콜백 기반
        
        1. 비동기 입출력 지원하는 소켓 생성
        2. 비동기 입출력 함수 호출(완료 루틴의 시작 주소를 넘겨준다)
        3. 비동기 작업이 바로 완료되지 않으면, WSA_IO_PENDING 오류 코드
        4. 비동기 입출력 함수 호출한 쓰레드를 → Alertable Wait 상태로 만든다
        ex) WaitForSingleObjectEx, WaitForMultipleObjectEx, SleepEx, WASWaitForMultipleEvents
        5. 비동기 IO 완료되면, 운영체제는 완료루틴 호출
        6. 완료 루틴 호출이 모두 끝나면, 쓰레드는 Alertable Wait 상태에서 빠져나온다
        
        void CompletionRoutine()
        
        1. 오류 발생시 0 아닌 값
        2. 전송 바이트 수
        3. 비동기 입출력 함수 호출시 넘겨준 WSAOVERLAPPED 구조체의 주소값
        4. 0
        
        ![Untitled](/assets/Network/Untitled%201.png)
        
        ```cpp
        void CALLBACK RecvCallback(DWORD error, DWORD recvLen, DLPWSAOVERLAPPED overlapped, DWORD flags)
        {
        	cout<<"Data Recv Len Callback = "<<recvLen<<endl;
        	Session* session = (Session*)overlapped;
        }
        while(true)
        {
        	WSABUF wsaBuf;
        	wsaBuf.buf = session.recvBuffer;
        	wsaBuf.len = BUFSIZE;
        	
        	DWORD recvLen = 0;
        	DWORD flags = 0;
        	if(::WSARecv(clientSocket, &wsaBuf, 1, &recvLen, &flags, &session.overlapped, RecvCallback)==SOCKET_ERROR)
        	{
        		if(::WSAGetLastError() == WSA_IO_PENDING)
        		{
        			//Pending
        			//Alertable Wait
        			::SleepEx(INFINITE,TRUE);
        		}
        		else
        		{
        			//TODO : 문제 있는 상황
        				break;
        		}
        	}
        	else
        	{
        		cout<<"Data Recv Len = "<<recvLen<<endl;
        	}
        }
        ```
        
        장점: 
        
        1. socket 하나당 event 하나 연동 안시켜줘도 됨. 
        2. 한 쓰레드당 64개씩 묶어서 처리 안해줘도 됨.
        3. 어느 소켓에서 콜백 호출해줬는지 알고싶을 때 클래스 안에 overlapped 구조체를 첫 멤버 변수로 넣으면 캐스팅을 해서 어느 소켓에서 그 콜백을 호출해줬는지 등과 같은 정보를 알 수 있다.
    
- 이벤트 기반
    
    장점: 성능
    
    단점: 64개 제한
    
- 콜백 기반
    
    장점: 성능
    
    단점: 모든 비동기 소켓 함수에서 사용 가능하진 않음(accept). 빈번한 Alertable Wait으로 인한 성능 저하(일감 분배 차원)
    
- IOCP
    - overlapped 모델(완료루틴 기반)
        - 비동기 입출력 함수 완료되면, 쓰레드마다 있는 APC 큐에 일감이 쌓임
        - Alertable Wait 상태로 들어가서 APC 큐 비우기(콜백 함수)
        
        - 단점: APC 큐 쓰레드마다 있고, Alertable Wait 자체도 부담
        - 단점: 이벤트 → 소켓 : 이벤트 1:1 대응
    
    IOCP 모델
    
    - APC → Completion Port(쓰레드마다 있는 것이 아니라 1개만 있다. 중앙 관리 APC 큐)
    - Alertable Wait → CP 결과 처리를 GetQueuedCompletionStatus
    - 쓰레드랑 궁합 아주 좋음
    
    CreateIoCompletionPort
    
    GetQueuedCompletionStatus
    
    ```cpp
    void WorkerThreadMain(HANDLE iocpHandle)
    {
    	while(true)
    	{
    		DWORD bytesTransferred = 0;
    		Session* session = nullptr;
    		OverlappedEx* overlappedEx = nullptr;
    		
    		BOOL ret = ::GetQueuedCompletionStatus(iocpHandle, &bytesTransferred, (ULONG_PTR*)&session, 
    		(LPOVERLAPPED*)overlappedEx,INFINITE);
    		
    		if(ret == FALSE || bytesTransferred == 0)
    		{
    			//todo : 연결 끊김
    			continue;
    		}
    		
    		//overlappedEx를 통해 어떤 동작하고 있는지 확인 가능
    		ASSERT_CRASH(overlappedEx->type == IO_TYPE::READ);
    		cout<<"Recv Data IOCP = " <<bytesTransferred <<endl;
    		
    		//낚싯대를 던졌다가 올리고 다시 던지는 행위
    		WSABUF wsaBuf;
    		wsaBuf.buf = session->recvBuffer;
    		wsaBuf.len = BUFSIZE;
    		
    		DWORD recvLen = 0;
    		DWORD flags = 0;
    		//read 라서 overlapped 재사용
    		::WSARecv(session->socket, &wsaBuf, 1, &recvLen,&flags, &overlappedEx->overlapped,NULL);
    	}
    }
    ```
    
    ```cpp
    //CP 생성
    HANDLE iocpHandle = CreateIoCompletionPort(INVALID_HANDLE_VALUE,NULL,0,0)
    
    //WorkerThreads
    for(int32 i=0; i<5;i++)
    	GThreadManager->Launch([=](){WorkerThreadMain(iocpHandle);});
    //Main Thread = Accept 담당
    while(true)
    {
    	//...accept
    	//힙 영역에서 쓰레드끼리 공유하게 하기 위해 동적 생성
    	Session* session = xnew<Session>();
    	session->socket = clientSocket;
    	sessionManager.push_back(session);
    	
    	cout << "Client connected!"<<endl;
    
    	//소켓을 CP에 등록 , key를 통해 정보 추출 가능
    	::CreateIoCompletionPort((HANDLE)clientSocket, iocpHandle, /*key*/(ULONG_PTR)session, 0);
    	
    	WSABUF wsaBuf;
    	wsaBuf.buf = session->recvBuffer;
    	wsaBuf.len = BUFSIZE:
    	
    	OverlappedEx* overlappedEx = new OverlappedEx();
    	overlappedEx->type = IO_TYPE::READ;
    
    	// 후에 ADD_REF해서 session 안날아가게 해야함, overlapped 정보 추출 가능
    	DWORD recvLen = 0;
    	DWORD flags = 0;
    	::WSARecv(clientSocket, &wsaBuf, 1, &recvLen, &flags, &overlappedEx->overlapped,NULL);
    ```
    
    유효하지 않은 주소 참조할 때는 ? (게임 플레이 도중 유저가 나갈경우 /overlapped 구조체도 마찬가지)
    
    IOCP 등록하면 날리지 못하게 : RefCount