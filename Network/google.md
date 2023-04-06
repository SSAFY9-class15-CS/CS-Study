# 브라우저 주소창에 www.google.com을 입력하면 일어나는 일

브라우저에 [www.google.com](http://www.google.com) 을 치면 일어나는 일에 대해 알아보자.

<사전 지식>

시작하기 전에 네트워크 전체의 대략적인 그림에 대해 미리 알아보자. 

클라이언트가 서버와 통신하기 위해서는 서버의 포트넘버와 IP 주소가 필요하다. 포트넘버는 사전에 약속되어 있으므로 클라이언트가 필요한 것은 서버의 IP 주소이다. 서버의 IP 주소만 알면 통신을 시작 할 수 있다.

그 원리에 대해 먼저 간략히 설명하자면, 인터넷이란 수 많은 라우터의 집합체라고 볼 수 있다. 도착지의 IP 주소를 알게 되면 도착하기 위해 거쳐가는 라우터들이 길을 알려주는 형식이다.

![제목_없는_아트워크(resize)](https://user-images.githubusercontent.com/108070719/230247949-4b944d62-7f64-4998-a9da-c2e4333c2bdc.jpg)

<설명 과정>

계층별 추상화된 네트워크 개념에 관해 한꺼풀 한꺼풀 벗겨가며 설명을 진행 할 예정입니다.

캐싱에 대해서는 우선 무시하고 설명하고 나중에 설명을 덧붙일 예정입니다.

# 1. 브라우저와 TCP 연결

브라우저 주소창에 [www.google.com](http://www.google.com) 을 입력하면 구글 서버와 TCP 연결을 시작한다. 3-way-handshake 를 통해 TCP Connection 을 만들고 다음과 같은 http 메시지를 작성한다.

1. **Request URL:** https://www.google.com/
2. **Request Method:** GET
3. **Status Code:** 200
4. **Remote Address:** 142.250.206.228:443
5. **Referrer Policy:** strict-origin-when-

이 메시지가 서버에 도착하게 되면 구글 서버는 response http 메시지를 보내주게 되고, 그 메시지가 브라우저에 도착하면 브라우저가 화면을 랜더링하여 사용자에게 웹사이트 화면을 보여주게 된다. 그리고 TCP Connection 이 종료되고 끝이나게 된다.

여기서 드는 의문점이 있다. 서버와 통신하기 위해서는 IP 주소가 반드시 필요하다고 했는데 우리는 ip 주소에 대해 입력한 바가 없다. 또한, TCP handshake 를 위한 정보가 구글 서버에 도대체 어떻게 도달한 것일까?

그걸 알기 위해선 DNS 에 대해 알아야 한다. DNS 는 자동으로 우리가 원하는 웹사이트 서버의 IP 주소를 알려주는 시스템이다. 그런데 여기서 또 한번의 의문이 있다. 그럴려면 DNS 서버의 IP 주소를 알아야 하는데, 그것은 어떻게 해결하는 것일까?

# 2. DHCP

DHCP(Dynamic Host Configure Protocol) 이 그것을 해결해준다.

이름 그대로 동적으로 IP 주소를 배분하는 시스템이다. 여러분들이 스타벅스에 가서 와이파이를 사용하도록 만들어주고, 집 공유기와 연결된 컴퓨터나 와이파이를 사용 할 때 인터넷을 사용할 수 있도록 만들어주는 프로토콜이다.

새로운 네트워크 인터페이스가 네트워크에 연결되었을 때 DHCP 서버는 그 네트워크 인터페이스에 

- IP 주소 / 서브넷마스크
- 게이트웨이 라우터 IP 주소
- DNS 서버 IP 주소

를 무조건 제공한다. 그렇게 IP 주소를 가지게 되고 DNS 서버 IP 주소를 알 수 있게 되는 것이다. 서브넷 마스크와 게이트웨이 라우터의 역할 에 대해서는 뒷부분에서 설명하도록 하겠다.

# 3. DNS

그럼 이제 DNS 서버의 주소에 대해 알게 되었으니 DNS(Domain Name System) 에 대해 알아보자.

DNS 는 UDP 프로토콜로 동작한다. 

위에서 DNS 란 웹사이트의 IP 주소를 알려주는 시스템 이라고 했다.  즉, [www.google.com](http://www.google.com) 이라는 도메인을 보내면 그에 맞는 IP 주소를 알려 주는 역할이다. 그렇게 우리는 이제 구글 서버의 IP 주소를 알게 된다.

DNS 동작의 흐름에 대해 자세히 알아보자.

<img width="605" alt="캡처 PNG" src="https://user-images.githubusercontent.com/108070719/230247984-aad20e85-ec60-4f1d-8681-af53be959bb9.png">

**1.** 웹 브라우저에 www.naver.com을 입력하면 먼저 PC에 저장된 **Local DNS(기지국 DNS 서버)**에게 "www.naver.com"에 대한 IP 주소를 요청한다. 

보통 Local DNS 에 IP 주소가 캐싱되어 있을 확률이 높지만, 이 경우엔 캐싱되어 있지 않다고 가정한다.

**2.** 그러면 Local DNS는 이제 "www.naver.com 의 IP 주소"를 찾아내기 위해 다른 DNS 서버들과 통신(DNS 쿼리)을 시작한다.먼저 **Root DNS 서버**에게 "www.naver.com 의 IP 주소"를 요청한다.

**3.** Root DNS 서버 는 "www.naver.com 의 IP 주소" 를 찾을 수 없어 Local DNS 서버에게 "www.naver.com 의 IP 주소 찾을 수 없다고 다른 DNS 서버에게 물어봐" 라고 응답을 한다.

**4.** 이제 Local DNS 서버는 **com 도메인을 관리하는 TLD DNS 서버(최상위 도메인 서버)**에 다시 www.naver.com에 대한 IP 주소를 요청한다.

**5.** com 도메인을 관리하는 DNS 서버에도 해당 정보가 없으면, Local DNS 서버에게 "www.naver.com 의 IP 주소 찾을 수 없음. 다른 DNS 서버에게 물어봐" 라고 응답을 한다.

**6.** 이제 Local DNS 서버는 **naver.com DNS 서버(Authoritative DNS 서버)**에게 다시 "www.naver.com 의 IP 주소" 를 요청한다.

**7.** naver.com DNS 서버 에는 "www.naver.com 의 IP 주소" 가 있다.그래서 Local DNS 서버에게 "www.naver.com에 대한 IP 주소는 222.122.195.6" 라는 응답을 한다.

**8.** 이를 수신한 Local DNS는 www.naver.com 의 IP 주소를 캐싱을 하고 이후 다른 요청이 있을시 응답할 수 있도록 IP 주소 정보를 단말(PC)에 전달해 준다.

9**.** 그 정보를 받은 PC도 IP 주소를 내부 저장소에 캐싱해 두게 된다.

# 4. 라우터

우리는 이제 나의 IP 주소도 가지고 있고 도착지의 IP 주소도 가지고 있다. 위의 설명에서 도착지의 IP 만 알게된다면 거쳐가는 라우터들이 길을 알려주는 형식이라고 했다. 

그 과정에 대해 자세히 알아보자.

  ## 1. 포워딩 테이블

IP : 32비트 2진수로 구성 되어있다. 사람이 읽기 쉽게 만들기 위해, 8비트 씩 끊어서 10진수로 표현하는 경우가 많다. ex) 192.168.2.145

포워딩: 패킷이 라우터에 도착했을 때 적절한 다음 경로를 골라 패킷을 보내주는 것.

라우터는 내부의 Forwarding Table을 보고 패킷을 포워딩해주게 된다. 포워딩 테이블은 다음과 같이 생겼다.

<img width="768" alt="routing table" src="https://user-images.githubusercontent.com/108070719/230247990-be223d24-e492-48ff-a63b-8fefd964618e.png">

이런 포워딩 테이블을 채우는 과정을 라우팅 알고리즘 이라고 한다.

# 5. ARP

이제 내 컴퓨터에서 보낸 http 메세지가 어떻게 구글 서버에 도달할 수 있는지 알게되었다.

하지만, 라우터 간에 실제로 어떻게 통신이 일어나는지 알기위해서는 추상화를 한꺼풀 더 벗겨서 L2 계층을 알아야 한다. L2 계층에서는 라우터가 패킷을 Frame 이라는 단위로 전송하게 되는데, 이때 MAC 주소가 필요하다. 즉, 다음 경로의 라우터의 MAC 주소를 알아야 한다.

그 MAC 주소를 알아내는 방법이 ARP(Address Resolution Protocol) 이다.

ARP 과정

1. 상대의 MAC 주소를 모르기 때문에, 네트워크에 ARP 요청을 브로드캐스트 방식으로 보낸다. 도착 MAC 주소를 (FF:FF:FF:FF:FF:FF) 으로 보내면 브로드 캐스트 방식이다. 도착 IP 주소는 알고 있기 때문에 IP 주소는 적어서 보낸다.
2. ARP 요청을 받은 라우터 중에 자신의 IP 와 일치하는 라우터가 자신의 MAC 주소를 알려준다.
3. 응답을 받은 라우터는 상대의 MAC, IP 주소를 모두 알게되었고 이제는 통신을 보낼 수 있다. 
    
    그리고 자신의 ARP Table에 상대의 MAC 주소를 기록해둔다. 
    

# 6. NAT

이때까지는 NAT 를 배제한 상황을 가정하고 설명을 진행했다. 현실에서는 NAT가 대부분의 네트워크에 적용되어 있으므로 이해할 필요가 있다. 

NAT란?

NAT(Network Address Translation) 는 부족한 IP 주소 문제를 해결해주는 시스템이다.

IP 는 32비트 주소 체계 이므로, 주소는 2의 32제곱 (=대략 42억) 개 이므로 전세계에 존재하는 네트워크 인터페이스 수를 생각해보면 이미 고갈되었어야 했다. 

NAT는 private address 라는 개념을 도입해서 이 문제를 해결하였다. private address 는 한 네트워크 내에서만 사용할 수 있는 IP 주소이다. 

그전에 IP 주소는 전세계에서 유일한 주소라고 하였는데 어떻게 이게 가능할까? 외부에선 private address 를 볼 수 없기 때문이다. DHCP 설명에서 우리는 IP를 할당받을 때 동시에 게이트웨이 라우터 IP 주소도 받는다고 했다. 브라우저가 http 메시지를 생성해서 그 패킷을 게이트웨이 라우터로 보내게 된다. 그럼 라우터는 패킷을 뜯어서 적혀있는 출발지 IP 주소를 자신의 public IP 주소로 변경하고,포트번호에는 임의의 포트번호를 적어서 내보낸다. 이때 이 임의의 포트번호는 원래 IP 주소와 매핑시켜 내부에 저장해 둔다. 그 후 응답메시지가 왔을 때 포트번호를 확인하고 매핑테이블을 체크해서 원래 보낸 클라이언트 컴퓨터에 메시지를 전달하게 된다.


### 출처

성공과 실패를 결정하는 1% 네트워크 ,Tsutomu Tone 저, 성안당.

컴퓨터 네트워크 강의, 한양대학교 교수 이석복 ([http://www.kocw.net/home/cview.do?cid=6b984f376cfb8f70](http://www.kocw.net/home/cview.do?cid=6b984f376cfb8f70))

[ARP 프로토콜 동작순서 (velog.io)](https://velog.io/@ragnarok_code/ARP-Address-Resolution-Protocol#:~:text=ARP%20(Address%20Resolution%20Protocol)%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C,%EC%97%AD%ED%95%A0%EC%9D%84%20%ED%95%98%EB%8A%94%20'%EC%A3%BC%EC%86%8C%20%ED%95%B4%EC%84%9D)

[🌐 DNS 개념 & 동작 ★ 알기 쉽게 정리 (tistory.com)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-DNS-%EA%B0%9C%EB%85%90-%EB%8F%99%EC%9E%91-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4-%E2%98%85-%EC%95%8C%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)
