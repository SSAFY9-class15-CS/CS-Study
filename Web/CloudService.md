# 0309 Cloud 자료

# 클라우드 컴퓨팅이란?

- 클라우드 컴퓨팅은 인터넷으로 가상화된 IT 리소스를 서비스로 제공하는 것을 의미
- 인터넷 기술을 활용해 IT자원을 언제 어디서든 필요할 때 필요한 만큼 사용하고, 사용한 만큼 비용을 지불하는 IT환경
- 클라우드 컴퓨팅에서 가상화하여 서비스로 제공하는 대상은 서버, 플랫폼, 소프트웨어

---

# 클라우드 컴퓨팅의 특징

- **필요에 따라 사용하는 셀프 서비스 (On Demand Self-Service)**
- **물리적 위치에 구애 받지 않는 리소스 풀링 (Resource pooling)**
- **광대역 네트워크 연결 (Broad Network Access)**
- **신속한 탄력성 (Rapid Elasticity)**
- **측정 가능한 서비스 (Measured Service)**

---

# 클라우드 컴퓨팅의 장단점

## 장점

- 신속한 인프라 도입
- 유연한 인프라 관리
- 손쉬운 글로벌 서비스
- 합리적인 요금제

## 단점

- 생각보다 비싼 이용 비용
- 데이터 보관의 불안함
- 클라우드 벤더 lock-in

**📝 하이브리드 클라우드란?**

(빈칸)

---

# On-Premise

> 직접 서버를 설치하는 것
> 
- 서버의 하드웨어 부분과 소프트웨어 부분을 둘 다 직접 관리
- 웹사이트 트래픽이 증가하면 서버 메모리가 부족하게 되고, 이에 대해 메모리를 사서 꼽아야 한다.
- 개발자가 개발만에 집중할 수 없음

---

# 클라우드 서비스 유형

![Untitled](https://user-images.githubusercontent.com/95271588/224863752-8b0f7ea7-ce8c-415e-bf83-4d3885fea434.png)

## 💭 IaaS (Infrastructure as a Service)

- 인프라를 제공해주는 클라우드 서비스로 컴퓨팅 자원을 사용자가 원하는 형태로 제공
- 서버를 관리할 자체 인력이 줄어든다.
- 특정 기간에 사용이 몰리는 경우 효율적으로 대비 가능 - Ecommerce
    - 온프레미스 환경이라면 자체적으로 몰리는 트래픽에 대비해 서버를 증설해야 한다. 특정 기간 이외에는 사용률이 떨어진다.
- 서비스로 제공되는 인프라스트럭처입니다. 개발사에 제공되는 물리적 자원을 가상화
- 스타트업의 경우에 IaaS를 활용함으로써 큰 이득을 볼 수 있다.
- 클라우드 서비스 업체에서 보통 다양한 곳에 데이터 센터를 가지고 있으므로 사용자들로 하여금 더 가까운 위치에서 서비스를 제공받아 좀 더 높은 퍼포먼스을 보여주고 지연을 최소화 할 수 있다.
- IaaS를 통해 기업은 로컬 인프라를 호스팅된 ‘클라우드’ 인프라로 옮겨 운영할 수 있다. 사용한 스토리지와 컴퓨팅 시간에 대해서만 비용을 지불하며, 어떠한 하드웨어나 네트워크도 설치하거나 관리할 필요가 없다.
- Persona : system admin, IT admin

### 사용 예시

- 고성능 컴퓨팅
- 웹사이트 호스트
- 빅데이터 분석
- 앱 개발
- Disaster Recovery
    - 이미 클라우드 서비스 제공자가 갖고있는 다양한 장소를 통해서 복구가 가능한 인프라를 손쉽게 구축할 수 있다.
    - 자체적으로 복구 가능한 인프라를 구축하려면 굉장히 많은 자원이 든다.
- On-premise 보다 더 빠른 개발 및 테스트 환경 구축

### 장점

- Higher availability
- Lower latency and improved performance
- Comprehensive security
- Fast access to the latest technologies

### 단점

- 기존 온프레미스에서 클라우드 시스템으로 바뀌면서 생기는 문제들
- 레거시 시스템
    - 클라우드 서비스를 지원하지 않는 구형 컴퓨팅 시스템은 업그레이드가 필요
- 클라우드 환경에 대한 교육
- 새로운 보안 위협 - Ransomeware, Malware, Data Breaches

### 컴퓨팅

- CPU
- GPU
- RAM

### 스토리지

- 블록 스토리지
    - SSD 또는 하드 드라이브와 같은 블록에 데이터를 저장합니다.
- 파일 스토리지
    - NAS와 같이 파일로 데이터를 저장합니다.
- 객체 스토리지
    - 객체 지향 프로그래밍과 유사한 객체로 데이터를 저장합니다.

### 네트워킹

- IaaS 인프라에는 라우터, 스위치 및 로드 밸런서와 같은 네트워킹 리소스도 포함됩니다.
- IaaS 모델은 소프트웨어에서 이러한 어플라이언스의 네트워킹 기능을 가상화하여 작동

## example

- AWS EC2

> 📝 컴퓨팅 자원
> 
> 
> 서버, 네트워크, 스토리지
> 

## 💭 PaaS (Platform as a Service)

> 서비스형 플랫폼(PaaS)은 애플리케이션을 개발하고 유지 관리하는 데 사용할 수 있는 하드웨어 및 소프트웨어 인프라를 제공

- 대상 : developers
- PaaS는 애플리케이션을 구축하는 데 필요한 모든 것을 임대해 준다. 여기에는 서버뿐만 아니라 운영체제, 프로그래밍 언어 환경, 데이터베이스 및 데이터베이스 관리 툴이 포함된다.

## 효과

- IaaS보다 한 단계 더 발전한 PaaS는 개발자가 시스템 관리 업무에서 벗어나 가장 중요한 것, 즉 애플리케이션에 집중할 수 있게 해준다.

## 💭 SaaS (Software as a Service)

> 서비스형 소프트웨어(SaaS)는 인터넷을 통해 전체 소프트웨어 애플리케이션을 제공
> 
- SaaS는 일반적으로 다양한 구독을 통해 이용할 수 있는 온라인으로 호스팅하는 애플리케이션을 가리킨다
- 이들 애플리케이션은 클라우드에서 완전히 작동하며 인터넷과 브라우저를 통해 접속
- 본질적으로, 클라우드에서 실행되고 가입 기반의 가격책정 모델을 갖는 모든 애플리케이션이 SaaS 애플리케이션으로 간주
- Persona : anyone

### 장점

- 설치가 필요 없다.
- 따로 무언가를 하지 않아도 항상 최신버전으로 유지된다.

## 💭 CaaS (Container as a Service)

- 컨테이너 플랫폼은 IaaS의 최신 구현 방식
- CaaS 업체는 완전한 서버 호스트를 제공하는 대신 기업이컨테이너 내에서 서비스나 애플리케이션을 호스팅할 수 있도록 해준다.
- CaaS 업체는 컨테이너를 대신 관리
- CaaS 업체는 서버에 컨테이너를 배치하고 컨테이너 인스턴스 수를 늘리고 줄이는 툴을 제공한다

> 📝 **Container**
> 
> - 컨테이너는 가상머신보다 기반 호스트 자원을 더 효율적으로 활용할 수 있다.
> - ‘작은 가상머신’이라고 생각할 수도 있다.
> - 컨테이너는 실행도 빠르고, 단일 서버에서 여러 인스턴스를 실행할 수 있다

## 💭 FaaS (Function as a Service)

![FaaS](https://user-images.githubusercontent.com/95271588/224863931-0ece7526-1b00-4bc1-a5d3-d7fe61e9c984.png)

> 함수를 서비스로 제공한다.
> 
- FaaS는 프로젝트를 한개 이상의 함수로 쪼개서, 매우 거대하고 분산된 컴퓨팅 자원에 서비스를 제공하는 회사가 준비해둔 함수를 등록하고, 이 함수들이 실행되는 횟수( 그리고 시간) 만큼 비용을 내는 방식을 말한다.
- 여기서 함수는 실제 프로그래밍 수준에서의 메서드를 의미
- 사용자는 Rest API와 같은 HTTP 요청을 통해 함수를 호출하고 원하는 파라미터를 전달하여 함수가 리턴 값이 있다면 리턴 값을 받거나 혹은 함수의 동작 시작 이벤트를 발생시킬 수 있다.

### FaaS 생성 단계

1. 사용자는 자신의 프로그램 내부에서 FaaS 함수를 호출하는 구문을 삽입
2. 함수가 호출되게 되면 FaaS에게 Rest API 형식의 HTTP 요청이 가게 된다.
3. FaaS는 해당 함수를 저장소로 부터 읽어서 컨테이너 혹은 가상머신으로 생성하게 된다.
4. 생성이 완료되면 함수를 실행하고 결과를 반환하거나 함수가 수행해야하는 동작을 수행한다.
5. 이후 일정시간 동안 함수의 호출이 없다면 함수를 포함하는 컨테이너 혹인 가상머신은 FaaS 시스템에 의해 삭제된다.

### 효과

> S/W 개발자와 IT 업계가 **프로그래밍 로직에만 집중**할 수 있도록 해준다.
> 

### Service Examples

- AWS Lambda
- MS Azure Function

## 💭 BaaS (Backend as a Service)

> BaaS 시스템은 앱 개발에 있어서 **필요한 다양한 기능들을 API로 제공**해 줌으로서, 개발자들이 서버 개발을 하지 않고서도 필요한 기능을 쉽고 빠르게 구현할 수 있게 해준다.
> 

> 애플리케이션 개발 시 요구되는 복잡한 백엔드 기능들을 개발자가 직접 개발하지 않고 클라우드 공급자가 제공하는 서비스를 이용해 쉽고 안정적으로 구현하는 것
> 
- 쉽게 말하면 회원관리, 데이터 저장, 파일 관리, 푸시 알림, 데이터베이스 설계 등을 개발자가 직접 만들지 않아도 API로써 제공이 된다. 이미 만들어진 API들을 적절히 호출해서 백엔드를 구성하면 되기 때문에 작업시간이 단축되고 백엔드에 대한 지식이 많이 없더라도 개발이 가능하다.
- 기능들 예시
    - 데이터베이스, 소셜서비스 연동, 파일시스템
- 비용은 api 사용한 만큼
- 클라우드 공급자가 백엔드 개발 환경까지 제공해준다

### 장점

- 개발 시간의 단축
- 인건비 감축
- 서버 확장 작업의 불필요함
- 백엔드에 대해서 심오한 지식이 별로 없더라도 아주 빠른 속도로 개발 가능

### Service Examples

- Firebase

> 📝 게임과 모바일 백엔드 서비스
> 
> - GBaaS : 게임 백엔드 서비스 (클라우드 게임 서버 엔진)
> - MBaaS : 모바일 백엔드 서비스

> 📝 IDC (Internet Data Center)
> 
> 
> IDC 서버는 안전한 물리적 공간에 많은 서버를 입주시켜 365일 문제없이 사용이 가능하도록 관리하는 서버의 호텔과 같은 서비스
> 

---

# 서버리스 (Serverless)

> 📝 **Serverless 란?**
> 
> 
> 우리가 직접 서버를 관리하지 않아 신경 쓸 필요없는 경우
> 

> 📝 **서버리스 아키텍처**
> 
> 
> 서버를 직접 관리할 필요가 없는 아키텍처
> 

## 서버리스 컴퓨팅

> 클라우드 서비스 업체가 특정 코드를 실행하는 데 **필요한 컴퓨팅 리소스와 스토리지만 동적으로 할당한 다음 그 부분에 대해서만 비용을 청구**하는 클라우드 실행 모델
> 
- IaaS, PaaS의 경우 유저가 0명이든 1000명이든 같은 돈을 내서 서버를 장만해야 됬지만, 서버리스에서는 수행한 함수 만큼만 돈을 내면 된다.
    - 대기상태를 제외한 실제 사용 자원에 대해서만 청구가 되기 때문에 굉장히 경제적이며 자원을 효율적으로 사용할 수 있게 된다.
- 대표적인 서버리스 서비스인 AWS 람다 같은 경우 백만개의 함수 실행을 단돈 20센트밖에 안든다.
- 서버는 클라우드 제공 기업에서 전적으로 관리
    - 사용자는 스케일링, 업데이트, 보안 등 런타임 관리와 운영에 대해 시간을 소모하지 않고 핵심 제품에 집중할 수 있다.
- 서버 운영에 소모 되는 오버헤드가 줄어들면 개발자는 시간과 에너지를 확장 가능하고 안정적이고 훌륭한 제품을 개발할 수 있게 된다.

## 서버리스 컴퓨팅 원리

- 개발자가 서버리스에 업로드한 함수는 24시간 내내 돌아가는게 아닌 휴면 상태
- 사용자 요청이 오는 순간 서버리스는 잠들어 있는 함수를 깨우고 실행시켜 요청한 작업을 수행

## 장점

- 비용 절약!! → 실제 사용한 자원만큼만
- 애플리케이션의 품질에 집중 가능
- 높은 가용성과 유연한 확장
    - 애플리케이션 자동으로 확장 가능
    - 개별 서버 단위가 아닌 사용단위를 설정/해제 하여 용량 조정 가능
- 빠른 개발 배포
    - 간단한 패키징 및 배포
    - 릴리즈 주기 감소
    - 높은 생산성
    - 유지보수나 기능 추가에 효율적이라 관리보다 개발에 집중할 경우

## 단점

- **Cold Start**
    - request가 와서 함수를 생성시키는데 시간 (1ms 정도)  시간이 걸린다.
    - 만약 이러한 지연을 허용하지 않는 서비스라면 좋은 선택이 아니다.
    - AWS Lambda의 경우 어떤 함수가 자주 쓰이는지 판단해서 해당 함수는 아예 잠들지 않고 리퀘스트에 대응되도록 항상 실행상태로 유지
    - 서버가 항시 요청에 대기하고 있는게 아니기 때문에 IaaS, PaaS 모델보다 요청시간이 느리다
    - 실시간 서비스에는 적합하지 않다.
    - 프로젝트 규모가 커지거나 속도를 요구하는 프로젝트라면 서버리스는 좋은 선택이 아니다.
- **긴 시간을 요하는 작업에 불리함**
    - 서버리스는 단순 작업(댓글 쓰기, 이메일 보내기 등)에는 적합하지만, 긴 시간을 요하는 작업(동영상 업로드, 데이터 백업 등)에는 굉장히 비효율적이다.
    - 서버리스는 함수가 1회 호출 될 때 사용할 수 있는 메모리 및 시간에 제한이 있기 때문이다. 이에 따라 큰 기능을 잘게 나누어 구현해야 한다.
        - 이미 구현되어 있는 라이브러리를 쪼갤수도..
- **로컬 데이터를 사용할 수 없다. - Stateless**
    - 하나의 작은 기능으로 나뉘어진 함수들은 요청마다 새로 기동되어 호출되기 때문에 전후 상태를 공유할 수 없기 때문이다.
    - 또한 변수와 데이터의 공유가 불가능하며, 데이터를 로컬 스토리지에서 읽고 쓸 수 없다. (이는 서버리스 벤더에 따라 추가 서비스를 통해 극복이 가능하지만, AWS S3, Azure Storage등 일반적인 서버리스는 불가능하다.)
- ****클라우드 제공 플랫폼에 심하게 종속적****
    - 기존 IaaS나 PaaS모델은 플랫폼을 바꾸는게 어렵지 않지만(ex. AWS에서 Google Cloud로) 서버리스는 애플리케이션의 구조 자체를 바꾸기 때문에 다른 플랫폼으로 이전하는게 굉장히 힘들다.
        - *의문… BaaS 는 그럴 것 같은데, FaaS 도 그럴까??*
    - 이는 곧 사용중인 플랫폼의 가격이나 정책, 서비스 변경에도 민감하게 반응해야됨을 의미한다. 제공업체를 변경하면 새로운 벤더 사양에 맞추기 위해 시스템을 업그레이드하는 비용이 발생할 수도 있다.

## 사용 사례

### 배치 작업

데이터를 실시간으로 처리하는게 아니라, 일괄적으로 모아서 처리하는 작업인 배치 작업의 경우 24시간 운영되던 서버가 필요 없으며, 특정 시간에 수행되던 기능을 서버리스로 구현하면 효율적이다.

### 자동화 작업

- 넷플릭스는 동영상 업로드 시 파일의 인코딩과 검증, 태깅 이후에 공개되는 작업을 AWS Lambda를 통해 자동화 했다
- 실시간 비디오 스트리밍 앱 개발사인 페리스코프(Periscope)도 동영상의 유해성 여부를 확인하는 기능을 Lambda에서 운영하고 있다.

### 분석과 모니터링 기능

- CPU 사용량이 임계치에 도달했을 때 알림을 받거나 지속적으로 기록되는 로그를 분석하고 리포팅 하는데 서버리스를 사용할 수 있다.
- 미국 온라인 패션 매거진 버슬(Bustle)은 하루 1억건의 이벤트 처리와 데이터 분석 리포팅에 서버리스를 적용해 84%의 비용을 절감하였다.

### 챗봇 서비스

- 챗봇을 서버리스로 구현하면 API 호출 시 요청을 처리하고 유연한 확장이 가능해 많은 사용자에게 안정적인 서비스를 제공할 수 있다.
- 슬랙(Slack)을 기반으로 하는 챗봇 어플리케이션이나 Amazon Echo 그리고 AWS Lambda를 이용한 음성인식 어플리케이션이 늘어나고 있다.

## ETC

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/38aa3351-c939-44ad-8906-c82a221a2891/Untitled.png)

---

# 클라우드 서비스 비유

## Car

- Iaas = leasing
- Paas = renting
- SaaS = taxi

---

# 클라우드 서비스 유형 비교

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/192fdae2-4f30-4599-9ede-557b2f6f45c5/Untitled.png)

- PaaS 및 SaaS보다 IaaS에서 클라우드 리소스 구성을 더 완벽하게 제어할 수 있다.
- PaaS 및 SaaS는 IaaS에 비해 더 많은 인프라 기능을 가상화하고 관리할 구성 요소가 적다.

- On-demand
    - 쓴 만큼 지불
    - IaaS, PaaS
    - On-demand 서비스를 이용한다고 하더라도 대부분의 클라우드 서비스들은 사용자들이 서버를 사용하지 않더라도 그냥 가동만 시켜도 시간마다 결제가 된다.

- CaaS는 금방 PaaS와 SaaS의 빌딩 블록 중 하나로 자리 잡으며 계층화 아키텍처를 구성한다. 하나의 플랫폼이 모든 애플리케이션 요구 사항을 제공할 만큼 유연하지는 않기 때문에, 복잡한 애플리케이션은 여전히 SaaS, PaaS 및 CaaS의 조합으로 이루어져 있다.

## 서버리스 클라우드 서비스 비교

---

# Edge Serverless

데이터와 컴퓨트를 수많은 데이터센터에 걸쳐 분산 배포한다. 엣지 서버리스는 컴퓨팅을 사용자와 더 가까운 곳으로 가져와 지연시간을 줄일 수 있다.

---

# 클라우드 서비스 제공 형태

## 프라이빗 클라우드

- 기업이 시스템 자원을 직접 소유하고 자신들의 회사 전용 클라우드로 사용하는 것으로 보안성이 매우 뛰어나며 개별 고객의 상황에 맞게 클라우드 기능을 커스터마이징 할 수 있다는 장점이 있다.

## 퍼블릭 클라우드

- 외부 클라우드 사업자가 제공하는 서비스를 통해 클라우드를 이용하는 것으로 인터넷에 접속 가능한 모든 사용자를 위한 클라우드 서비스이다.
    
    ex) AWS / KT 클라우드 등
    

## 하이브리드 클라우드

- 퍼블릭 클라우드 서비스와 자체인프라(프라이빗 클라우드 / 온프레미스)를 함께 사용하는 서비스이다.

## 멀티 클라우드

- 여러 벤더가 제공하는 동일한 유형의 클라우드(퍼블릭 또는 프라이빗)의 클라우드를 2개 이상 사용하는 서비스이다.

> 📝 온프레미스 (On-premise)
> 
> - 온프레미스란 기업의 서버를 클라우드 같은 원격 환경에서 운영하는 방식이 아닌, 전산실 서버에 직접 설치해 운영하는 방식을 의미
> - 보안에 장점
> - IT 인프라 유지 비용 (복잡해지는 IT 환경에서는 더욱더 부담이 커진다)

---

# 클라우드 기술

# 가상화

> 물리적인 하드웨어 장치를 논리적인 객체로 추상화 하는 것
> 

## 가상화의 장점

- 물리적인 서버의 개수를 줄여 통합함으로써 서버의 전력 및 냉각 비용, 하드웨어 공간 비용 등을 줄 일 수 있다.
- 별도의 가상화된 시스템에 구축함으로써 문제가 발생해도 전체 시스템에 영향을 끼치는 것을 방지할 수 있다.
- 컴퓨팅 자원의 사용을 최대화하고 보다 쉽게 관리할 수 있다.
- 동일한 머신에서 다양한 유형의 앱 및 운영체제를 실행할 수 있다.
- 서버를 가상화하고 가상화된 이미지를 사용함으로써 프로비저닝 하는데 걸리는 시간을 줄일 수 있다.

## 가상화 기술의 종류

### 호스트 OS 형

![호스트 OS 형](https://user-images.githubusercontent.com/95271588/224864160-40c5b6fc-3ad7-4bd6-a480-52c4c384e5aa.png)

- **물리적 하드웨어에 호스트 OS를 설치하고, 그 위에 게스트 OS 전체를 가상화**
 하는 방식이다.
- ex) VMWare, Virtual Box

### 전가상화 방식 (Bare-metal/hypervisor)

![전가상화 방식](https://user-images.githubusercontent.com/95271588/224864233-c3e024aa-a057-4c58-92e5-bf35d4e93b81.png)

- 호스트 OS를 필요로 하지 않는 가상화 방식이다.
- 하이퍼바이저라는 소프트웨어를 물리 하드웨어 위에 직접 작동 시키고, 하이퍼바이저 위의 각각의 가상머신에 게스트 OS를 설치하여 작동 시키는 방식이다.
- 각각의 가상머신은 마치 독립적인 호스트 시스템처럼 작동하며, 가상머신들이 서로 간섭하지 않도록 하는 것이 하이퍼바이저의 역할이다.
- 각각의 OS 커널에서 사용하는 명령어가 다르기 때문에 하이퍼 바이저가 이러한 명령어를 번역하여 하드웨어에 전달
- 하이퍼바이저가 중재를 하기 때문에 처리 속도가 느리다.
- 하드웨어를 완전히 가상화
- Guest OS 는 자신이 가상머신의 OS인지 인지하지 못함 → 하이퍼 바이저의 존재를 모른다.

- 장점 : 하드웨어를 완전히 가상화하기 때문에 Guest OS 운영체제의 별다른 수정이 필요 없음
- 단점 : 하이퍼바이저가 모든 명령을 중재하기 때문에 성능이 비교적 느림

- 하드웨어 지원 전 가상화
    - Trap과 Emulation을 이용하여 Guest OS의 Application이 직접 하드웨어의 리소스를 요청해 사용할 수 있는 구조이다.
- 소프트웨어 적 전 가상화
    - Binary Translation을 이용하여 각 단계에서 모든 명령에 대해 하나하나 다 가상화하는 방법으로 진행된다.

### 반가상화 방식 (Para Virtualization)

- 전가상화의 가장 큰 문제점인 성능 문제를 해결하기 위해 나온 기술로, 하드웨어를 완전히 가상화 하지 않는 기술이다.
- 하이퍼콜(Hyper Call)이라는 인터페이스를 통해 하이퍼바이저에게 직접 요청을 날릴 수 있습니다.
- 쉽게 말하면 가상화된 각 OS들이 각각 다른 번역기를 갖고 있는 것입니다. 그 번역기는 각각 다른 OS에서 내리는 각각 다른 명령어를 “더해라”라고 번역해주게 되는 것입니다.
    - 이 과정을 **Binary Translation**이라고 한다.
- Guest OS 는 자신이 가상머신의 OS인지 알고 있음.
- 따라서 반가상화를 위해서는 중요한 명령어와 아닌 명령어를 구분할 수 있어야 하는데, 그렇기에 Guest OS의 커널의 수정이 필요한 것이다.

- 장점 : 모든 명령을 DOM0를 통해 하이퍼바이저에게 요청하는 전가상화에비해 성능이 빠름
- 단점 : 하이퍼바이저에게 Hyper Call 요청을 할 수 있도록 각 OS의 커널을 수정해야하며 오픈소스 OS가 아니면 반가상화를 이용하기가 쉽지 않음

### 컨테이너 가상화

호스트 OS위에 컨테이너관리 소프트웨어를 설치하여, 논리적으로 컨테이너를 나누어 사용합니다.

컨테이너는 어플리케이션 동작을 위한 라이브러리와 어플리케이션등으로 구성되기때문에 이를 각각 개별 서버처럼 사용가능합니다.

• 장점 : 컨테이너 가상화는 오버헤드가 적어 가볍고 빠른 장점이 있음

---

# 컨테이너(Container)
> **컨테이너란** 호스트 OS상에 논리적인 구획(컨테이너)을 만들고, 어플리케이션을 작동시키기 위해 필요한 라이브러리나 어플리케이션 등을 하나로 모아, 마치 별도의 서버인 것처럼 사용할 수 있게 만든 것
> 
- 운영체제에서 실행되는 프로세스를 격리(Isolation)하여 별도의 실행 환경을 제공
- 해당 프로세스는 운영체제 상에서 실행되는 유일한 프로세스인 것처럼 작동하는 기술
- 호스트 OS의 리소스를 논리적으로 분리시키고, 여러 개의 컨테이너가 공유하여 사용
- 컨테이너는 오버헤드가 적기 때문에 가볍고 고속으로 작동하는 것이 특징
- 컨테이너 기술을 사용하면 OS나 디렉토리, IP 주소 등과 같은 시스템 자원을 마치 각 어플리케이션이 점유하고 있는 것처럼 보이게 할 수 있습니다.
- ****컨테이너는 어플리케이션의 실행에 필요한 모듈을 컨테이너로 모을 수 있기 때문에 여러개의 컨테이너를 조합하여 하나의 어플리케이션을 구축하는 마이크로 서비스형 어플리케이션과 친화성이 높은 것이 특징
- 컨테이너는 가상 머신과 마찬가지로 어플리케이션을 관련 라이브러리 및 종속항목과 함께 패키지로 묶어 소프트웨어 서비스 구동을 위한 격리 환경을 마련
- 컨테이너를 사용하면 개발자와 IT운영팀이 훨씬 작은 단위로 업무를 수행할 수 있음

## 장점

### 가벼움

사용자의 Request Traffic 이 증가함에 따라, 가상머신이나 컨테이너를 추가적으로 배포할 때 가상머신의 크기는 최소 GB 단위 이지만, 컨테이너의 경우 Guest OS가 없기에 MB 단위의 크기를 가집니다.

### 탄력성

컨테이너는 Linux, Windows, 가상머신, Data Center, Public Cloud 등 어느 환경에서나 구동이 되므로 개발 및 배포가 크게 쉬워집니다.

### 유지 관리 효율

운영 체제 커널이 하나밖에 없기 때문에 운영 체제 수준에서 업데이트 또는 패치 작업을 한 번만 수행하면 변경 사항이 모든 컨테이너에 적용됩니다.

이를 통해 서버를 더 효율적으로 운영하고 유지 관리할 수 있습니다.

## 컨테이너 아키텍처

### Linux Namespace

> 격리를 담당
> 
- 마운트 포인트
- 프로세스
- 네트워크 - IPC
- UTS
- 사용자

### Control Group

> 리소스를 제어
> 
- CPU
- 메모리
- 네트워크 대역폭
- 디스크 입출력

## 단점

### 커널 공유

- 컨테이너는 호스트 OS의 통제된 영역을 사용한다 하더라도 많은 컨테이너들이 동일한 운영체제 커널을 공유. 따라서 컨테이너들은 가상머신만큼 철저하게 분리되어 있지 않기 때문에 보안이나 안정성 측면에서 문제가 발생할 수 있음
- 이를 보완하는 안전 장치가 있긴 하지만 이 역시도 동일 호스트 위에서 이루어지기 때문에 잘못된 스케줄링이나 예기치 못한 컨테이너간의 충돌 등이 발생할 가능성이 존재

### 영속성

- 또한 컨테이너는 기본적으로 비 저장성을 갖는 이미지로부터 실행. 이는 시스템을 간결하게 하는 장점으로 작용했지만 작업의 영속성 측면에서는 단점
- 이를 위해서는 컨테이너와 연결된 별도의 데이터베이스나 독립적인 스토리지가 있어야 하므로, 자체 파일 시스템을 가지고 있어 기본적으로 세션에 대한 영속성을 가지고 있는 가상머신보다는 불리

### 하이브리드

- 컨테이너 방식이 확산될 거라는 전망에는 업계 관계자 대부분 이견이 없는 상황. 그러나 컨테이너가 하이퍼바이저를 당장에 대체하기에는 어려울 것이라는 시각이 지배적
- 따라서 컨테이너와 서버 가상화 기술이 서로의 약점을 보완하면서 공존할 것으로 전망하고 있으며, 현재 하이퍼바이저 진영에서 컨테이너 기술을 융합한 가상화 기법들을 시도하고 있어 하이브리드 형태의 새로운 기술이 등장할 수도 있을 것으로 전망

## 컨테이너 vs 가상머신

가상머신은 하드웨어 스택을 가상화합니다. 컨테이너는 이와 달리 운영체제 수준에서 가상화를 실시하여 다수의 컨테이너를 OS 커널에서 직접 구동합니다. 컨테이너는 훨씬 가볍고 운영체제 커널을 공유하며, 시작이 훨씬 빠르고 운영체제 전체 부팅보다 메모리를 훨씬 적게 차지합니다.

---

# 도커
![Docker](https://user-images.githubusercontent.com/95271588/224864458-025ac62d-90f1-4058-a3d0-3b15c7a11e11.png)

### 이미지

- 실행할 애플리케이션과 라이브러리 및 환경을 하나의 패키지로 묶은 것

### 컨테이너

- 이미지를 실행한 컨테이너

### 레지스트리

- 이미지를 저장하고 공유할 수 있는 스토리지

---

# 쿠버네티스

> 컨테이너에 배포된 많은 서비스를 관리하는 것, 컨테이너 오케스트레이터의 역할
> 
- Auto scaling 을 이용한 탄력적인 자원 사용이라고 할 수 있습니다.
- 또한, Rolling Update 로 서비스 중지없이 업데이트를 할 수 있다는 점도 있습니다.
- 쿠버네티스의 역할은 컨테이너를 분산 배치, 상태 관리 및 컨테이너의 구동 환경까지 관리해 주는 도구

> 📝 **오케스트레이션(Ochestration)**
> 
> - 컨테이너 역시 그 수가 많아지게 되면 관리와 운영에 있어서 어려움이 따름. 컨테이너 오케스트레이션은 이러한 다수의 컨테이너 실행을 관리 및 조율하는 시스템
> - 오케스트레이션 엔진을 통해 컨테이너의 생성과 소멸, 시작 및 중단 시점 제어, 스케줄링, 로드 밸런싱, 클러스터링 등 컨테이너로 어플리케이션을 구성하는 모든 과정을 관리할 수 있음

---

# 필요 개념
## 📔 IDC 서버 vs 클라우드 서비스

|  | IDC 서버 | 클라우드 서비스 |
| --- | --- | --- |
| 이용 방식 | 물리적 서버를 단독으로 임대 또는 구매하여 서버 관리 인프라 및 기술력을 외주형태로 제공받음 | 가상의 서버를 임대하는 방식으로 별도의 서버 구매 비용 없이 즉시 서비스를 시작할 수 있음 |
| 주 사용자 | 상시적으로 대규모 트래픽과 안정적인 서비스가 필요한 대형 쇼핑몰, 인터넷 서비스 기업 등 | 트래픽이 유동적이며 많지 않은 경우. 소규모 그룹 및 개인 그리고 스타트업 기업 등 |
| 장점 | 안정적인 서비스, 전문 관리인력의 상주 | 저렴한 비용으로 안정적인 서비스 가능. (이용한만큼 요금 발생) |
| 단점 | 사용하지 않아도 항상 일정한 요금이 발생, 초기 서버 구입비용이 큼 | 트래픽이 증가할 경우 추가 비용부담 발생 가능 (사전에 요금인지 가능) |

## 📔 프로비저닝

- 프로비저닝은 IT 인프라를 설정하는 프로세스
- 사용자와 시스템에서 사용할 수 있도록, 데이터와 리소스에 대한 액세스를 관리하는 데 필요한 단계

### 서버 프로비저닝

- 서버 프로비저닝이란 필요한 리소스를 기반으로 네트워크에서 사용될 서버를 설정하는 프로세스입니다.

### 사용자 프로비저닝

- 사용자 프로비저닝이란 액세스 권한과 인증 권한을 모니터링하는 Identity 관리 유형
- 사용자 프로비저닝은 IT와 HR 사이에서 관리되는 경우가 많습니다.

### 네트워크 프로비저닝

- 네트워크 프로비저닝은 특히 사용자, 서버, 컨테이너, IoT 기기가 액세스할 네트워크를 설정하는 작업으로 구성될 수 있습니다.
- 네트워크 프로비저닝이란 필요한 장비와 배선을 비롯해 사용자에게 통신 서비스를 제공하는 것을 지칭하는 방식

### 서비스 프로비저닝

- 서비스 프로비저닝이란 서비스 설정과 이와 관련된 데이터 관리를 포함하는 프로비저닝을 뜻합니다.
- 서비스 프로비저닝은 통신 업계에서 고객을 위한 서비스나 클라우드 인프라
를 설정하는데 사용

### 프로비저닝 자동화

- 과거 IT 인프라 프로비저닝은 물리적 서버의 설정부터 하드웨어를 원하는 상태로 설정하는 것까지 보통 수동으로 이루어졌으며, 추가 용량을 원하는 경우 하드웨어를 더 주문해서 도착할 때까지 기다리고 나서야 설정과 프로비저닝 작업을 진행할 수 있었습니다.
- 하지만 오늘날 인프라는 소프트웨어에 정의되어 있는 경우가 많으며 가상화
와 컨테이너 덕분에 프로비저닝 프로세스 속도를 높이면서도 하드웨어 프로비저닝과 관리를 빈번하게 진행할 필요가 없어졌습니다.프로비저닝은 자동화를 통해 처리할 수도 있습니다.

## 📔 미들 웨어 (Middleware)

> 미들웨어(Middleware)는 응용 소프트웨어가 운영체제로부터 제공받는 서비스 이외에 추가적으로 이용할 수 있는 서비스를 제공하는 컴퓨터 소프트웨어
> 
- 미들웨어는 **양 쪽을 연결**하여 데이터를 주고 받을 수 있도록 **중간에서 매개 역할**을 하는 소프트웨어
- 네트워크를 통해서 연결된 여러 개의 컴퓨터에 있는 많은 프로세스들에게 어떤 서비스를 사용할 수 있도록 연결해 주는 소프트웨어
- 3계층 클라이언트/서버 구조에서 미들웨어가 존재한다.
- 웹 브라우저에서 데이터베이스로부터 데이터를 저장하거나 읽어올 수 있게 중간에 미들웨어가 존재하게 된다.

- **개발 조직** 👉🏻 소스 개발/운영에 집중하고 사용자를 고려한 서버 튜닝과 분석을 맡길 수 있다.
- **인프라 조직** 👉🏻 실제 유입되는 서비스를 알아채고 특정 로직이 수행되면서 인프라 영역에 문제가 된다를 통역해줄 수 있다.
- 즉, 미들웨어라는 분야답게 개발 <> 인프라 조직 간 중간 다리 역할을 수행하는 분야로써 필요하다.


# Reference

1. [[클라우드] 클라우드 컴퓨팅](https://velog.io/@klloo/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%BB%B4%ED%93%A8%ED%8C%85)
2. [[클라우드] 가상화](https://velog.io/@klloo/virtualization)
3. [가상화 종류 3 가지](https://tech.cloud.nongshim.co.kr/2018/09/18/%EA%B0%80%EC%83%81%ED%99%94%EC%9D%98-%EC%A2%85%EB%A5%983%EA%B0%80%EC%A7%80/)
4. [한방에 이해되는 가상화 기술 용어 정리](https://tech.ktcloud.com/77)
5. [Full Virtualization & Para Virtualization](https://suyeon96.tistory.com/53) — good
6. [[OS/운영체제] 인터럽트(Interrupt), 시스템 콜(System Call)](https://www.notion.so/0309-Cloud-4a525e423c584d0fa2d9793d347136ad)
7. [Kernel Trap(트랩) —- 깊은 내용](https://autumnrain.tistory.com/entry/Kernel-Trap-%ED%8A%B8%EB%9E%A9)
8. [서비스형 인프라(IaaS)란 무엇인가요?](https://aws.amazon.com/ko/what-is/iaas/)
9. [클라우드 서비스 이해하기 IaaS, PaaS, SaaS](https://www.whatap.io/ko/blog/9/)
10. [iaas paas saas](https://www.veritas.com/information-center/iaas-paas-saas)
11. [Paas Explained](https://www.youtube.com/watch?v=QAbqJzd0PEE) - IBM Technology
12. [https://inpa.tistory.com/entry/WEB-🌐-서버리스ServerLess-개념-💯-총정리-BaaS-FaaS](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4ServerLess-%EA%B0%9C%EB%85%90-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC-BaaS-FaaS)
13. [“FaaS를 넘어 엣지로” 차세대 엣지 서버리스 아키텍처](https://www.itworld.co.kr/news/144224)
14. [프로비저닝이란?](https://www.redhat.com/ko/topics/automation/what-is-provisioning)
15. [[프로그래밍] 미들웨어(Middleware)란?](https://12bme.tistory.com/289)
16. [인프라 뿌시기1 - 미들웨어 개념을 알아보자](https://velog.io/@unyoi/%EC%9D%B8%ED%94%84%EB%9D%BC-%EB%BF%8C%EC%8B%9C%EA%B8%B01-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4-%EA%B0%9C%EB%85%90%EC%9D%84-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
17. [[쿠버네티스] 컨테이너가 뭐에요? (컨테이너의 기본 개념, 컨테이너란?)](https://nearhome.tistory.com/83)
18. [클라우드 가상화 기술 정리3](https://blog.naver.com/shakey7/221600947441)
19. [https://www.apthow.com/linux에서-windows-컨테이너를-호스팅-할-수-있습니까-를-기반net462/?amp=1](https://www.apthow.com/linux%EC%97%90%EC%84%9C-windows-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EB%A5%BC-%ED%98%B8%EC%8A%A4%ED%8C%85-%ED%95%A0-%EC%88%98-%EC%9E%88%EC%8A%B5%EB%8B%88%EA%B9%8C-%EB%A5%BC-%EA%B8%B0%EB%B0%98net462/?amp=1)