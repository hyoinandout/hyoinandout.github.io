# 메모리

- 스마트 포인터
    - 스마트 포인터의 필요성
        
        동일한 객체의 포인터를 가지고 있는데 한쪽에서 삭제를 해버린다면?
        
        생명 주기를 관리하는데 있어서 정책이 없기 때문
        
        누군가 참조하고 있을때는 삭제 안하게
        
        해결 방법 : refCount
        
        멀티쓰레드 환경에서 항상 문제
        
        이제 atomic으로 바꿔줘야함
        
        수동으로 하는 것은 힘들어 → 자동으로 해주는 스마트 포인터
        
    - 스마트 포인터
        - TSharedPtr
            
            refCount를 wrapping해서 들고있는다
            
            lockguard와 비슷한 컨셉
            
            쓰는법 : using MissleRef = TSharedPtr<Missile>
            
            항상 refCount 1이상이기 때문에 멀티쓰레드 환경 대응 가능
            
            참고)내부적으로, 잠시만 사용한다고 하면 스마트포인터의 ref 넘겨주면 됨
            
            한계)
            
            1. 이미 만들어진 class 대상으로 사용 불가 (외부 라이브러리 사용하는 경우)
            2. 순환 (싸이클) 문제
                
                a→b b→a 라서 메모리 해제 안됨 (데드락과 비슷)
                
                해결 방법 1: setTarget(nullptr)
                
                상속 문제에서도. → memory leak 사용량이 늘어난다.
                
        - unique
            
            쌩 포인터랑 같은데 복사하는 부분이 막혀있음
            
            ```cpp
            unique_ptr<Knight> k2 = make_unique<Knight>();
            unique_ptr<Knight> k3 = k2; // 에러
            
            unique_ptr<Knight> k3 = std::move(k2);//소유권 포기하고 넘겨라.
            ```
            
        - **shared & weak**
            
            아까 TSharedPtr 처럼 구현하면 그 상속받는 객체 안에 AddPtr, ReleasePtr과 같은 함수 구현해 줘야함 하지만 여기는 아님. 알아서 다 함
            
            코드를 추적해보면...
            
            ```cpp
            //shared_ptr
            element_typr _Ptr{nullptr}; // TSharedPtr에서 _ptr와 같은 부분
            _Ref_count_base* _Rep{nullptr}; //RefCountable 에서 _refCount와 같은 부분
            // 결국 TSharedPtr와의 차이는 상속받지 않고 한 클래스 내에서 모두 구현했다는 부분
            
            //[Knight][RefCountingBlock]
            //[T*][RefCountingBlock*]
            shared_ptr<Knight> spr(new Knight());
            
            //or
            //[Knight | RefCountingBlock(uses,weak)]
            shared_ptr<Knight> spr = make_shared<Knight>();
            
            //[T*][RefCountingBlock*]
            shared_ptr<Knight> spr2 = spr;
            
            //RefCountBlock(useCount(shared),weakCount)
            weak_ptr<Knight> wpr = spr;
            
            bool expired = wpr.expired();
            
            shared_ptr<Knight> spr2 = wpr.lock();
            ```
            
            weak_ptr은 반쪽짜리 포인터, 왜냐하면 이제 ptr 구조에서 
            [Knight | RefCountingBlock(uses,weak)] 인데, Knight 참조 사라져서 메모리 없어져도 bool값으로 확인해야됨. 반면에 같은 경우에서 uses는 바로 nullptr로 리턴. RefCountingBlock이 사라지는 건 usecount =0 && weakcount = 0 될 때. 그전까진 계속 살아있음
            
            uses = 0 되면 실제 가리키고 있는 거 ([T*])날아감
            
            weak_ptr로 순환 문제 해결 가능한데 왜냐하면 상대방의 수명 주기에 영향 주지 않기 때문
            
- 메모리
    - BaseAllocator
        
        요즘은 그냥 new,delete로 하면 되긴 하는데 알고 하는거랑 모르고 하는거랑 차이는 있으니...
        
        일단 풀링하는게 좋긴 하다 왜냐하면 커널단으로 가는 컨텍스트 스위칭의 빈도를 줄일 수 있고
        
        그리고 stl쓸 때 메모리를 잘 할당 못해서 낭비될 수도 있는데 풀링으로 해결 가능
        
        그래서 메모리를 쓰는데, 이제 placement new 와 같은 새로운 개념이 나온다
        
    - StompAllocator
        
        버그 잡기(메모리 leak)
        
        - 메모리 오염 예시
            1. 해제한 메모리를 다시 참조할 때(use-after-free)
                
                nullptr로 밀어주면 되긴 하지만, 복잡한 상황들이 많다
                
                스마트포인터로 해결 가능
                
            2. for문으로 stl접근하는데 for문안에서 clear해줬을때
            3. casting 문제(overflow)
        - 가상 메모리 기본
            
            ![Untitled](/assets/Memory/Untitled.png)
            
            유저레벨(프로그램들)
            
            ————
            
            커널레벨(OS code)
            
            프로그램들끼리 메모리 공유 X
            
            2GB [                                      ]
            
            2GB / 4KB [r][w][rw][X][][][][][][][][][][]
            
            페이지가 존재하는 이유: 만약 1B당 접근 가능/불가능 판단한다면 추가로 2GB의 메모리가 필요하게 될것
            
            ---
            
        
        VirtualAlloc | VirtualFree
        
        new delete와의 차이: 위 함수들은 해제된 메모리 참조할 시 crash 내준다
        
        new | delete는 메모리 유동적으로 관리 (메모리 풀 관리한다는 것, 운영체제까지 가지 않는다는것)
        
        단점 : 작은 바이트 할당하고 싶어도 페이지 단위로 할당하게 됨 
        
        BaseAllocator로는 memory leak 못잡는다
        
        오버플로우 잡으려면 약간의 조정이 필요 : [                     [      ]]
        
    - STLAllocator
        
        
    - 메모리 풀링
        
        재활용 개념
        
        커널레벨 까지 가서 하는 컨텍스트 스위칭 & 메모리 파편화 때문에
        
        쉬운거 : 동일한 데이터 > 유동적 데이터
        
        메모리 헤더
        
        크기가 비슷하면 같은 풀에 , object 풀링도 할 수 있긴함
        
        xalloc 안쓰는 이유: 거긴 결국 메모리 해제함
        
        아쉬운점: Lock 걸어서 접근하는 부분, queue가 동적 배열로 구현되어 있음
        
        그래서 lock - free 로 사용하는 법도 있음 → todo
        
- type conversion
    
    todo:  typecast
    
    dynamic_cast 느리다.
    

shared_from_this?