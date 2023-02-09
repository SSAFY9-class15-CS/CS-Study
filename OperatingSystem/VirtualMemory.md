## **가상 메모리 정의**

메인 메모리의 물리적 크기 보다 더 큰 메모리 영역을 사용자에게 제공하는 메모리 관리 기법. 가상 메모리 공간으로 실제 물리적 메모리 공간과 swap area를 사용한다.

## **가상 메모리 장점**

1. 메인 메모리 크기보다 큰 프로세스를 수행 할 수 있다.
2. Degree of Multiprogramming 을 높일 수 있다.

## 배경지식

### Segmentation

속성이 다른 section 들에게 다른 메모리 segmentation 할당하는 메모리 관리 방법.

### Paging

External Fragmentation을 극복하기 위한 메모리 관리 방법. 논리주소의 메모리를 고정된 크기의 페이지로 나누어 관리하는 기법. 페이지 테이블을 통해 논리적 페이지 주소와 물리적 메모리 공간인 프레임을 매핑한다.



### 지역성

Temporal Locality : 최근에 접근했던 주소값을 다시 접근하는 경향.

Spatial Locality: 최근에 접근했던 주소값 근처의 주소들을 접근하는 경향.

## Demand Paging

당장 필요한 페이지를 메인 메모리에 올리는 것. 

-구현 

Page table의 한개의 bit를 valid bit으로 둔다. 

valid bit이 ‘1’이면 메인 메모리 상에 해당 페이지가 올라와 있다는 뜻이고, 페이지 요청이 들어왔을 때 page hit이 일어나서 메인 메모리에 올라와 있는 페이지를 cpu에게 전달한다.

valid bit이 ‘0’이면 해당 페이지는 스왑 영역에 있다는 뜻이고, page fault가 발생하게 된다.



**-Swap Area**

메인 메모리에 저장되지 못한 페이지들을 저장하는 디스크의 공간으로 file에 비해 운영체제가 직접 빠르게 접근 할 수 있다.

**-Page Fault**

소프트 인터럽트의 한 종류이다. Virtual address가 가리키는 페이지가 메인 메모리에 없어서 프로세스가 더 이상 진행할 수 없는 상황을 OS에게 알려준다. 그렇게 되면 Page fault handler라는 ISR이 돌아가게 되고 해당 프로세스는 Block 되고 disk I/O operation을 기다린다. 

## TLB

Translation Lookaside Buffer. 페이지 테이블은 메인 메모리 위에 존재한다. 그로인해 cpu가 원하는 데이터를 얻기 위해서는 메인 메모리에 총 2회 접근해야 한다. 이런 과정을 효율적으로 개선하기 위한 캐시 구조.

자주 참조되는 가상주소-물리주소 변환 정보를 저장해놓은 캐시이다. 가상 주소로 데이터 접근시, TLB에 먼저 접근하여 변환 정보가 있는지 확인하고 있으면 바로 변환하고 없으면 페이지 테이블에 찾으러 간다. 단, TLB에 변환 정보가 없는 경우(TLB 미스), 페이지 테이블에서 해당 물리 주소를 찾아 TLB에 갱신 후, TLB 미스가 발생한 명령어를 재실행한다.



## P**age Replacement Policy**

Swap area에 있는 page를 메모리로 swap-in을 하려는데 비어있는 frame이 없을 수도 있다. 이런 경우 OS가 현재 당장 사용되지 않는 frame을 swap-out 시킨다. 이러한 과정을 Page Replacement라고 부른다.

Page Replacement Policy는 어떤 페이지를 쫓아낼지 정하는 방법이다.

-이상적인 방법

가장 먼 미래에 사용될 page를 내보낸다. 하지만, 현실에선 미래를 예측할 수 없으니 구현할 수 없는 방법이다.

-LRU

Least Recently Used. 이상적인 방법의 approximation 이다. 최근에 가장 사용되지 않은 페이지를 내보내는 방식이다. 구현하려면 사용된 시간을 저장해야 하는데 이 경우 메모리 오버헤드가 커져서 실제로 잘 사용하지 않는 방식이다.

-Clock Algorithm

LRU의 approximation 이다. 원형 큐와 Reference bit을 사용한다. 시간을 일정한 간격으로 나누고, 그 간격안에서만 사용유무를 체크한다. 

하드웨어적으로 일정 시간마다 reference bit 을 0으로 초기화 해준다. 그리고 해당 프레임이 사용될경우 reference bit을 1로 변경해준다. 쫓아낼 frame을 탐색할때는  reference bit 이 0 인 frame을 찾아 내보낸다.



## Page Replacement Style

-Global

Page replacement 를 할때 메모리에 존재하는 모든 page들을 replacement 대상으로 본다.

-Local

Page replacement 를 할때 해당 프로세스의 page들을 replacement 대상으로 본다.

장점: 다른 프로세스에 영향을 미치지 않는다.

단점: 자주 사용하는 페이지가 스왑 영역으로 옮겨져 시스템의 효율이 떨어진다.
