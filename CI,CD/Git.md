## **Git 과 GitHub 의 차이는 뭘까?**

우선 Git 은 분산버전관리시스템으로, 파일 그 자체를 저장하기보다는 그 **변경사항을 추적**하는것에 집중된 시스템이다

그렇다면 깃허브는?

유튜브 노마드코더님의 비유에 따르면 깃은 커피이고, 깃허브는 커피샵이라고 한다

깃허브는 클라우드 서버를 사용하여 여러 사용자가 작업할 수 있도록,  
Git 을 사용하는 프로젝트를 지원하는 웹 호스팅 서비스다

<br>

## **Git 동작원리**

간단하게 git 의 4가지 구성을 말하라고 하면,

**Working Directory**  
**Staging Area**  
**Local Repository**  
**Remote Repository (github)**

로 나눌 수 있을 것이다.

이 4가지의 관계도를 그림으로 나타내보자면 아래와 같다

![image](https://user-images.githubusercontent.com/96935132/234571143-03213e0a-f870-4f9c-87ee-e344c5d63b86.png)

우리에게 너무 익숙한 명령어들이 눈에 보일 것이다

습관적으로 git init, add, commit, push, pull, commit 다시 push.. 하지만  
이 과정이 어떻게 돌아가는지, 내가 작업한 파일의 변경사항을 파악하는건지,  
github 로 어떻게 연동되는지 궁금하지 않은가?

우선

<br>

### **Working Directory**

말 그대로 내가 지금 열어서 편집하고있는 폴더에 존재하는 파일들! 그 자체!를 의미한다

작업폴더를 열어보면 , 그리고 git 초기화를 해두었다면 .git 폴더가 있을텐데

그 폴더를 제외한 모든 영역이 Working directory 다

<br>

### **Staging Area (Index)**

이 영역은 커밋이 될 준비를 하는 파일들의 정보가 위치하는 곳이다

즉, 우리가 git add 명령어를 수행한 파일들에 대한 정보가 위치하는 곳이다

이 파일들에 대한 정보는, 파일들이 들어가는게 아니라 말그대로 정보, 즉 **해당 파일의 내용을 담고있는 blob 파일의 주소주소와 파일명**이 기록되는데

![image](https://user-images.githubusercontent.com/96935132/234571742-33d7b419-e4fd-4e31-aabd-199a1d8c4ecd.png)

.git 폴더 내부의 index 파일 안에 적혀있다

blob 파일의 주소? blob ? 파일명이면 hello.java 처럼 utf-8 형태로 적힌건가? 라고 할 수 있지만~

당연히 아니고 git 에서는 **Hashing Function** **(SHA - 1)** 이라는 것을 이용하여 파일, 디렉토리, 커밋 등을 저장한다

이는 아래에 설명할 tree 파일과 유사한 구조를 보인다

따라서 이 내부에 적힌 값들에 대한 설명은 아래를 참고하자

![image](https://user-images.githubusercontent.com/96935132/234571956-3508962c-26fe-43e9-99e8-4ec1c57f53c2.png)

당연히도! git folder 내부에선 해시 중복이 일어날 확률은 0에 수렴하니, 중복 우려를 하지 않아도된다

<br>

### **Local Repository (.git/objects)**

Staging area 를 마저 설명하기 위해서는 blob 이 뭔질 알아야하고, blob 이 뭔질 알려면 **objects 폴더 내부**로 들어가야한다

여기서 Object 파일 내부가 Local repository 를 의미한다

![image](https://user-images.githubusercontent.com/96935132/234572079-815766f3-2798-41dc-a205-07405b2e762c.png)

이 폴더 내부에 저장되는 파일들은
![image](https://user-images.githubusercontent.com/96935132/234572141-65d0a952-b136-49c3-8765-5faac50a5dd3.png)

위와 같은 구조를 보이는데,

Blob, Tree, Commit 순으로 알아보자.

먼저 위에도 언급된   **Blob 파일 !**

파일 각각의 데이터 자체만은 Git 에서 이 Blob 파일의 형태로 저장된다

이 파일은 **데이터 자체만을 저장**하고 파일명과 같은 메타데이터들은 저장되지않아서

동일한 코드를 가진 파일이 여러개 있더라도 하나의 blob 파일만 생성된다

<br>

다음은   **Tree 파일 !**

이 tree 파일에는 파일 식별자, 파일 데이터의 해시값, 파일의 이름이 저장된다

여기서 파일 식별자는 총 3가지로,

읽기파일(blob)(100644), 실행파일(blob)(100755), 디렉토리(tree)(040000) 이다

파일을 살펴보면 더 잘 이해가 될텐데,

아래의 파일은 읽기파일 a, b, c / 실행파일 d / 그리고 dir 폴더를 가지고있다

![image](https://user-images.githubusercontent.com/96935132/234572216-2835ca7d-b4f0-4835-a4ff-ed000f377ca5.png)

노란 박스가 파일 식별자 부분, 파란 박스가 파일 데이터의 해시값, 주황박스가 파일 이름을 나타낸다

파란박스를 보면 a, b, c, d 파일의 데이터 해시값이 전부 일치하는 것을 확인할 수 있는데

그 이유는 실제 4개 파일의 데이터 내용이 모두 동일하기 때문이다

또한 dir 에서 볼 수 있듯, **tree 에서는 파일의 폴더 구조, 디렉토리 정보들을 확인할 수 있다는게 큰 특징**이다

<br>

다음은 **Commit 파일 !**

말그대로 commit 메시지 등의 commit 기본 정보가 적혀있는 파일이다

commit 파일은 commit 관련 메타데이터 포함 추가 두가지도기록하는데,

첫째 하나의 tree 파일의 주소,

둘째 직전 commit 파일의 주소 이다

백문이 불여일견이니, 직접 프로젝트를 진행한 폴더 내부에서 git 명령어로 commit 부터 tree, blob 파일 내부까지 알아보자

우선 최근 커밋 기록부터 확인하자

![image](https://user-images.githubusercontent.com/96935132/234572345-ae3c66b0-4f2a-4979-b054-8321c7db91c0.png)

위에 commit 하고 적인 a3a7... 이 부분이 commit 의 해싱된 SHA-1 값이다

<br>

**여기서 잠깐!**

objects 폴더 안에는 두글자로 된 폴더명이 있고 그 안에 38글자의 파일명을 가진 파일이 있는데,

이 폴더명과 파일명을 정하는 것이 바로 이 commit 해싱 값이다

즉, 위의 예에서 보면

우리는 object 폴더 내에 a3 라는 폴더 내에 a7ab69.... 라는 파일명의 commit 파일이 생성됐음을 확인할 수 있다는 뜻이다

![image](https://user-images.githubusercontent.com/96935132/234572502-b999e478-d0ec-4185-9fd6-71a54d880cf7.png)

<br>

이어서 진행해보자

**이 commit 파일 안에 있는 내용을 확인**해볼건데,

아까 공부했던 내용에 따르면 tree 파일의 주소와, parent 주소, 그리고 commit 관련 메타데이터 정보가 있어야한다

![image](https://user-images.githubusercontent.com/96935132/234572563-5b5ed1fe-fdcf-42e2-8600-72df7a3ed5e6.png)

예상했던 대로, tree 파일을 해싱한 값과 부모 commit 파일의 해싱값, 그리고 author 와 커밋한 사람이 누군지, 그리고 commit 메시지에 대한 정보가 나온다

<br>

**그럼 이제 tree 파일을 확인해보자**

![image](https://user-images.githubusercontent.com/96935132/234572616-a4dfac48-a0ee-42f3-8cd8-c4ea6c3b4828.png)

이 파일 내부를 살펴보니,

위에서 언급한대로 각각의 파일 식별자, 파일 데이터의 해시값, 그리고 파일명(혹은 폴더명) 의 구조를 보이는 것을 확인할 수 있다

정말 마지막으로 이제 **blob 파일** 중 하나인 pom.xml 을 살펴보겠다

파일 데이터의 해시값으로 cat 해오면

![image](https://user-images.githubusercontent.com/96935132/234572731-72b817a9-b599-4b36-8ae5-87caa9608c3c.png)

짜란 ~!

드디어 내가 수정한 파일의 정보가 보인다!!

여기까지 해서 Staging Area 와 Local Repository 에 대한 설명이 끝났다 !! 👏👏

이제 내 컴퓨터 내부 repository 에 전부 커밋이 끝났으니 원격저장소로 보내기만 하면된다!

<br>

### **Remote Repository (github)**

원격 저장소, 즉 github 는 Working Directory 와 Index 가 없고 Repository 만 존재하는 원격 서버이다

<br>

### **그 외의 .git 파일 내부엔?**

![image](https://user-images.githubusercontent.com/96935132/234572803-73c8ee3f-1383-4f33-b756-c6306834351f.png)

우리가 앞서 살펴봤던 index 와 objects 폴더 내부 외에 다음과 같은 폴더, 파일들이 존재한다

중요한 것만 살펴볼텐데,

그 전에 생각해볼점! 내가 이렇게 커밋 기록과, 변경 사항 등등을 다 저장해뒀는데 내가 지금 현재! 마지막으로 commit 한 파일은 무엇이고, 무슨 브랜치에서 일을 하고있고 이런걸 매번 확인을 할까?

<br>

**아니다**

우리는 **HEAD 와 브랜치 포인터**라는 것을 사용하여 이 일을 수행한다

HEAD 는 우선, 현재 브랜치의 포인터를 가리키는 포인터이고 .git/HEAD 에 위치해있다

브랜치포인터는 브랜치 마다 가장 최신 commit 파일을 가리키는 포인터로

.git/refs/heads/{브랜치명} 에 위치해 있다

우리는 이 HEAD 와 브랜치포인터를 사용하여 포인터가 가리키는 곳을 변경하는 동작을 통해

git reset 이나 checkout, merge 등의 명령을 수행할 수 있다

<br>

## **Git 의 브랜치 전략**

Git 에서는 동시에 다양한 작업을 할 수 있도록 브랜치라는 작업공간을 제공한다

이 브랜치는 기존 코드의 복사본을 만들어 새로운 작업을 진행할 수 있게하는데,

개발자 각자가 각각의 브랜치에서 작업을 하고 그 변경사항을 저장소에 merge 하며 협업이 이루어진다

하지만,

만약 브랜치가 중구난방이라면 어떨까?

여러명이 협업을 하는 상황에선, 어떤 브랜치가 어디까지 개발이 완료되었는지, 혹은 이미 개발이 완료된 브랜치인지

하나하나 들어가보지않으면 알 수 없다

이러한 상황을 피하기 위해 **규칙을 만들자! 라고 한 것이 브랜치 전략이다**

**그럼 가장 유명하고 많이 사용되는 2가지 브랜치 전략에 대해 알아보자.**

<br>

### **Github Flow 전략**

Github 에서 만든 단순한 구조의 브랜치 전략으로, 지속적으로 테스트 및 배포를 하는 경우 많이 사용된다

![image](https://user-images.githubusercontent.com/96935132/234572938-08ff1746-a94a-487b-8b83-1f71c3cee888.png)

위의 사진을 보면 알 수 있듯이, master 브랜치를 중심으로 작업용 브랜치가 생겨났다가 merge 되는 구조이다

이 master 브랜치는 딱봐도 중요해보이는데, 맞다.

**stable 상태로 배포를 담당하는 브랜치이다.**

따라서,

개발, bug 수정 그 어떠한 이유로든 코드를 수정할 일이 생기면 **새로운 branch** 를 생성한다

이렇게 생성된 branch 에서 개발을 진행하고, pr을 보내 code review 도 하고 검토도 하고 기타 등등 배포하기 전까지 할 수 있는 모든 테스트를 다한다

심지어, github 에서는 브랜치에서 코드를 미리 배포해볼 수 있는 기능이 있어서 테스트가 더 용이하다! 👀

이 과정을 모두 거친 후에서야 master 에 merge 되어 배포된다

<br>

다음은,

### **Git flow 전략**

많은 기업에서 표준으로 사용중인 브랜치 전략이다

이 전략에서는 위의 github 전략과 다르게 브랜치를 5개로 쪼갠다

![image](https://user-images.githubusercontent.com/96935132/234573071-718f983a-8dc7-48d0-a5f1-beb6889b0a66.png)

**하나하나 branch 들을 살펴보자**

**이 branch 들은 크게 메인브랜치(2개)와 보조 브랜치(3개)로 나눈다**

<br>

**1\. 메인 브랜치**

이 브랜치들은 **항상 존재한다! 없어지지 않는다!**

2가지 브랜치가 있는데

**1) master :** 배포가 가능한 브랜치

**2) develop** : 다음 버전 개발을 위한 브랜치

이다.

master 브랜치가 하는 일은, github 브랜치전략에서의 master 브랜치와 일치하고,

develop 브랜치는 말그대로, 새로운 버전 배포를 앞서 개발의 서두를 여는 브랜치이다

이 브랜치로부터 비롯되어 그 버전에 포함될 feature 들의 각각 개발에 따른 branch 가 분기된다

그 브랜치들은 보조 브랜치이므로 이제 보조 브랜치를 살펴보자

<br>

**2\. 보조 브랜치**

이 브랜치들은 제 할 일을 마치면 **브랜치가 삭제된다!** 계속 살아있는 메인브랜치와의 차이다

3가지 브랜치가 있는데

**1\. feature 브랜치 :** 각 기능 구현

feature 브랜치는 새로운 버전 배포에 들어갈 기능들 각각마다 생성되는데,

develop 브랜치에서 분기되어 기능 개발을 마치면 develop 브랜치에 merge 된 후 삭제된다

**2\. release 브랜치 :** 배포 테스트 담당

develop 브랜치에서 분기되어 테스트를 마치면 develop 브랜치와 master 브랜치에 merge 된 후 삭제된다

쉽게 생각하자면 배포 전 마지막 테스트를 담당하는 브랜치이다

성공하면 그대로 배포로 가는거고, 아니면.. 다시 개발하는거다

**3\. hotfix 브랜치 :** 버그 수정 담당

master 브랜치, 즉 배포 버전에서 긴급하게 오류가 발생했을 때 사용하는 브랜치이다

따라서 이 브랜치는 master 브랜치에서 분기되어 오류가 수정된 후에 develop 브랜치와 master 브랜치에 merge 되는 형태이다.

<br>

**\## 참고 ##**

[https://tecoble.techcourse.co.kr/post/2021-07-08-dot-git/](https://tecoble.techcourse.co.kr/post/2021-07-08-dot-git/)

.git 내부 구조 파헤치기

[https://wikidocs.net/17166](https://wikidocs.net/17166)

.git 내의 파일 분석

[https://kotlinworld.com/300](https://kotlinworld.com/300)

Git은 어떻게 파일을 저장하고 복구하는가?

[https://it-eldorado.tistory.com/4](https://it-eldorado.tistory.com/4)

내부 동작 원리에 대한 이해

[https://coding-groot.tistory.com/68](https://coding-groot.tistory.com/68)

Git Internals 정리 :: Git은 어떻게 동작할까?

[https://opentutorials.org/module/2676/15212](https://opentutorials.org/module/2676/15212)

git의 원리 - 지옥에서 온 Git
