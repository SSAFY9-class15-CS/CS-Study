## Synchronization

- **Process Synchronization** : 멀티 프로세스 환경에서 프로세스들이 **자원을 공유**하며 협력하는 경우 프로세스들 간의 **실행 순서를 조정**하는 것 ⇒ 공유 자원의 **일관성**을 유지한다!
- 공유 자원을 사용하는 상황 중 context switching이 일어나면 공유 자원 값이 뒤섞일 수 있기 때문에 동기화가 등장
- 쓰레드, 프로세스 모두 동기화를 필요로 한다.


> 💡 **scheduling은 언제 어디서든 발생할 수 있다는 전제를 생각하자!**


## Synchronization Problem

- **race condition** : 여러 개의 프로세스/쓰레드가 자원을 공유할 때 실행할 때마다 결과가 달라지는 경우를 의미 ⇒ **비정상적이다, 예측과 다른 결과를 낳을 수 있다.**
- 클래식한 동기화 예제인 Bank Account 예제를 살펴보자.
    - Context Switching 모습
        
        ![Untitled (10)](https://user-images.githubusercontent.com/81673820/233978900-1edf2f0c-0d8b-497a-b60b-d3320e32297f.png)
        
    - withdraw 메소드
        
        ```c
        int withdraw(account, amount){ // 현금 인출 메소드, account:계좌, amount:인출액
        	balance = get_balance(acoount); // 1. balance라는 변수에 계좌 잔액 저장
        	balance = balance - amount;     // 2. balance를 인출액만큼 빼줌
        	put_balance(account, balance);  // 3. 인출액만큼 뺀 balance를 계좌의 잔액으로 갱신
        	return balance;                 // 4. 갱신된 잔액 return
        }
        ```
        
    - A, B라는 사람(프로세스)이 은행에가서 현금을 인출하는 상황을 상상해보자. 서로 다른 ATM기에서 먼저 A라는 사람이 현금을 인출하고 B라는 사람이 거의 동시에 현금을 인출한다. 이때 은행 전산 시스템은 현금 인출 메소드(withdraw)를 통해 인출 프로세스를 진행하는데 위 그림과 같이 A라는 사람의 작업을 수행하던 도중 context switching이 발생하여 B의 작업을 수행하게 된다면 A의 잔액이 엉망이 되는 **예상치 못한 결과가 발생**하게 된다.
    - 그 이유는 balance라는 자원을 A, B 두 프로세스가 **공유**하기 때문이다. A의 작업을 수행 도중 context switching이 일어나 B가 공유 자원인 balance의 값을 수정하고 다시 context switching이 되어 cpu가 B가 수정한  balance 값을 지닌 채 A가 이전까지 수행한 이후 코드 부터 작업을 수행하기 때문이다.
    
- 동기화는 프로세스 간 공유 자원이 존재할 때 필요하다.
    - ex) buffer, queue, list
- count++ 이라는 한 줄 짜리 코드도 여러 개의 machine instruction으로 구성되고 이 사이에도 context switching이 일어날 수 있다.
    - ex) load, add, store…


> 💡 Java의 `SimpleDateFormat` 클래스 thread-unsafe해서 반드시 동기화가 필요하다.


## Critical Sections(임계 영역)

- 프로세스/쓰레드 간 공유하고 있는 메모리 영역이나 파일에 접근하는 프로그램 영역을 의미하고 하나의 프로세스/쓰레드만이 진입할 수 있다.
- 구성 요소
    - `entry` : critical section에 들어가기 직전 영역으로 critical section에 들어갈 수 있는지 여부 확인
    - `critical section` : 공유 자원을 사용하는 영역
    - `exit` : cirtical section에서 나온 직후 영역
    - `remainder` : critical section 이외의 영역

## Synchronization시 필요 조건

- **Mutual Exclusion(상호 배타적)**
    - **critical section은 하나의 프로세스/쓰레드**만 수행 중이어야 한다.
    - critical section을 수행하려는 다른 프로세스들은 entry에서 대기한다.
- Solutions들이 만족해야하는 조건
    1. **Mutual exclusion** : critical section에는 한 프로세스 또는 쓰레드만 존재해야한다.
    2. **Progress** : critical section 밖에 있는 프로세스가 다른 프로세스가 critical section에 들어가는 걸 막으면 안된다, critical section을 아무도 사용중이지 않다면 사용할 수 있어야 한다라는 의미와 동
        1. ex. 두 개의 프로세스가 critical section에 들어가는 속도가 다를 때 느린 속도에 빠른 속도의 프로세스가 맞춰야 하는 경우가 없어야 한다. 
    3. **Bounded waiting**(no starvation)
        1. critical section에 들어가기 위해 기다리는 프로세스는 **무조건 critical section에 들어갈 수 있어야 한다**.(우선순위가 계속 밀려서 starvation이 발생하면 안된다.)
    4. **Performance**
        1. synchronization에 너무 많은 시간을 낭비해선 안된다.
        2. bakery algorithm을 살펴보면 동기화를 위해 acquire, release 메소드에서 해야하는 일이 너무 많다.
- Solution 종류
    - **Locks**
        - SW(os)에서는 Mutex HW에서는 spinlock이 존재
    - **Semaphores**
    - **Monitors**
        - cirtical section을 지정해주면 컴파일러가 자동으로 동기화 시켜주는 것
    - **Messages**
        - 프로세스 P가 Q에 메세지를 보내고 프로세스 Q가 P로 부터 메세지를 받아야만 프로세스 Q의 코드들이 수행 ⇒ 순서가 발생

## Locks

- lock은 두 개의 operation을 제공하는 object로 특정 상태를 지닌다 .
- operation 종류
    - `acquire()`
        - lock 자원을 아무도 사용하지 않으면 lock을 획득하고 lock 자원의 상태를 사용 중(used)이라고 표시
        - lock 자원을 누군가 사용 중이라면 lock 자원이 free가 될 때까지 대기
        
        
        > 💡 **spin lock** : lock 자원이 free인지 계속해서 프로세스가 직접 확인(busy wait)
        **blocking lock(mutex)** : os가 lock 자원이 free가 되면 프로세스를 깨워줌
        
        
        
    - `release()`
        - lock을 다 쓰고나서 lock을 놓아주고 lock 자원의 상태를 free라고 표시
- 사용 방법
    - lock 처음 생성 시 상태를 fee로 초기화
    - critical section 진입 전에는 acquire, 나갈 때는 release호출
    - lock을 다른 프로세스가 사용 중이라면 acquire하기 위해선 대기해야한다.
- 동작 과정
    
    ![Untitled (10-2)](https://user-images.githubusercontent.com/81673820/233978922-c9868bb5-d91a-4307-819b-5b85100dd72b.png)
    
    - critical section에 한 프로세스만 진입하게 된다.
    - S1, S2, S3를 한 덩어리로 묶어 **transaction**이라고 부른다.
- 유의할 점
    - acquire 자체도 여러 machine instruction으로 구성되므로 **그 사이에 timer interrupt가 일어나 context switching이 일어날 수 있다**. ⇒ atomic(원자성)하게 acquire 메소드를 구현해야 한다!
- 구현 방법
    - software적 구현 방법
    - hardware적 구현 방법
    - timer-interrupt를 활성, 비활성화

## Atomic Hardware Instructions

- lock을 하드웨어 적(cpu level)으로 구현한 방법
    
    ```c
    void struck lock {
    	int value = 0;
    }
    
    void acquire(struct lock *I) {
    	while(TestAndSet(&I- > value));
    }
    
    void release(struck lock *I) {
    	I- > value = 0;
    }
    ```
    
    - test-and-set 사용한 예
- **atomic instruction**을 사용
    - read-modify-write를 하나의 machine cycle로 수행(by OS)
    - test-and-set, compare-and-swap등이 존재
        
        ```c
        int TestAndSet(int *v) { //test-and-set 구현 모습
        	int rv = *v;
        	*v = 1;
        	return rv;
        }
        ```
        
- 단점
    - **spinning lock**은 ****busy waiting(lock을 획득할 수 있는 지 계속 확인)하기 때문에 cpu 낭비가 심하다
        - **busy waiting** : 맛집 앞에서 계속 내 순번이 올 때 까지 대기 하는 것 과 유사하여 시간(CPU)이 낭비된다
        - lock, semaphore, monitor 등은 테이블링 앱에 번호를 저장하고 내 순번 때 연락이오면 찾아가는 것과 유사하여 시간(CPU)이 낭비되지 않는다.
    - critical section이 길 수록 낭비가 더욱 심해진다.
- 위 단점들에 의해 lock은 커널 안에서 아주 짧은 critical section 구간에 진입할 때만 사용한다.(일반 어플리케이션에서는 사용하지 않는다.)

## Disabling Interrupts

- 아예 context switching이 일어나지 않도록 timer interrupt를 막아버리는 방법
- 주로 커널 안에서만 사용(일반 어플리케이션에서는 사용하지 않는다.)
- 멀티 프로세서 환경에서는 작동하지 않는다.
    - 프로세서가 여러 개면 하나의 프로세서에서만 timer interrupt가 꺼지기 때문에

## Synchronization Types

- **Mutual Exclusion**
    - critical section에는 오직 하나의 쓰레드만이 진입할 수 있다.
- **Waiting for events**
    - waiting
    - producer/consumer
    - pipeline
    - defer work with background thread

## Higher level Synchronization

- 등장배경
    - 앞서 배운 Spinlock, Disabling interrupts는 cpu의 performance를 떨어뜨린다.
    - 일반적으로 user-level에서 사용하기 보다는 kernel-level에서 사용(아주 짧은 순간 동기화가 필요할 때)
    - 이러한 이유 때문에 더 성능이 좋은 synchronization 기법이 필요해 고수준의 기법들이 등
- 종류
    - Mutex
    - Semaphore
    - Monitor

## Mutex

- OS가 제공하는 Locking Object(**객체**)
    - busy waiting을 피하기 위해 등장
    - **state(상태)**를 지님
        - 0 : 아무도 사용 중이지 않는 경우
        - 1 : 이미 다른 프로세스에 의해 사용 중인 경우
- **Operations(동작)**
    - `mutex_lock` : lock을 획득, 만약 공유자원이 이미 사용중이라면 free가 될때 까지 cpu를 스스로 반납(**block**)하고 대기
    - `mutex_unlock` : lock을 반납, spin lock과 다르게 반납 시 기다리는 다른 프로세를 깨워줌(**wake up**)

> 💡 객체는 **상태**와 그 상태를 조작하는 **동작(**행위)으로 구성된다.


- kernel operations for Mutex
    - `block()` : 다른 프로세스가 자신을 깨워줄 때 까지 cpu를 반납하고 대기
    - `wakeup(int pid)` : pid를 인자로 받아 waiting queue에 있는 프로세스를 ready queue로 옮겨준다(깨워줌)
    
- Mutex 내부 구현 모습(슈도 코드)
    
    ```c
    typedef struct{
    	int lock; // lock == 0 : 아무도 사용 중이지 않은 경우, lock == 1 : 누군가 이미 사용중인 경우
    	Queue wq; // 프로세스들이 기다릴 waiting queue
    } MUTEX;
    
    mutex_lock(MUTEX *m){ // lock 자원을 획득
    
    	if(m->lock == 0){ // 아무도 lock을 사용 중이지 않다면
    		m->lock = 1;
    	} else{  // 이미 누군가 lock을 사용 중이라면
    		m->wq.enqueue(getpid());
    		block();
    	}
    	return;
    }
    
    mutex_unlock(MUTEX *m){ // lock자원을 반납
    
    	if(!m->wq.isEmpty()){ // lock을 얻기 위해 기다리는 프로세스가 존재한다면
    		wakeup(m->wq.dequeue());
    	}
    	m->lock = 0; // lock 반납
    	return;
    }
    
    ```
    
    - mutex_lock, mutex_unlock 안에서 lock의 값을 변경하는 도중 context switching이 일어나지 않도록 내부적으로 **Test and Set** or **Interrupt disable**을 사용한다.
    
> 💡 멀티 코어 환경에서 critical section에서의 작업이 context swtiching 보다 더 빨리 끝난다면 spin lock이 mutex보다 더 좋을 수 있다.(싱글 코어에서는 무조건 mutex가 더  좋다)


## Semaphores

- Mutex 보다 풍부한 표현(기능)을 제공하는 synchronization 기법
    - 공유 자원이 **여러 개**인 경우에도 처리가 가능하다.
    - ex. 공중화장실(공유하는 영역)에 여러 사람들이 사용하는 변기(공유 자원)가 여러 개 존재
- Dijkstra가 발명한 기법
- Mutex와 동일하게 특정한 **상태(state)**와 **동작(method, operation)**을 지니는 **객체(object)**이다.
    - state는 user program에 의해 직접 접근하여 수정될 수 없다. ⇒ 오로지 semaphore가 제공하는 operation을 사용해서 수정 해야 한다.
    - Operation
        - `S_wait()`
            - value를 감소 시킴
            - 만약 감소 시킨 값이 음수가 될 경우(사용가능한 자원이 없는 경우) value가 0이상이 될 때 까지 wait
        - `S_signal()`
            - value를 증가 시킴
            - 만약 증가 시킨 값이 여전히 음수일 경우(기다리는 프로세스가 존재) 대기중인 프로세스를 깨움
- 프로세스/쓰레드의 **실행 순서**를 결정할 때 사용하기도 한다.

- Semaphore 내부 구현 모습(슈도 코드)
    
    ```c
    typedef struc{
    	int value; // 공유 자원의 개수로 초기화 되이있다.(공유자원이 3개라면 3으로 초기화)
    	struct process *Q;
    }
    
    void S_wait(semaphore *S){ // 세마포어 획득
    	S->vlaue--; // 세마포어 값 감소
    	if(S->value < 0){  // 이미 다른 프로세스들이 사용 해서 현재 사용할 수 있는 공유 자원이 없다면
    		add this process to S->Q  // 현재 프로세스를 waiting queue에 추가
    		block();  // cpu를 반납하고 대기
    	}
    }
    
    void S_signal(semaphore *S){ // 세마포어 반납
    	S->value++; // 세마포어 값 증가
    	if(S->value <= 0){ // 세마포어를 사용하기 위해 대기 중인 프로세스가 존재한다면 이를 깨워준다.
    		remove a process P from S->Q;
    		wakeup(P); // waiting queue에 있는 프로세스를 깨워줌
    	}
    }
    ```
    
    - S_wait, S_signal 안에 명령을 수행 중 context switching이 일어나지 않도록 내부적으로 **Atmoic Instruction** or **Interrupt disable**을 사용한다.
    - waiting queue의 size는 세마포어를 획득하기 위해, 즉 공유 자원을 사용하기 위해 기다리는 프로세스/쓰레드의 수이다.

## Types of Semaphores

- **Binary Semaphore**
    - mutex와 유사(단지 상태를 어떻게 규정하느냐만 차이가 존재)
    - 자원이 하나 뿐인 경우 사용
    - 초기값을 1로 설정
- **Counting Semaphore**
    - 체육시간에 농구공에 비유
    - 사용할 수 있는 자원이 여러 개인 경우
    - 초기값을 N으로 설정(N : 자원의 개수)
- **Mutex와 Binary Semaphore의 차이점**
    1. mutex는 lock을 가진 프로세스만이 lock을 해제할 수 있지만 semaphore는 그렇지 않다.
    2. mutex는 **priority inheritance** 속성을 지니지만 세마포어는 그러한 속성이 존재하지 않는다.(세마포어를 지닌 여러 프로세스 중 누가 signal을 날릴 지 알 수 없기 때문에)


> 💡 mutal exclusion만 필요하다면 Mutex, 작업 간의 실행 순서를 지정해줘야 한다면  Binary Semaphore 사용


## Bounded Buffer Problem

- Producer/Consumer Problem이라고도 불린다.
    - producer는 자원(데이터)을 버퍼에 넣는 역할
    - consumer는 자원 버퍼에서 빼는 역할
- 제한된 크기를 갖는 버퍼를 producer와 consumer가 공유 ⇒ 동기화 필요
- producer와 consumer가 데이터를 넣고 빼는 속도가 다르다 ⇒ 동기화 필요
- **동기화 하기 전 모습(슈도 코드)**
    
    ```c
    struct item buffer[N]; // 데이터를 담거나 뺄 버퍼
    int in, out; // in : 다음에 데이터를 넣을 index, out : 다음에 데이터를 뺄 index
    int count;   // count : 현재 버퍼에 들어있는 데이터의 개수
    
    void Producer(data){ // 데이터 생성
    	
    	while(count != N){ // buffer에 데이터가 꽉 찼는지 확인
    		buffer[in] = data;
    		in = (in+1) % N;
    		count++;
    	}
    }
    
    void Consumer(data){ // 데이터 조회
    	
    	while(count != 0){ // 버퍼에 데이터가 존재하는지 확인
    		data = buffer[out];
    		out = (out+1) % N;
    		count--;
    	}
    }
    
    ```
    
    - produer, consumer가 여러 개 있을 때 producer들 간 `in` 변수를, consumer들 간 `out` 변수를 **공유**하므로 **동기화**가 필요
    - while문을 사용하여 **busy waiting**하기 때문에 **cpu가 낭비됨**

- **세마포어를 사용하여 동기화 한 예제(슈도 코드)**
    
    ```c
    Semaphore mutex = 1; // binary semaphore, producer들, consumer들 간 in, out 변수의 동기화
    Semaphore empty = N; // counting semaphore, 데이터를 넣을 수 있는 빈 공간을 의미
    Semaphore full = 0;  // counting semaphore, 데이터가 채워져 뺄 수 있는 공간을 의미
    
    struct item buffer[N]; // 데이터를 담거나 뺄 버퍼
    int in, out; // in : 다음에 데이터를 넣을 index, out : 다음에 데이터를 뺄 index
    int count;   // count : 현재 들어있는 데이터의 개수
    
    void Producer(data){ // 데이터 생성
    	S_wait(&empty);    // buffer에 데이터를 채워 넣을 빈 공간이 존재하는지 확인
    	S_wait(&mutex);    // buffer를 사용할 수 있는 지 확인(producer들 간 동기화)
    	buffer[in] = data;
    	in = (in+1) % N;
    	count++;
    	S_signal(&mutex);  // buffer 사용 권한 반납
    	S_signal(&full);   // buffer에 데이터가 생성되었다고 알림
    }
    
    void Consumer(data){ // 데이터 조회
    	S_wait(&full);     // buffer에 데이터가 존재하는 지 확인
    	S_wait(&mutex);    // buffer를 사용할 수 있는 지 확인(reader들 간 동기화)
    	data = buffer[out];
    	out = (out+1) % N;
    	count--;
    	S_signal(&mutex);  // buffer 사용 권한 반납
    	S_signal(&empty);  // buffer에 데이터를 채워 넣을 빈 공간이 존재한다고 알림
    }
    
    ```
    
    - entry section에서 empty semaphore 또는 full semaphore 값이 0 이하일 때 특정 프로세스가 `S_wait`을 호출할 경우 내부적으로 sys call `block`을 호출하여 waiting 상태에 들어간다.
    - 작업 완료 시 exit section에서 `S_signal`을 통해 waiting 상태에 있는 프로세스를 깨워준다.
    

## Reader-Writers Problem

- 동기화를 하지 않을 경우 발생할 수 있는 문제점을 보여주는 대표적인 예제 중 하나로 여러 개의 프로세스/쓰레드들이 공유하는 버퍼에 데이터를 읽거(Reader)나 쓰기(Writer) 작업을 수행한다.
- 세마포어를 사용하여 Reader & Writer Problem을 동기화 한 예제
    
    ```c
    int readCount = 0;   // 현재 몇 명의 reader가 data를 읽는 지
    Semaphore readerSemaphore = 1;    // 여러 reader들 간 공유하는 데이터에 대한 동기화 수행
    Semaphore readWriteSemaphore = 1; // reader와 writer간 공유하는 데이터에 대한 동기화 수행
    
    void Writer(){
    	S_wait(&readWriteSemaphore);  // 데이터를 생성할 buffer에 접근할 권한을 획득, 이미 사용중이라면 waiting queue로 이동
    	...
    	Write Data
    	...
    	S_signal(&readWriteSemaphore); // 작업 완료 후 데이터가 담긴 buffer에 접근할 권한을 반납 후 waiting queue에서 대기중인 프로세스를 꺠워줌
    }
    
    void Reader(){
    	S_wait(&readerSemaphore); // reader들 간 공유하는 readCount 자원의 사용 권한을 획득하기 위해 readerSemaphore wait
    	readCount++:
    
    	if(readCount == 1){ // 만약 첫 번째 reader라면 데이터를 읽을 buffer에 접근할 권한을 획득, 첫 번째 reader가 아니라면 이미 얻어져 있기 때문에 넘어가도 된다.
    		S_wait(&readWriteSemaphore);
    	}
    
    	S_signal(&readerSemaphore); // readerSemaphore 반납
    	...
    	Read Data
    	...
    	S_wait(&readerSemaphore);  // readCount를 수정하기 위해 readerSemaphore wait
    
    	readCount--;
    
    	if(readCount == 0){ // 만약 모든 reader가 작업을 종료했다면 buffer에 접근할 권한을 반납 후 waiting queue에서 대기중인 writer를 꺠워줌
    		S_signal(&readWriteSemaphore);
    	}
    
    	S_signal(&readerSemaphore);  // readerSemaphore 반납
    }
    
    ```
    
    - writer는 데이터의 수정(create, update)이 일어나서 한 번에 하나의 프로세스/쓰레드만이 buffer에 접근 가능하다.
    - reader는 단지 데이터를 읽기(read)만 하기 때문에 변경이 일어나지 않아 한 번에 여러 프로세스/쓰레드가 접근 가능하다.
    - reading 하는 중에 writing이 불가능하고 writing 하는 중에 reading이 불가능해야 한다.

## Dining Philosophers Problem

- 철학자 : 프로세스
- 포크 : 컴퓨터 자원(파일, 메모리…)
- 철학자는 양쪽에 있는 포크를 집어야 식사가 가능한데 모든 철학자들이 동시에 왼쪽 혹은 오른쪽에 있는 포크를 집어들고 왼쪽 포크를 다른 철학자가 다 쓸 때 까지 기다린다면 영원히 기다리게 된다. ⇒ Dead Lock이라고 부른다.(Dead Lock free solution에 대해서는 추후 다룰 예정)
- Dead Lock Free Solution
    
    ![Untitled (12)](https://user-images.githubusercontent.com/81673820/233978936-9061582b-e2ce-4878-be7d-3a94ea0926c6.png)
    
    - 한 명만 오른 쪽 포크부터 집고 왼쪽 포크를 집게 하고 나머지는 왼쪽 포크부터 집도록 한다면 dead lock이 발생하지 않는다.

## Using Semaphores

- **장점**
    - mutex의 역할(bianry semaphore)과 counting semaphore 역할 모두 수행 가능하다.
    - 하지만 역할을 분명히 하기 위해서 binary semaphore를 사용하기보단 mutex를 사용하는 게 좋다.(counting semaphore와 구분하기 위해)
- **단점**
    - 프로그램 코드 여기저기에서 semaphore에 접근할 수 있다. (semaphore는 결국 객체이기 때문에 주소값만 알면 접근이 가능하다)
    - 사용이 복잡하다

## Condition Variables

- 프로세스가 mutex를 얻은 후 critical section에 들어와 작업을 수행 중 만족해야하는 특정 조건이 만족되지 않아 대개해야하는 상황이 발생 시, 다른 프로세스들이 critical sectino에 들어와 작업을 할 수 있도록 mutex를 반납하고 block되도록 한다.
- condition variable은 **waiting queue**라고 생각하면 편하다.
- operations
    - `wait()`
        - 자신이 가지고 있던 mutex lock을 반납
        - 누군가 깨워줄 때 까지 대기
        - 위 두 동작은 atomic 하게 동작한다.
    - `signal()`
        - waiting queue에서 대기하는 가장 앞에 있는(오래된) 프로세스를 깨워줌
        - 프로세스가 깨워났다고 해서 바로 mutex를 얻는 게 아니라 다시 mutex를 획득하기 위해 기다리게 된다.
    - `broadcast()`
        - waiting queue에서 대기하는 모든 프로세스들을 깨워줌

## CV Semantics

- **Mesa semantics(Signal and Continue)**
    - signal을 보내 waiting thread를 깨우면 waiting thread는 ready queue로 가게되고, caller thread는 여전히 critical section을 수행할 수도 있다.
    - waiting thread가 깨어나면 다른 프로세스들에 의해 기다리던 조건의 상태가 변할 수도 있다. ⇒ waiting thread가 자신이 기다리던 조건이 만족되었는지 **다시 check**해봐야 한다.
    - OS에서는 Mesa semantics를 사용한다.
    - Bounded Buffer Problem with Mesa semantics
        
        ![Untitled (13)](https://user-images.githubusercontent.com/81673820/233978938-76a99443-58ca-45af-91a2-180e9dd41675.png)
        
        - 조건을 다시 check해봐야하지만 그렇지 않고 바로 다음 코드를 수행하기 때문에 while문을 if로 바꾸게 되면 불안정한 상태에 빠질 수 있다.
        - Hoare semantcis일 경우 if로 해도 가능하다.
        
- **Hoare semantics(Signal and Wait)**
    - 매우 엄격
    - signal을 보낼  caller와 waiting thread를 바로 switiching한다.(waiting thread가 cpu를 바로 얻 ,caller는 block 상태 진입) ⇒ waiting thread가 signal로 깨어나면 바로 mutex를 얻을 수 있다.
    - 조건이 바뀔 가능성이 없다. ⇒ waiting thread가 **조건이 만족**됐다고 확신할 수 있다 .
    - **overhead**가 크고 구현하기 어렵다.

## Using Broadcast

- C언어의 동적할당 시 사용하는 `malloc`의 대략적인 동작 과정을 보여준다.
    
    ![Untitled (14)](https://user-images.githubusercontent.com/81673820/233978941-aaa45650-2fe4-4049-af08-04b805d477cc.png)
    
- 시그널을 보내는 프로세스가 waiting queue에서 대기 중인 프로세스 중 어떤 프로세스를 깨워야할 지 모르는 경우 `boradcast`를 사용하여 모든 프로세스들을 깨우고 각 프로세스들이 자신이 기다리던 조건을 만족하는지 **re check**하도록 한다.

## Monitor

- 프로그래밍 언어에서 제공하는 구조체(객체)
- 여러 프로세스들이 공유하는 공유 정보(변수)와 공유 정보에 접근하기 위해 사용되는 procedure(메소드, operations)로 구성된다.
- 공유자원에 접근하는 영역을 원래는 개발자가 synchronization을 해야하지만 monitor 사용 시 **컴파일러가 자동**으로 해준다.
- monitor는 내부적으로 **mutex**와 **condition variable**을 사용하여 synchronization을 수행한다.
- 구조
    
    ![Untitled (15)](https://user-images.githubusercontent.com/81673820/233978942-7f252ad0-f155-4264-bda4-7afdffcbd54e.png)
    
    1. monitor를 사용하기 위해 **mutex**를 획득해야 하므로 이미 누군가가 사용중이라면 **entry queue**에서 대기
    2. monitor안에 들어와 operation을 수행 중 특정 조건을 만족할 때 까지 기다려야 하는 상황이 온다면 가지고 있던 mutex를 반납하고 monitor안에 있는 **condtion variable(waiting queue)**에서 대기

> 💡 entry queue, waiting queue 안에서의 프로세스/쓰레드 간 순서는 스케쥴링 우선순위에 따른다.


## Java에서의 Synchronization

- 모든 자바 클래스에는 하나의 **monitor(mutex + condition variable)**가 존재한다.
- 묵시적으로 클래스 당 기본적으로 하나의 condition variable을 지닌다.
    - condition variable을 더 필요로 한다면 명시적으로 추가할 수 있다.
- mutual exculsion 사용 시 `synchronized` 키워드를 붙여준다.
- Bounded Buffer Problem
    
    ```java
    class BoundedBuffer{
    	volatile Buffer q; // 데이터를 입력하거나 소비할 버퍼
    	int first = 0; // first : 데이터를 꺼낼 지점 
    	int last = 0;  // last : 데이터를 넣을 지점
    	
    	public synchronized void producer(int item){ // 컴파일러가 synchronized 키워드를 보고 mutex로 procedure를 감싸줌
    		while((last - first) == N){ // 데이터를 넣을 빈 공간이 존재하는 지 확인
    			wait();
    		}
    
    		buff[last % N] = item;
    		notify(); // 버퍼에 데이터가 생성되기를 기다리던 프로세스를 깨움
    		last++;
    	}
    	
    	public synchronized int consumer(int item){
    		while(last == first){ // 버퍼에 데이터가 존재하는 지 확인
    			wait();
    		}
    		
    		int item = buff[first % N];
    		first++:		
    		notify(); // 버퍼에 빈 공간이 생기기를 기다리던 프로세스를 깨움
    		return item;	
    	}
    
    }
    ```
    
    ```java
    global volatile Buffer q;
    global Lock mutex;
    global CV fullCV;
    global CV emptyCV;
    
    public method producer(){
    	while(true){
    		task MyTask = ...;
    
    		mutex.acquire();  // monitor를 사용하기 위한 mutex 획득
    	
    		while(q.isFull){  // 데이터를 쓸 공간이 없다면 mutex 반납 후 fullCV에서 대기
    			wait(lock, fullCV);
    		}
    
    		q.enqueue(myTask); // 버퍼에 데이터 생성
    
    		signal(emptyCV); -- or-- boradcast(emptyCV); // 버퍼에 데이터가 생성되었으므로 emptyCV에서 대기중인 프로세스를 깨워줌
    
    		mutex.release(); // mutex 반납
    	}
    }
    
    public method consumer(){
    	while(true){
    		mutex.acquire();  // monitor를 사용하기 위한 mutex 획득
    
    		while(q.isEmpty()){ // 버퍼 내 데이터가 없다면 mutex 반납 후 fullCV에서 대기
    			wait(lock, emptyCV);
    		}
    
    		myTask = q.dequeue(); // 데이터 소비
    
    		signal(fullCV); --or-- broadcast(fullCV); // 버퍼에 데이터를 쓸 빈 공간이 생겼으므로 fullCV에서 대기중인 프로세스를 깨워줌
    
    		mutex.release();  // mutex 반납
    		
    		doStuff(myTask);  // 꺼낸 데이터로 작업 수행
    	}
    }
    ```
    
    - `wait` : condition variable의 wait와 동일
    - `notify` : condition variable의 signal과 동일
    - `notifyall` : condition variable의 broadcast와 동일