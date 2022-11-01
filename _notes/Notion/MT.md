# 멀티쓰레드 프로그래밍

- 멀티쓰레드
    - 개론
        
        heap data 공유
        
    - 쓰레드 생성
        
        windows : CreateThread, 종속적
        
        따라서 c++11부터 추가된 #include <thread> 이용하는게 좋다
        
        std::thread t(void *f);
        
        - APIs
            
            t.join()
            
            t.hardware_concurrency : cpu core 개수?
            
            t.get_id() : 쓰레드 마다의 id
            
            t.detach() : 부모 쓰레드와 연결고리를 끊는다 ( trhead 객체에서 실제 쓰레드를 분리)
            
            t.joinable() : id가 0인지 확인함으로써, 쓰레드 살아있는지 확인
            
        
        베릴드 템플릿, functor, lambda
        
    - Atomic
        
        sum++ =
        
        어셈블리어로 봤을 때 int32 eax = sum; eax= eax+1; sum=eax; (컴퓨터 구조상 어쩔 수 없음)
        
        동기화 기법들이 있다.
        
        <atomic> : 쪼개지지 않음(all-or-nothing)
        
        atomic<int32> sum = 0;
        
        sum.fetch_add()
        
        mutex와 같은 원리(타 쓰레드 접근 잠깐 막아버림)
        
        하지만 느림.필요할 때만
        
    - Lock
        
        atomic보다 조금 더 일반적인 상황
        
        stl은 멀티쓰레드에서 잘 동작 안함 (vector 쓰레드 두개에서 push_back할때 (vector 특성상 double free 일어남))(reserve해줘도 같은 index에 밀어넣을수도)
        
        atomic<vector<int>> ? 안됨 (세부적 기능 활용 못함)
        
        따라서 lock(mutex) (mutual exclusive) (상호 배타적)
        
        windows에서는 criticalsection 이지만, c++에서는 mutex
        
        mutex m;
        
        m.lock() & m.unlock() (순차적 접근을 보장)
        
        경합이 있어 느리게 동작
        
        재귀적으로 lock 가능? 불가능, 따로 있음
        
        수동으로 lock 관리하면 복잡해짐.
        
        RAII 패턴이 있다. 생성자에서 잠구고 소멸자에서 풀어준다 : std::lockguard
        
        참고) unique_lock : defer_lock : 잠기는 시점 뒤로 미룰 수 있다.
        
    - Deadlock
        
        lockguard로도 deadlock 무조건 피할 수 있는 것은 아님
        
        서로가 서로를 참조할 때
        
        순서를 정해주면 해결 가능  ex) accountLock → userLock 과 같은 식으로
        
        m.lock(m1,m2) 나 adopt_lock 같은 방법 도 있음
        
    - Lock 구현 이론
        
        lock 풀리기 기다릴 때 방법
        
        1. 존버 (spinlock)
            1. 왔다갔다 하는 비용 없음 (컨텍스트 스위칭) (유저 모드 → 커널 모드)
                
                ![Untitled](/assets/MT/Untitled.png)
                
        2. 일단 자리로, 나중에 다시(random) (sleep)
        3. 갑질 메타(직원한테 부탁) (event)
    - SpinLock
        
        volatile : compiler한테 최적화 하지 마라는 것과 같음
        
        bool로 구현했을 때 동시에 lock 가질 수도 있음
        
        atomic하게 하려면 CAS(Compare-And-Swap)  (atomic 안 volatile 있음)
        
        - cas
            
            ```cpp
            //cas 의사코드
            if(locked == expected)
            {
            	expected = locked;
            	locked = desired;
            	return true;
            }
            else
            {
            	expected = locked;
            	return false;
            }
            ```
            
            ```cpp
            atomic<bool> _locked = false;
            bool expected = false;
            bool desired = true;
            
            _locked
            _locked.compare_exchange_strong(bool& _Expected, const bool _Desired) volatile
            ```
            
            ```cpp
            //아래와 같은 코드임
            //while(flag)
            //{
            //}
            //flag = true;
            
            //이런 식으로 사용
            while(_locked.compare_exchange_strong(expected,desired)==false)
            {
            	expected = false;
            }
            ```
            
        
        경합이 붙어서 스핀락이 무한으로 돈다면 CPU 점유율 높아짐
        
    - Sleep
        
         timeslice
        
        실행 소유권(timeslice)를 포기한다
        
        ![Untitled](/assets/MT/Untitled%201.png)
        
        this_thread::sleep_for(std::chrono::miliseconds(100))
        
        this_thread::yield() == this_thread::sleep_for(0)
        
        교훈 : system call 자제하자
        
    - Event(윈도우즈 시프랑 관련있음)
        
        장점: 효율
        
        단점: 커널모드 개입
        
        따라서 대기 확실할 때 쓰면 좋다
        
        unique_lock?
        
        ```cpp
        #include <windows.h>
        //커널 오브젝트
        //Usage Count
        // Signal(파) / Non-signal(빨)
        handle h = ::CreateEvent(null/*보안속성*/,false/*bManualReset*/,false/*binitialState*/,null)
        
        ::SetEvent(handle); /* non -> signal */
        ::WaitForSingleObject(handle,infinite);
        ```
        
        너무 빈번하게 일어나면 안하는게 좋을수도
        
    - Condition variable (Event 변종)
        
        event랑 lock 별개다
        
        ```cpp
        #include <mutex>
        
        // 유저레벨 오브젝트이다.
        condition_variable cv;
        
        // producer
        // 1) Lock
        // 2) 공유변수 값 수정
        // 3) unlock
        // 4) 조건 변수 통해 다른쓰레드에게 통지 : cv.notify_*();
        
        //consumer
        //cv.wait(lock,[](){return q.empty() == false;});
        // 1) Lock잡으려 시도
        // 2) 조건 확인
        // 3) -만족 -> 빠져나와서 이어서 코드 진행
        // 4) -만족X -> Lock 풀어주고 대기상태
        // unique lock 사용가능, lock guard 사용 불가 (lock 풀어줘야해서)
        
        //spurious wakeup(가짜 기상)
        //notify_one 할때 lock을 잡고있는 것이 아니기 때문에 한번더 확인해줘야
        
        //lock안에 notify_one? 별 도움 안됨(cv와 lock 순서 실행 문제 문제 해결되지 않음)
        ```
        
        cpu낭비 아끼고 최대한 효율적으로 동작
        
        event보다 cv가 더 좋아요
        
    - future
        
        단발성 이벤트 같은 경우임 (mutex, cv까지 가지 않고 간단하게 처리하는 것)
        
        ```cpp
        #include <future>
        
        {
        	// 1) deferred -> lazy evaluation 지연해서 실행하세요
        	// 2) async -> 별도의 쓰레드를 만들어서 실행하세요
        	// 3) deferred | async -> 둘 중 알아서 골라주세요
        	std::future<int64> future = std::async(std::launch::async,Calculate);
        	
        	//언젠가 미래에 결과물을 뱉어줄 것이다
        	std::future_status = future.wait_for(1ms); // ready?
        
        	int64 sum = future.get();//결과물이 이제 필요하다
        }
        ```
        
        동기 비동기
        
        future : 비동기 
        
        객체 멤버 함수도 사용 가능
        
        ```cpp
        {
        	//미래(std::future)에 결과물을 반환해줄거라 약속(std::promise) 해줘~(계약서)
        	std::promise<string> promise;
        	std::future<string> future = promise.get_future();
        }
        ```
        
        promise : 창구 개념
        
        ```cpp
        {
        	std::pacakged_task<int64(void)> task(Calculate);
        	std::future<int64> future = task.get_future();
        
        	std::thread t(TaskWorker, std::move(task));
        	int64 sum = future.get();
        	t.join()
        }
        ```
        
        task : 일감 개념
        
        결론
        
        1. async : 원하는 함수를 비동기적으로 실행
        2. promise : 결과물을 promise를 통해 future로 받아줌
        3. packagaed_task : 원하는 함수의 실행 결과를 packaged_task를 통해 future로 받아줌
        
        비동기 ≠ 멀티쓰레드
        
        rvalue?
        
    - 캐시와 파이프라인
        
        temporal locality
        
        spatial locality
        
        멀티쓰레드 프로그래밍에서 가시성, 코드 재배치 문제
        
        이게 싱글쓰레드에서는 저 stage를 바꿔줘도 크게 티가 안났는데 멀티쓰레드가 되면서 티나게 되는거임
        
        ![Untitled](/assets/MT/Untitled%202.png)
        
    - 메모리 모델
        
         atomic 연산(cpu가 한번에 처리할 수 있는 연산)에 한해, 모든 쓰레드가 동일 객체에 대해서 동일한 수정 순서를 관찰 (is_lock_free로 cpu가 한번에 처리할 수 있는지 확인)
        
        ![Untitled](/assets/MT/Untitled%203.png)
        
        ```cpp
        {
        	//위험
        	//bool prev = flag;
        	//flag = true;
        
        	bool prev = flag.exchange(true);
        }
        
        //cas 조건부 수정
        {
        	bool expected = false;
        	bool desired = true;
        	flag.compare_exchange_strong(expected,desired, 메모리 정책);	
        }
        
        {
        	// Spurious Failure
        	// 하드웨어 적인 이유로 flag == expected라더라도 false가 뜰 수 있음
        	// 원래 이게 정상, strong은 따라서 부하가 좀 더 많이 감
        	// 그래서 while이랑 같이 씀
        	flag.compare_exchange_weak();
        }
        ```
        
        - **메모리 정책**
            
            seq-cst(default) (가장 엄격 = 컴파일러 최적화 여지 적음 = 직관적)
            
            가시성 문제 바로 해결! 코드 재배치 해결!
            
            amd는 힘들고, intel이나 arm은 별 부하 없음
            
            acq-rel (consume,acquire,release,acq_rel)
            
            짝을 맞춰줌 : release 이전 명령들이 이후 명령으로 재배치 되는것을 막아줌(절취선)
            
            그리고 acquire로 같은 변수를 읽는 쓰레드가 있다면 가시성 보장 (위로 절취선)
            
            fence라는 기능도 있음 (절취선 예)
            
            relaxed (자유롭다 = 컴파일러 최적화 여지 많음 = 직관적이지 않음)
            
            동일 객체 동일 관찰 순서만 보장
            
        - TLS
            
            나만의 전역 메모리 ( 큰 덩어리를 가지고 와서 작업 ) ( 찌개 그릇에 옮겨담는 느낌)
            
            ```cpp
            thread_local int32 LThreadId = 0;
            ```
            
    - lock-based stack/queue
        
        안전하게 하려고 front 보고 pop하는 것
        
        std::move란?
        
    - todo: lock-free stack/queue
        
        popCount
        
        smart pointer (사용/구현) (reference 사용)
        
    - dev
        
        r-w (X)
        
        w-w (O)
        
        w-r (O)