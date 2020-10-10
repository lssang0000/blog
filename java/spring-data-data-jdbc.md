(정리중 ... )

Persistence Framework
- JDBC 프로그래밍의 복잡함이나 번거로움을 간단하게 제공
- SQLMapper, ORM ...

SQLMapper
- 직접 SQL문을 작성해 DB에 접근한다.
- 단순히 필드를 매핑시키는 것이 목적
- Mybatis, jdbcTempletes ...

ORM, Object Relational Mapping
- 데이터베이스 객체를 자바 객체로 매핑하여 객체간의 관계를 바탕으로 SQL을 자동 생성 
- 관계형 데이터베이스의 '관계'를 자바 object에 반영
- 메서드 호출을 이용해 직관적으로 데이터를 조작할 수 있다.
- JPA, Hibernate

- 장점
  - 객체 지향적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 도와줌
  - 재사용 및 유지보수의 편리성이 증가한다.
  - DBMS에 대한 종속성이 줄어든다

- 단점
  - 완벽한 ORM으로만 서비스를 구현하기 어렵다
    - 사용하기는 어렵지만 설계는 매우 신중히 해야한다. (어떤 테이블들을 조인할 것인가.. 등)
	- 프로젝트의 복잡성이 커질수록 난이도가 올라간다.
	- 실수는 성능저하 및 일관성을 무너뜨리는 결과를 초래할 수 있다.
  - 프로시저가 많은 시스템에서는 ORM의 객체지향적인 장점을 활용하기 어렵다.
    - 이미 프로시저가 많은 시스템에선 다시 객체로 바꿔야하며, 그 과정에서 생산성 저하나 리스크가 많이 발생할 수 있다.

https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md#orm%EC%9D%B4%EB%9E%80

====


JDBC, java databse connectivity
- db에 접근할 수 있도록 JAVA에서 제공하는 API
- Plain JDBC API의 문제점
  - 쿼리를 실행하기 전과 후에 많은 코드를 작성해야함
  - 데이터베이스 로직 외 많은 예외처리 코드를 수행해야 함
  - 트랜잭션 처리를 해야함
  - 이걸 반복함

JDBC Template
- Spring JDBC 접근 방법 중 하나
- 다음과 같은 기능을 수행함
  - Connection 열고닫기
  - Statement 준비닫기
  - Statement 실행
  - ResultSet Loop
  - Exception 처리와 반환
  - Transaction 처리
- 개발자가 할일
  - datasource 작성
  - sql문 작성
  - 결과 처리

Datasource 
- JDBC 명세의 일부이며 일반화된 연결 팩토리
- DB와 관계된 connection 정보를 담고있음
- bean으로 등록하여 spring-jdbc가 사용

JAP(Java Persistanat API)
- Java ORM에 대한 표준 명세, Java에서 제공하는 API
- ORM을 사용하기위한 표준 인터페이스를 모아둔 것
- 구성요소
  - javax.persistance 패키지에 정의된 API 그 자체
  - JPOL(Java Persistence Query Language)
  - 객체/관계 메타데이터
- 구현체(= ORM Framework)
  - Hibernets, EclipseLink, DataNucleus, OpenJPA, TopLink 

Hibernate
- JPA 구현체중 하나
- 많은 내용이 감싸져있기 때문에 어렵다. 많은 어노테이션들이 의미하는 기능을 알아야한다.
- 잘 이해하지 못하고 사용하면 데이터 손실이 있을 수 있다.

Mybatis
- 개발자가 지정한 SQL, 저장 프로시져, 고급 매핑을 지원하는 SQL Mapper
- Vender 종속성이 있다.

https://gmlwjd9405.github.io/2018/05/15/setting-for-db-programming.html
https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html


Spring data jdbc
- JPA단점
  - JPAs Complexity			// 복잡성
  - Lazy Loading Exception	// 지연 로딩 예외
  - Dirty Checking			// 더러운 검사
  - Session / 1st Level Cache	// 세션, 1단계 캐시
  - Proxies for Entities		// 엔티티에 대한 프록시
  - Map almost anything to anything // 거의 모든걸 매핑
- data-jdbc 설계 디자인
  - No Lazy Loading			// 지연 로딩 없음
  - No Caching				// 캐싱 없음
  - No proxies				// 프록시 없음
  - No deferred flushing	// 지연된 플러싱 없음
  - Very simple & limited & opinionates // ORM	간단하고 제한적인 ORM

https://brunch.co.kr/@springboot/105