# Docker 와 Virtual Machine
원래 제목은 도커 살펴보기, 도커란 무엇인가? 였고, 단순히 도커에 대해 간단히 정리하는 글을 작성하려고 했다. 어떤 계기로 '그럼 Virtual Machine의 장점은 없나? VM은 이제 사라져야하나?'라는 질문에 대해 고민할 기회가 생겼고, 두 기술에 대해 장단점을 조사하고 비교해보는 관점에서 정리하기로 했다.

## Virtual Machine

가상 머신의 정의는 다음과 같다.

> 컴퓨팅 환경을 소프트웨어로 구현한 것, 즉 컴퓨터를 에뮬레이션 하는 소프트웨어 ... 시스템 가상 머신들은(또한 완전한 가상화 가상 머신들으로 알려진) 실제 기계의 대체제를 제공하고 완전한 운영체계의 실행을 위한 요구되는 기능성의 수준을 제공한다. 
> <br/><br/>[출처] Wikipedia

<br/>

가상 머신을 이해하기 위해서는 하이퍼바이저(Hypervisor)를 이해할 필요가 있다.

> 호스트 컴퓨터에서 다수의 운영체제를 동시에 실행히기 위한 논리적 플랫폼을 말한다.
> <br/><br/>[출처] Wikipedia

하이퍼바이저는 일반적으로 2가지로 나뉜다.

- 타입 1 (native 또는 bare-metal)  
운영 체제가 프로그램을 제어하듯이 하이퍼바이저가 해당 하드웨어에서 직접 실행되며 게스트 운영 체제는 하드웨어 위에서 2번째 수준으로 실행된다.

- 타입 2 (hosted)  
하이퍼바이저는 일반 프로그램과 같이 호스트 운영 체제에서 실행되며 VM 내부에서 동작되는 게스트 운영 체제는 하드웨어에서 3번째 수준으로 실행된다

![hyperviseur.png](images/hyperviseur.png)

정리하자면, 가상 머신은 하이퍼바이저라는 VM Manager(또는 Monitor)에 의해 관리되는 컴퓨팅 환경이며 OS(Gust OS)를 포함한다. 가상 머신들은 하드웨어를 공유하고 커널 수준에서 격리된다. 가상 머신은 일반적으로 2가지로 분류된다.

- 시스템 가상머신
  - 가상 하드웨어 구서된 컴퓨터
  - CPU, 메모리, 디스크 등 모든 주변 기기가 가상화
  - 하이퍼바이저 위에서 동작하는 가상 컴퓨터
  - Guest OS가 관할하는 컴퓨터
- 프로세스 가상머신
  - 응용프로그램 가상머신
  - JVM, CLR, Dalvik ...

이처럼 하이퍼바이저와 가상머신의 특징으로 미뤄볼 때 다음과 같은 상황에서 유리하게 사용할 수 있다.

- 동일한 하드웨어(서버 또는 클러스터)에서 서로 다른 목적의 작업 환경을 구축 할 때 
- 동일한 하드웨어(서버 또는 클러스터)에서 서로 다른 업체의 애플리케이션을 제공할 때 보다 견고한 보안 경계를 제공할 때
- 소프트웨어가 실행됨에 따라 CPU, 메모리 등 컴퓨팅 자원 사용 정책을 직접 제어할 필요가 있을 때
- 커널 수준의 시스템 프로그래밍이 필요할 때

## Docker

도커의 정의는 다음과 같다.

> 도커(Docker)는 리눅스의 응용 프로그램들을 소프트웨어 컨테이너 안에 배치시키는 일을 자동화하는 오픈 소스 프로젝트이다.
> <br/><br/> [출처] Wikipedia

<br/>

도커를 이해하기 위해서는 소프트웨어 컨테이너에 대해 이해할 필요가 있다.

- Shipping Container  
IT 용어가 아닌 일반적인 선적 컨테이너의 특징은 선적 컨테이너 회사의 광고 애니메이션에서 나오듯 다양한 운송 수단을 이용해 어디든 배송할 수 있다는 점이다[3]. 

- Software(Linux) Container  
Software Container의 사전적 정의는 애플리케이션을 실제 구동 환경으로부터 추상화할 수 있는 논리적 패키징 메커니즘을 제공하는 운영체제 수준의 가상화이다[4][5]. 

가상화 기술이라고 하니, 자칫 애플리케이션 성능을 떨어뜨릴까 걱정되는 이 기술을 사용하는 큰 이유중 하나는 이 가상화 공간(Software Container)이 앞서 언급한 선적 컨테이너(Shpping Container)가 가지는 특징을 그대로 구현하기 때문이다. 다시 말해, 잘 만들어놓은 Software Conatiner(이하 Container) Image는 사용자 PC, 서버 컴퓨터, IDC 또는 클라우드 가릴 것 없이 어디에든 쉽게 배포할 수 있다.

![](images/container-anywhere.png "그림 출처 : sanghwan.lee")

'어디든 쉽게 배포할 수 있다'라는 특징은 VM도 이미지만 잘 만들어 놓으면 다를바 없을 것 같지만, 도커와 VM의 차이는 도커와 하이퍼바이저가 가상화시키는 영역에서 온다.

<img src="images/docker.png" title="그림 출처 : https://www.docker.com/resources/what-container" width="49%" />
<img src="images/vm.png" title="그림 출처 : https://www.docker.com/resources/what-container" width="49%" />

하이퍼바이저는 하드웨어에서 직접(또는 GuestOS에서) 실행되며 가상화 공간에 Guest OS를 설치한다. 때문에 커널 수준의 프로그래밍이나 시스템 리소스 관리 측면에서 유리하지만 하나의 가상화 공간의 크기가 매우 크다. 반면, 도커가 관리하는 가상화 공간들은 Host OS의 커널 및 시스템 자원을 공유하며, 오로지 애플리케이션을 위한 바이너리와 라이브러리, 설정 파일을 격리한다. 따라서 VM에 비해 훨씬 작고 이는 클라우드 등 다양한 환경에 배포할 때, 시간, 네트워크 트레픽 등 다양한 측면에서 유리하게 작용한다.

가상화 공간에 GustOS를 포함하지 않는다는 것은 애플리케이션 변경에서도 유리하다. VM을 사용하는 환경에서 변경된 애플리케이션을 배포하기 위해서는 VM에 접속하여 애플리케이션 바이너리 저장소에서 바이너리를 다운로드하고 변경된 설정을 적용하기위해 Jenkins를 구성하는 등의 작업을 수행하거나, 이와같은 작업이 완료된 VM을 구축하여 새롭게 배포해야한다(이 경우엔 심지어 GuestOS가 재부팅되는 시간동안 Downtime이 발생할 것이다).

반면 도커를 사용한다면 이미지 생성을 위한 Dockerfile을 수정하여 다시 배포하거나, 컨테이너에 마운트된 볼륨에서 몇 가지 파일을 변경하는 것 만으로도 애플리케이션 변경을 수행할 수 있다. 물론 애플리케이션이 재시작되는 동안 Downtime이 발생할 수 있지만 VM에 비해서 매우 짧은 시간일 것이다. 최근 Kubernets 등 컨테이너 오케스트레이션 솔루션들이 많이 등장하여 이 단점 또한 없어지는 추세이다.

이처럼 도커는 다음과 같은 상황에서 유리하게 사용할 수 있다.

- 릴리즈 등 애플리케이션 배포가 자주 일어나는 환경
- 다양한 환경에 동일한 애플리케이션을 실행해야할 때

## Reference
[1] [Waht is a Container](https://www.docker.com/resources/what-container#/package_software)  
[2] [컨테이너와 가상머신 비교](https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/about/containers-vs-vm)  
[3] [ITS ConGlobal](https://www.youtube.com/watch?v=vqmlM7TIvXo)  
[4] [Software Container](https://cloud.google.com/containers?hl=ko)  
[5] [운영 체제 수준 가상화 - Wiki](https://ko.wikipedia.org/wiki/%EC%9A%B4%EC%98%81_%EC%B2%B4%EC%A0%9C_%EC%88%98%EC%A4%80_%EA%B0%80%EC%83%81%ED%99%94)  
[6] [Docker와 VM](https://medium.com/@darkrasid/docker%EC%99%80-vm-d95d60e56fdd)

## [**Back to Blog Home**](../README.md)