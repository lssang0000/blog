# REST
결론부터 내리자면 정리가 되는 것 같으면서 많은 부분이 머릿속에 정의되지 않은 느낌이다. 일단 이런 저런 정보들을 나열하고 정리하는 수준의 글을 작성해보았고, 이 분야에서 경험을 쌓으며 지속적으로 돌이켜봐야할 내용일 것 같다.

REST는 Roy T. Fielding의 박사학위 논문 “Architectural Styles and the Design of Network-based Software Architectures” 에서 처음 소개되었다. 

REST는 소프트웨어 아키텍처가 아니라 아키텍처 스타일이라고 한다. REST를 이해하기위한 저 두 단어들이 한번도 생각해본 적 없는 단어이다. 이 개념부터 살펴보자.

## Software Architecture
> 소프트웨어 시스템의 기본 구조와 이러한 구조 및 시스템을 만드는 원칙을 나타낸다. <br/>
> 출처 : [wiki - Software architecture](https://en.wikipedia.org/wiki/Software_architecture)

> A software architecture is an abstraction of the run-time elements of a software system during some phase of its operation. A system may be composed of many levels of abstraction and many phases of operation, each with its own software architecture. <br/>
> 출처 : [[1]](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

> A software architecture is defined by a configuration of architectural elements--components, connectors, and data--constrained in their relationships in order to achieve a desired set of architectural properties. <br/>
> 출처 : [[1]](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

**시스템의 구성(컴포넌트, 컨넥터, 데이터) + 구축 원칙** 정도로 이해하고 넘어간다.

## Architecture Style
> An architectural style is a coordinated set of architectural constraints that restricts the roles/features of architectural elements and the allowed relationships among those elements within any architecture that conforms to that style.
> 출처 : [[1]](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

> 특정 스타일을 따르는 아키텍처가 지켜야하는 제약 조건들의 집합이다.
> 출처 : [[2]](https://blog.npcode.com/2017/03/02/바쁜-개발자들을-위한-rest-논문-요약/)

소프트웨어 아키텍처의 **구축 원칙**에 해당하는 개념인 것 같다.

<br/>

## 다시 REST
Representataional State Transfer는 웹 [아키텍처의 문제들](https://www.ics.uci.edu/~fielding/pubs/dissertation/web_arch_domain.htm)을 해결하기 위해 고안된 **아키텍처 스타일**이다. REST가 설명하는 아키텍처 구축 원칙을 정리하자면 다음과 같다. 

- Client-Server
  - User Interface와 Data-storage의 문제를 분리한다.
  - 여러 플랫폼으로부터의  User Interface 이식성을 개선한다.
  - 서버 구성요소를 단순화 한다. 
- Stateless
  - 클라이언트와 서버의 통신에는 상태가 없어야한다. (상태를 저장할 구조가 필요 없어짐)
  - Reqeust 하나에 서버가 처리하기위해 필요한 정보가 모두 담겨야한다.
  - 상태를 저장할 구조가 필요 없어 규모 확장에 용이하고 작업에 대해 가시성과 (실패시 복원에 의한)신뢰성을 증가시킨다.
- Cache
  - 모든 서버의 응답은 캐싱될 수 있는지 알 수 있어야한다.
- Uniform Interface
  - 시스템을 구성하는 각 요소들 사이의 인터페이스는 균일(Uniform)해야한다.
  - 인터페이스는 다음 네가지 제약 조건을 고려하여 구성한다. (마지막 두개는 감이 안온다.)
    - 리소스 식별
      - identification of resources
      - http의 경우에는 URI로 표현한다.
    - 리소스 조작 방법
      - manipulation of resources through representation
      - http의 경우에는 GET, POST, PUT, DELETE 가 해당된다.
    - 자기 서술 메시지
      - self-descriptive messages
      - 각 메시지는 메시지 자체를 어떻게 처리해야 하는지 정보를 포함하고 있어야한다.
    - 하이퍼미디어
      - hypermedia as the engine of application state  
- Layerd System
  - 계층으로 구성할 수 있어야 한다. 
  - 각 계층에 속한 구성요소는 인접하지 않는 계층의 구성요소를 알 수 없다.
- Code-on-Demnad(Optional)
  - 서버가 네트워크를 통해 클라이언트에 프로그램을 전달하면 클라이언트에서 실행될 수 있어야 한다.
  - Java Applet, Javascript 등
  - 필수 아님

<br/>

## RESTful
**REST 아키텍처 스타일을 준수하는** 정도로 해석하면 되겠다.

## RESTful API

> REST 아키텍처 스타일의 제약을 준수하는 웹서비스의 API를 RESTful API라고 한다.
> 출처 : [wiki - Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)

HTTP에 기빈한(http basted) RESTful은 다음과 같은 요소로 정의된다. (wiki, REST가 꼭 http 기반으로 구현될 필요는 없다는 말인듯)

- base URI
- standard HTTP methods
- a media type that defines state transition data elements.
  - Atom
  - microformats
  - application/vnd.collection+json
  - ...

## 생각 정리
아직 많은 부분을 더 살펴보고 이 글도 보완해야 하겠지만, 필드에서 RESTful API라고 구현한 것들이 REST냐 아니냐 하는 이슈가 있는 것 같다. 

[Spring Core Techonologies](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#spring-core)에 보면 Srping AOP는 Java-ee AOP 요구의 80%를 충족시킨다고 한다. 이런 관점에서, 내가 만든 API가 얼마나 REST의 요건을 충족시키는지 생각해보면 답이 나올 것 같다. Spring F/W가 제공해주는 기능을 통해 구현한 것이라면 Spring F/W가 충족시키는 무엇인지, 내가 추가로 구현한 코드는 어떤 기능을 만족하는지 점검해보는 것이 좋겠다.

<br/>

## Reference
[1] Architectural Styles and the Design of Network-based Software Architectures  
[2] https://blog.npcode.com/2017/03/02/바쁜-개발자들을-위한-rest-논문-요약/  
[3] http://haah.kr/2017/06/12/rest-the-big-lie/  


## [**Back to Blog Home**](../README.md)
