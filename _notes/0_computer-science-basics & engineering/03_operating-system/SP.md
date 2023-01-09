---
title: "System Programming"
---

# 시스템 프로그래밍 중간~

- **Signals**
    - Signal 1
        - Process groups and sessions
            
            ![process belongs to group, session : collection of groups](/assets/SP/Untitled.png)
            
            process belongs to group, session : collection of groups
            
            - process groups
                - getpgrp, getpgid
                    
                    leader: group id == pid
                    
                - int setpgid(pid_t pid, pid_t pgid)
                    
                    set process group id to pgid whose process id == pid
                    
                    - if pid == pgid
                    group leader
                    - if pid == 0
                    caller pid
                    - if pgid == 0
                    process group id ⇒ pid
                    - 
                
            - sessions
                - pid_t setsid(void)
                    - if calling process ≠ process group leader ⇒ create new session
                        - the process ⇒ session leader of new session, process group leader of new process group, no controlling terminal
            
            ![example of process groups & sessions](/assets/SP/Untitled%201.png)
            
            example of process groups & sessions
            
        - concepts
            
            Background job → problem occurs
            solution: Exceptional control flow! (**signal**)
            
            - sent form kernel to a destination process
            - information : its id & the fact that it arrived
            - SIG~
            
            ![Untitled](/assets/SP/Untitled%202.png)
            
            ![Untitled](/assets/SP/Untitled%203.png)
            
            - 
        - Sending a signal
            - int kill(pid_t pid, int signo)
                - pid > 0
                - pid == 0 : sent to all processes whose pgid == pgid of sender
                - pid < -1 : sent to every process in the process group whose id is -pid
                - int raise(int signo) == kill(getpid(),signo)
            - unsigned int alarm(unsigned int seconds)
                - timer expires, sigalrm signal generated
                - one alarm clock per process
                
            - int pause(void)
                - suspends the calling process
            
            ![Untitled](/assets/SP/Untitled%204.png)
            
            ![Untitled](/assets/SP/Untitled%205.png)
            
        - Receiving a signal
            
            A destination process receives a signal when it is forced by the kernel to react in some way to the delivery of the signal:
            
            - **ignore the signal**
            - **terminate the process**
            - **catch the signal by signal handler**
            
            ![Untitled](/assets/SP/Untitled%206.png)
            
        - Pending and Blocked signals
            - A signal is pending if sent but not yet received
                - Signals are not queued : if k in pending, after k same type k signal discarded
            - A process can block the receipt of certain signals
                - delivered but not received
            - A pending signal is received at most once
            
            Kernel maintains pending and blocked bit vectors in each process
            
            - pending: set bit k when delivered, clear bit k when received
            - blocked : sigprocmask
    - Signal 2
        - handler
            
            Signal default action
            
            - process terminates
            - process stops until restarted
            - ignores signal
            
            handler_t *signal(int signum, handler_t *handler) (address of a user-level signal handler)
            
            installing the handler = catching or handling the signal
            
            handlers can be interrupted by other handlers
            
            ![Untitled](/assets/SP/Untitled%207.png)
            
        - block
            
            Implicit : Kernel blocks any pending signals of type currently being handled.
            
            explicit : signal mask(can modify with sigprocmask())
            
            - int sigprocmask(int how, cons sigset_t *restrict set, sigset_t *restrict oset)
                
                how : SIG_BLOCK, SIG_UNBLOCK, SIG_SETMASK
                
                ![Untitled](/assets/SP/Untitled%208.png)
                
            
        - race condition 문제
            
            sigprocmask를 통해 race condition 해결 가능 but while문 확인해야함, pause나 sleep써도 문제있음 (signal wait)
            
            더 나은 방법 : sigsuspend
            
            int sigsuspend(const sigset_t *sigmask)
            
            ==
            
            sigprocmask(SIG_BLOCK, &mask, &prev); 
            
            pause(); 
            
            sigprocmask(SIG_SETMASK,&prev,NULL);
            
        - nonlocal jump
            
            useful for error recovery and signal handling
            
            - int setjmp(jmp_buf j)
                - must be called before longjmp
                - called once, returns one or more times
                - implementation: store the current **register context, stack pointer and PC value in jmp_buf**
                - return 0
            - void longjmp(jmp_buf j, int i)
                - return from the setjmp
                - return i
                - called once, but never returns
                - implementation: restore register context(stack pointer, base pointer, PC value)
            - limitations
                
                함수가 끝나기 전에 실행이 되어야함
                
                ![Untitled](/assets/SP/Untitled%209.png)
                
                그리고 handler에서 사용할 경우 handler 같은 경우 한번 실행되고 그 처리한 signal에 대해서 blocking bit를 활성화함 그리고 다시 unblock전에 longjmp로 돌아감 → handler 안 nonlocal jump를 구현할 경우 sigsetjmp&siglongjmp로 구현해야함.
                
                ![Untitled](/assets/SP/Untitled%2010.png)
                
- **Virtual Memory**
    - Virtual Memory 1
        
        Virtual memory : an array of N contiguous bytes stored on disk
        
        ![Untitled](/assets/SP/Untitled%2011.png)
        
        page table: an array of page table entries that maps virtual pages to physical pages
        
        page hit: reference to VM word that is in physical memory
        
        page fault: reference to VM word that is not in physical memory
        
        demand paging: waiting until the miss to copy the page to DRAM
        
        ![Untitled](/assets/SP/Untitled%2012.png)
        
        working set: active virtual pages
        
        thrashing: performance meltdown where pages are swapped(copied) in and out continuously
        
        VM as a tool for memory management : each process has its own virtual address space
        
        ![Untitled](/assets/SP/Untitled%2013.png)
        
        VM as a tool for memory protection : permission bits
        
        ![Untitled](/assets/SP/Untitled%2014.png)
        
        ![Untitled](/assets/SP/Untitled%2015.png)
        
        ![Untitled](/assets/SP/Untitled%2016.png)
        
        ![Untitled](/assets/SP/Untitled%2017.png)
        
        efficient only because of locality
        
    - Virtual Memory 2
        - TLB
            
            ![Untitled](/assets/SP/Untitled%2018.png)
            
            PTE requires a small L1 delay → TLB : Translation Lookaside Buffer
            
            - hardware cache in MMU
            - maps virtual page numbers to physical page numbers
            - contains complete page table entries for small number of pages
            
            ![Untitled](/assets/SP/Untitled%2019.png)
            
            ![Untitled](/assets/SP/Untitled%2020.png)
            
            → 계속 참조했던 페이지 계속 참조하기 때문
            
            ![Untitled](/assets/SP/Untitled%2021.png)
            
            ![Untitled](/assets/SP/Untitled%2022.png)
            
            ![Untitled](/assets/SP/Untitled%2023.png)
            
            tlb miss but page table hit
            
            ![Untitled](/assets/SP/Untitled%2024.png)
            
        
        linux organizes vm as collection of areas
        
        ![Untitled](/assets/SP/Untitled%2025.png)
        
        shared objects
        
        ![Untitled](/assets/SP/Untitled%2026.png)
        
        private cow objects
        
        ![Untitled](/assets/SP/Untitled%2027.png)
        
        - memory mapping
            
            void *mmap(void *start, int len, int prot, int flags, int fd, int offset)
            
            map len bytes starting at offset of the file specified by file description fd, preferably at address start
            
            ![Untitled](/assets/SP/Untitled%2028.png)
            
- **Thread**
    - Thread 1
        - process
            
            instance of a running program
            
            context of process: text, global user variables, values stored in process, contents of user and kernel stack
            
            ![Untitled](/assets/SP/Untitled%2029.png)
            
        - thread
            
            a single execution sequence that represents a separately schedulable task
            
            own: program counter, stack space, register set, priority
            
            share: memory space, code section, OS resources(open files, signals)
            
            TCB holds two types of per-thread information:
            
            - The state of the computation being performed by the thread
            - Metadata about the thread that is used to manage the thread
            
            process = process context + code,data,stack = thread + code,data and kernel context
            
            ![Untitled](/assets/SP/Untitled%2030.png)
            
            ![Untitled](/assets/SP/Untitled%2031.png)
            
        
        process and threads are similar in...
        
        each has own control flow
        
        run concurrently with others
        
        context switched
        
        different in..
        
        thread: share all code and data(except local stacks) ↔ process do not
        
        thread is less expensive
        
    - Thread 2
        
        kernel thread
        
        user thread
        
        pthread
        
        1-1,1-n,n-1,n-m
        
        ![Untitled](/assets/SP/Untitled%2032.png)
        
        every thread has a thread id
        
        - functions
            
            int pthread_create(pthread_t *restrict tidp, const pthread_attr_t restrict attr, void* (*start_rtn) (void *), void *restrict arg)
            
            void pthread_exit(void *rval_ptr)
            
            int pthread_join(pthread_t thread, void **rval_ptr)
            
        
        ![Untitled](/assets/SP/Untitled%2033.png)
        
        ![Untitled](/assets/SP/Untitled%2034.png)
        
        ![Untitled](/assets/SP/Untitled%2035.png)
        
        ![Untitled](/assets/SP/Untitled%2036.png)
        
        ![Untitled](/assets/SP/Untitled%2037.png)
        
        ![Untitled](/assets/SP/Untitled%2038.png)
        
        ![Untitled](/assets/SP/Untitled%2039.png)
        
        race conditions
        
        ![Untitled](/assets/SP/Untitled%2040.png)
        
        2,2,2,3,5,4,6...
        
- **Synchronization**
    
    ![Untitled](/assets/SP/Untitled%2041.png)
    
    Synchronizing access to shared variables
    
    - pthread_mutex_init
    - pthread_mutex_[un]lock
    
    A variable x is shared if and only if multiple threads reference some instance of x
    
    conceptual ≠ operation model
    
    sharing global variable, static:
    
    Mapping variable instances to memory:
    
    - global variables, local static variable
        - virtual memory contains exactly one instance of any global/ local static variable
    - local variables
        - each thread stack contains one instance of each local variable
        
        ![Untitled](/assets/SP/Untitled%2042.png)
        
        ![Untitled](/assets/SP/Untitled%2043.png)
        
        i and myid are not shared
        
        ![Untitled](/assets/SP/Untitled%2044.png)
        
        ![Untitled](/assets/SP/Untitled%2045.png)
        
        How to guarantee a safe trajectory? : synchronize the execution of the threads : mutex, semaphores
        
        int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr)
        
        int pthread_mutex_destroy(pthread_mutex_t *mutex)
        
        int pthread_mutex_lock(pthread_mutex_t *mutex)
        
        Semaphore : non-negative global integer synchronization variable, manipulated by P and V operations
        
        P(s) : s- - V(s) : s+ + (invariant : s≥0)
        
        int sem_init(sem_t *s, 0, unsigned int val) (s = val)
        
        void P(sem_t *s)
        
        binary semaphore == mutex
        
- **Inter Process Communication**
    - asynchronous VS synchronous
    - unidirectional VS bidirectional
    - related process VS unrelated processes
    - network communication
    - Means of IPC
        - characteristics
            
            ![Untitled](/assets/SP/Untitled%2046.png)
            
            ![Untitled](/assets/SP/Untitled%2047.png)
            
            ![Untitled](/assets/SP/Untitled%2048.png)
            
            ![Untitled](/assets/SP/Untitled%2049.png)
            
            ![Untitled](/assets/SP/Untitled%2050.png)
            
            ![Untitled](/assets/SP/Untitled%2051.png)
            
            ![Untitled](/assets/SP/Untitled%2052.png)
            
            ![Untitled](/assets/SP/Untitled%2053.png)
            
        - pipes
            - for transmitting data between related processes
            - open() / automatic blocking when full or empty
            - stdout → | → stdin
            - int pipe(int fd[2])
                - two file descriptors are returned through the fd argument:
                    - fd[0] can be used to read from the pipe
                    - **fd[1] can be used to write to the pipe**
            - FILE *popen(const char *cmd, const char *type);*
            
            ![Untitled](/assets/SP/Untitled%2054.png)
            
        - fifos
            
            way for two unrelated process to share a pipe
            
            int mkfifo(const char *path, mode_t mode)
            
            named pipe
            
            tee(1)
            
            ![Untitled](/assets/SP/Untitled%2055.png)
            
        
        System V IPC
        
        each IPC structure has a nonnegative integer
        
        when creating an IPC structure, a key must be specified
        
        id = xxxget(key,...) // xxx = msg,shm,sem
        
        get, ctl, op
        
        ipc_perm
        
        cat input | cat > output
        
        ![Untitled](/assets/SP/Untitled%2056.png)
        
        - message queue
            
            send and receive amount of data called messages(classified by type)
            
            - int msgget(key_t key, int flag)
            - int msgctl(int msqid, int cmd, struct msqid_ds *buf)
            - int msgsnd(int msqid, const void *ptr, size_t nbytes, int flag)
            - int msgrcv(int msqid, void *ptr, size_t nbytes, long type, int flag)
                - type == 0 : first message on the queue
                - type > 0 : mtype
                - type < 0: lowest value ≤ |type|
            
            ![Untitled](/assets/SP/Untitled%2057.png)
            
        - shared memory
            
            allows two or more processes to share a given region of memory
            
            - int shmget(key_t key, int size, int flag) returns shared memory id if ok
            - int shmctl(int shmid, int cmd, struct shmid_ds *buf) returns 0 if OK
            - void *shmat (int shmid, void *addr, int flag)
                - addr == 0: at the first address selected by the kernel(recommended)
                
                ![Untitled](/assets/SP/Untitled%2058.png)
                
            - void shmdt(void * addr)
        
        ![Untitled](/assets/SP/Untitled%2059.png)
        
        - semaphores
            
            process synchronization and resource management
            
            - test semaphore that controls the resource
            - if value > 0, value — , grant use
            - if value == 0, sleep until value >0
            - release resource, value ++
            
            step 1,2 must be an atomic operation
            
            semget,semop,semctl(semid,0,IPC_RMID,0)
            
            int semget(key_t key, int nsems, int flag) semaphore ID if Ok, -1 on error
            
            int semctl(int semid, int semnum, int cmd, arg)
            
            int semop(int semid, struct sembuf semoparray[], size_t nopes) returns 0
            
        - sockets
        
        ipcs : checking status of system V IPC
        
        ipcrm : delete
        
    

## Note

process~ipc

코딩 문제 X, 개념과 이해

코드레벨 이해 결과 예측

- signal
    
    process group id는 잘 안나올 것 같다
    
    개념을 잘 알아야하고 어떻게 동작하는 코드인지를 알아야함
    
    signal action
    
    signal number 외울필요 x
    
    signal 종류. function은 무슨 역할을 하는지
    
    pending.block
    
    mask 함수들
    
    race condition
    
    suspend
    
    해결방안
    
    setjmp longjmp
    
- virutal memory
    
    개념
    
    PE VMA 바꾸는 개념 설명
    
    layout
    
    mapping
    
    demand paging
    
    shared object
    
    cow
    
- thread
    
    ***process랑 무엇이 다른지 개념적으로 다 이해***
    
    thread 문제들
    
    tcb에는 무엇이 있는가
    
    process랑 함께 코딩 했을 때 결과
    
    race condition 어떨 때 생기고 해결법
    
- synchronization
    
    function 의 동작
    
    race condition이란 특징 해결하기 위한 방법
    
    thread를 이용했을 때 왜 race condition이 생기고 어떻게 그걸 해결할 수 있을까
    
    코드레벨에서 이해
    
    terminology 설명
    
- IPC
    
    특징들 pipe fifo shm mq socket socketpair
    
    각각의 장점들
    
    어떻게 쓸 수 있는지
    
    코드레벨 이해
    
    flag 제공
    
    어쩔 때 쓸 수 있고 쓸 수 없는지 어떻게 개념적으로 구현이 되는지
    
    예제 코드 응용
    
    포인트만 알면 설명할 수 있는 문제
    
    semaphore 동작원리는 알아야함