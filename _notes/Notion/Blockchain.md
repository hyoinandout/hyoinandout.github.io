# Blockchain

- First Block
    
    첫 블록 : Genesis Block
    
    type block struct{
    
    data string
    
    hash string
    
    prevHash string
    
    }
    
    B1
    
    data : genesisblock, hash = sha256(data+prevhash), prevhash = “”
    
    prevHash와 data 조합으로 hash를 만들어내기 때문에 chain 꼴이며, 데이터 변경여부를 쉽게 알 수 있다.
    
    Singleton : 우리의 application에서 오로지 하나의 instance
    
    go의 sync package의 [once.do](http://once.do) 로 오직 한번만 실행되게 할 수 있다
    
- visualization
    
    marshal로 json처리 := newencoder(rw).encode
    
    go의 interface : 상속한다고 명시할 필요 없이 함수만 구현해주면 됨
    
    mux - multiplexer, url 요청에 대한 handler mapping 제공
    
    generic 이랑 비슷한 adapter pattern 이라는게 있다
    
- db연결
    
    bucket 두개 : block, blockchain
    
    block에는 data, blockchain에는 newestHash와 height
    
    defer : _exit 같은 개념 (함수 끝나기 전에 실행)
    
- 채굴 (pow (poc))
    
    pos가 환경적으로 좋긴한
    
    nonce만 변경가능 근데 구현 어려움
    
    검증은 쉽지만 풀기 어려운것 : n개의 0으로 시작하는 n찾기.. difficulty로 (비트코인이랑 비슷한 방식)
    
    nonce 변경가능 → noce 변경하면서 일
    
    얼마나 빠르게 블록 생성되는지 확인해서 난도조절
    
- transaction
    
    입력: 내가 가진 돈 출력: 거래 끝난후 각자가 갖고있는 돈 액수 1-2
    
    코인베이스 입력값 : blockchain → hyoin
    
    mempool : 확정되지 않은 거래내역
    
    블록안에 거래내역이 있다면 확정된 허가내역 → 수수료 붙는 이유 (전력, 컴퓨팅 파워)
    
    label : for문 중첩될때 탈출하게
    
    method와 function의 차이 : method는 객체 변경시에 
    
    once.Do에서 f가 또 Do func call을 하면 deadlock
    
- wallet
    
    verification&signature : public  / private
    
    - hash the msg : “i love you” → hash(x) → “hashed_message”
    - generate key pair : Keypair(privateK, publicK) (save priv to a file)
    - sign the hash : (”hashed_message”  + privateK) → “signature”
    - verify : (”hashed_message” + “signature” + publicK) → true/false
    
    wallet은 private key
    
    go 문법 중에서 a ,b := f() / c,b := f() 가능 ↔ _,b:=f() / _b:=f() 불가능
    
    참고) named return ex) func reStoreKey() (key *ecdsa.PrivateKey) : var 초기화 하고 끝에 return만 때려주면 알아서 return해줌
    
    참고) anonymous struct
    
    결론:
    
    type Tx struct{
    
    TxIn[
    
    (Txout1)
    
    (Txout2)
    
    ]
    
    Sign: X
    
    }
    
    TxIn.Sign + Txout1.Address = true if the money is yours
    
    Address = wallet where others send you money
    
- p2p
    
    decentralized p2p network
    
    - channel
        
        goroutine - thread
        
        channel - thread간 통신,pipe같은 거임(WaitForSingleObject, recv signal)
        
        f() {c ← person +” is sexy”} main(){println(←c)}
        
        close 가능
        
        read/write 전용으로 할 수 있음 ex f(i int, c chan← int)
        
        BUFFER channel 만들 수 있음 ex c:=make(chan int,5)
        
    - webSocket(WS)
        
        protocol http:stateless / ws : stateful
        
        stateful
        
        websocket reading message 는 blocking
        
    
    서버 - 클라 관계가 아닌 node-node 관계 구현을 위해 p2p
    
    data race: 둘 이상의 go 루틴이 app의 동일 데이터에 access할때 → mutex
    
    struct로 mutex 접근 (협약)
    
    - block chain p2p cases (각 port에 저장된 block height에 따라 case나뉨)
        
        ![Untitled](/assets//Blockchain/Untitled.png)
        
        ![Untitled](/assets//Blockchain/Untitled%201.png)
        
    
    iota ~= enum
    
    새로운 block 채굴하면 broadcast로 다른 노드에도 알릴 수 있음
    
    ![Untitled](/assets//Blockchain/Untitled%202.png)
    

on-chain : metadata(block hash, ipfs hash, 파일 정보들)

off-chain: data - 실제 data(영상.이미지)