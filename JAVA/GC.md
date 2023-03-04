# Garbage Collection
## 1. GC 사용이유

- 이런 함수가 실행된다고 해보자.

```java
for (int i = 0; i < 1000; i++){
	Person p = new Person();
	p.write();
}
```

- 다음과 같은 함수의 지역변수는 for문 탈출후 재사용 될일이 없다.
- Java에서는 이러한 쓸모없는 객체들을 자동으로 처리해준다. 이를 Garbage Collection이라 한다.
- Java에서의 Garbage Collection은 예측하기 쉽지않기 때문에, 최소한으로 횟수를 줄이고, 실행시간을 단축시키는게 중요하다.
- GC가 실행되면 모든 스레드가 멈춰버리기때문이다. (Stop-the-world)
- 따라서 Java에서 GC처리는 매우 중요하다.
- GC 튜닝 : GC 최적화 작업. GC는 자주 실행되는 것을 줄이기위해 튜닝을 하기도한다.
    - 목적: Old 영역으로 넘어가는 객체 수 줄이기, Full GC (Major GC) 실행시간 줄이기
    - GC 튜닝은 주 목적이 Stop-the-world 시간을 조정하는것이기 때문에 자신이 사용하는  서비스의 전반적인 흐름이 파악된 후 적용해야한다.

## 2. GC 동작 과정

### 2-1. Minor GC

- 힙영역의 설계 기준 2가지를 전제로 한다.
1. 대부분의 객체는 금방 접근 불가능 상태가 됨.
    - Reachable vs unreachable 객체 찾기
        - 접근 가능하고 접근 불가능한 객체는 주소로 참조할 수 있는지 여부이다.
        - 이를 찾아 주기적으로 제거
2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재함.
    
    → 이를 바탕으로 Minor GC를 설계.
    
    → 객체의 수명을 관리하기 위해 heap을 물리적으로 Young 영역, Old 영역으로 나눔.
    
- Young 영역은 Eden과 Survivor 영역으로 또 나뉨.

![Minor GC1](https://user-images.githubusercontent.com/70370578/222874314-57173aa2-784d-458b-88be-15a70e8f1255.png)

- 새로 들어온 객체는 Eden 영역으로 보관되고, Eden 영역이 꽉 차면 Minor GC가 발생함.

![Minor GC2](https://user-images.githubusercontent.com/70370578/222874362-0a3c1bda-a648-40c1-9326-ce1db407af9c.png)

- Minor GC가 발생할때 참조되지않은 객체는 제거된다.
- 여기서 살아남은 객체는 survivor 영역으로 이동함.
- 따라서 survivor 영역은 Minor GC에서 1번이상 살아남은 객체들이다.
- survivor 영역에는 0,1 영역이 있다.
- 원래 survivor 0 영역에 있던 객체들은 survivor 1영역으로 이동한다. survivor 0 영역이 가득 차게 되면 살아남은 객체를 survivor 1영역으로 이동시킨다.

→ 사실 가득차게 되면 이동한다기 보다는 계속 번갈아가면서 이동하는것이다.


![Minor GC3](https://user-images.githubusercontent.com/70370578/222874077-7b92365c-1e2e-45d3-ac80-45925001186c.png)

- eden → survivor0 으로 이동했다가, 다음 번째에 eden 영역에서 살아남은 객체들은 survivor0 객체들과 함께 survivor1 영역으로 이동한다.
- 그리고 다음 라운드에 다시 survivor0 영역으로 이동한다.
- 따라서 가비지 컬렉터가 잘 작동하는지 확인하려면, survivor 영역에 주목하면된다.
    1.  survivor 영역 0,1 둘중 하나만 이용되어야하고
    2.  사용량이 0이 아니여야한다.
- 다시 반복하여 살아남은 객체들은 old 영역으로 이동시킨다.

![Minor GC4](https://user-images.githubusercontent.com/70370578/222874080-1fac1ffd-2a36-45c7-baf4-11f32d4794c3.png)

- 각 객체는 age bit를 가지고있다.
- age bit는 Minor GC에서 살아남은 횟수를 기록하는 비트이다. 이는 Minor GC가 발생할때마다 하나씩 증가한다.
- MaxTenuringThreshold(임계점) : age bit의 기준 값
    - age bit가 MaxTenuringThreshold 값을 초과하게 되는경우, old generation 영역으로 이동하게된다. → Promotion
    - MaxTenuringThreshold 값은 JVM에 의해 동적으로 정해진다.

### 2-2. Major GC

- Old Generation 영역에서 실시됨.
- 주기가 느리다.

![Major GC1](https://user-images.githubusercontent.com/70370578/222874093-5b7b6fdd-58de-4a0d-9765-02931c5823ce.png)

- old generation 영역에서 young generation 영역으로의 참조는 보통 드물게 존재하는데, 이 경우에는 512바이트짜리 카드테이블 형태에서 young generation 을 참조하는 방식으로 수행한다.
- 카드테이블에는 old generation 영역에서 young generation 영역 객체를 참조할때마다 정보를 저장하는데,
- old generation 영역 옆에는 permanent generation이 위치
    - permanent generation : method area (클래스와 메소드의 메타데이터 저장)
- 이 method area는 mark and sweep의 root로도 활용될수있다.

## 3. GC 알고리즘

- 사전 지식
    - Stop-the-World
        - GC를 실행하기 위해 JVM이 어플리케이션 실행을 멈추는 작업.
        - GC Thread 외에 모든 Thread를 중지. GC가 완료되면 다시 작업 재개.
        - 기본적인 방식임. 어떻게 하더라도, stop-the-world 시간이 발생한다.
- 기본 사용방식
    - reference counting
        - 단점 : 순환참조를 해결할수없음.
    - Mark and Sweep
        - Young Generation : Mark-Sweep-(Compaction)
            - Mark : 사용되고있는 메모리와 사용되고있지않은 메모리를 식별
            - Sweep : Mark에서 사용되고있지않은 식별된 메모리를 해제
        - 동작 과정
            
            ![Mark and Sweep](https://user-images.githubusercontent.com/70370578/222874257-7ae84873-e10d-45a1-b5e3-ede79c6dfafe.png)
            
        - GC Root에서 그래프 순회.
        - 참조되어있는 모든 객체를 Mark하고, Mark 되지 않은 객체들의 메모리를 해제한다.(sweep)
        - 이는 루트로부터 연결되지않는 다른 부분에 참조된 메모리도 같이 해제한다. (다른 곳을 참조하여 사용이 되지않는 부분들)
        - GC Root는 기본적으로 하나의 집합이다. (GC Root Set)
        - GC root는 힙영역을 참조하는 stack, method area 등이 됨.
        - compaction: sweep 후에 분산된 객체들을 heap의 시작주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나누어 압축한다.(가비지 콜렉터 종류에따라 하지않는 경우도 있다.)
    - Old generation : Mark-Sweep-Compaction.
        - 컴팩션 과정을 포함

### 3-1. Serial GC : Single-thread

- Mark-Sweep-Compact
    - Mark: old 영역에서 살아있는 객체 식별
    - Sweep: heap 앞부분부터 확인해서 살아있는것을 남김.
    - Compact: heap 앞부분부터 차례대로 쌓이도록 가장 앞부분부터 놓음.
- 적은 메모리와 CPU 코어 개수가 적을때 사용

### 3-2. Parallel GC : Multi-thread

- Java 8의 default GC.
- serial GC보다 빠르게 객체 처리 가능 (Multi-thread)
    - 단, Old Generation 영역은 Single-thread,
    - Parallel Old GC (Parallel Compacting Collector) 에서는 Old Generation 영역도 Multi-thread로 변경하였다.
- 메모리가 충분하고 CPU 코어 개수가 많을때 사용
- 기본적으로 CPU 개수만큼 GC thread를 할당하는데, 이는 유동적으로 조절할수있다.

- Serial GC vs Parallel GC

![Serial vs Parallel](https://user-images.githubusercontent.com/70370578/222874154-f21aa699-26b7-4670-b135-d5898f55e6e8.png)

### 3-3. CMS GC (Concurrent Mark Sweep GC)

- Application Thread와 GC Thread 동시 실행 → Stop the world 시간을 줄인다.
    - 즉, 평소에 쓰는 스레드와 가비지 컬렉션 스레드가 동시에 실행된다는 뜻
- 대신에, GC 과정이 복잡해진다.
- 그 중 하나가 GC 대상을 판별하는 방식이다.

![Serial](https://user-images.githubusercontent.com/70370578/222874172-4cc40b16-1167-4b36-94a2-041bfb2698bc.png)
![Parallel](https://user-images.githubusercontent.com/70370578/222874188-5255419c-44d9-4811-b9a3-8a5edff07592.png)
![CMS GC](https://user-images.githubusercontent.com/70370578/222874202-def15de5-91a1-4fb7-8f31-7034fa2070b4.png)
![Pointer](https://user-images.githubusercontent.com/70370578/222874478-be9c33fc-c477-4253-96d7-55cae1dc4db6.png)

- Serial GC, Parallel GC와 달리, CMS GC에서는 세부적으로 나눠서 실행된다.
- 기존 GC Thread만 실행되던게
    - Initial Mark →  Marking and Pre-cleaning → Remark → Sweeping and Reset이 된다.
- 보기만해도 세부적으로 4단계나 더 실행되기때문에 자원을 많이 소모할수밖에없다.
- 또한 Memory Fragmentation도 발생한다.
- 따라서 Java 9에서 시작해서 Java 14에서는 사용 중지 되었다,

### 3-4. G1GC (Garbage First Garbage Collector) : jdk 7


![G1GC](https://user-images.githubusercontent.com/70370578/222874216-34f4e2e1-af64-4232-8a11-b858d8ae058e.png)

- CMS GC를 대체하기위해 만듬.
- Java 9+ 부터 GC default.
- 이 방식은 메모리 제한과, Stop-the-world 시간 제한이 있다.
    - Heap 영역은 4GB이상, Stop-the-world : 0.5초까지
- G1GC는 기존 GC 알고리즘과 영역을 나누는 방식이 완전히 다르다.
    - 기존 : Young / Old Generation → G1GC : Region
- 체스판처럼 여러 region으로 나누어 관리한다.
    - 기존 영역에 정해진 역할 (Eden, Survivor, Old)을 미리 정해두지않고, Region 상황에 맞게 동적으로 부여한다.
    - 이는, 가득찬 영역을 바로 회수해서 빈공간을 확보후, 사용할수있으므로, GC 횟수가 줄어든다.
        - 기본적으로 메모리 탐색 방식이 전체 탐색이 아니라 Region 별 탐색이다.
    - 게다가, 기존 순차적 이동방식과는 달리 이동도 하지않는다. 즉, 메모리 복사가 필요없다. 대신 Reallocate 시킨다. (주소만 바꿈.)
    - 순차적 이동 방식도 없으므로, 기존 survivor 1 에 있는 객체를 eden 영역으로 재할당할수도 있음.
- 비어있는 영역에만 새로운 객체가 들어간다.
- 쓰레기가 쌓여 꽉 찬 영역을 우선적으로 청소한다.
- 실시간 방식이 아니다.
- 일시정지(Stop-the-world) 시간을 완전히 없애진못한다.
