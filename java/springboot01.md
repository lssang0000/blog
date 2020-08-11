> ---
> **NOTE** | 블로그에 작성한 내용은 **의견** 입니다. 레퍼런스가 없는 문장은 참고만 해주세요. 그럼에도 의견과 피드백은 감사합니다.
>
> ---

## Spring Boot 기본 개념
[Spring F/W](spring01.md "[Spring F/W] Spring Framework 기본 개념")가 비기능적 요구사항에 대해 검증된 기술을 제공한다면, Spring Boot는 더 나아가 단독으로 실행 가능(Stand-alone)하고, 제품 수준(Production-grade)의 Spring 기반 애플리케이션을 목표로 진행된 Framework 프로젝트이다. 다시 말해, Spring Boot는 Spring F/W가 기본이 되지만 Tomcat, Jetty, Undertow 등의 3rd-party libary를 이용하여 Data, Web, JDBC, Security 등 Spring의 여러 기술들을 더 쉽게 사용할 수 있게 한 라이브러리 모음이다.

```
Spring Boot = Spring F/W + 3rd-pary library + Spring Boot library
```

## Spring Boot를 사용하는 이유

- 빠른 개발 
  - Spring F/W와 잘 어울리는 3rd-party 라이브러리를 이미 포함
  - DB 접근, 웹 서비스, RESTfull 등의 유틸리티 구성이 어노테이션 하나로 완성됨

- 테스트 
  - 단독 실행 가능한 형태의 Boot App은 로컬 테스트 환경을 구축하기 쉬움
  - JUnit, H2 DB 등을 사용하여 애플리케이션 테스트를 용이하게 할 수 있음

- 유지보수 
  - 비기능 요구사항에 대한 코드가 어노테이션으로 감춰지면서 애플리케이션의 주요 비즈니스 로직이 잘 보임
  - 즉, 어플리케이션 트러블에 대해 문제가 되는 코드를 찾아내기 쉽고 유지보수가 용이해짐

## Spring boot 기본 활용 (작성 중)

- Spring Boot Project 생성
  - https://start.srping.io/ 

- 기본 설정 
  - 포트 설정
  - 로그 레벨 설정

- Spring Boot Life Cycle
  - String/Started Listener
  - Runner

- Property
  - Command line Arguments
  - application.properties
  - Proeprty 우선 순위
  1) 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties 
  2) 테스트에 있는 @TestPropertySource 
  3) @SpringBootTest 애노테이션의 properties 애트리뷰트 
  4) 커맨드 라인 아규먼트 
  5) SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로퍼티)에 들어있는 프로퍼티 
  6) ServletConfig 파라미터 
  7) ServletContext 파라미터 
  8) java:comp/env JNDI 애트리뷰트 
  9) System.getProperties() 자바 시스템 프로퍼티 
  10) OS 환경 변수
  11) RandomValuePropertySource 
  12) JAR 밖에 있는 특정 프로파일용 application properties 
  13) JAR 안에 있는 특정 프로파일용 application properties 
  14) JAR 밖에 있는 application properties 
  15) JAR 안에 있는 application properties 
  16) @PropertySource 
  17) 기본 프로퍼티 (SpringApplication.setDefaultProperties)

- Profile

- Logging
  - 로깅 퍼사드
    - SLF4j
    - Commons Logging
  - 로거
    - Logback
    - Log4j2
    - JUL(java.util.logging)

- Devtools

