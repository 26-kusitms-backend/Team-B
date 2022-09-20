# 큐시즘 스프링 스터디 2 주차

날짜: 2022년 9월 17일

### 

### ❓ORM은 무엇인가

ORM = Object Relational Mapping

객체와 관계형 데이터베이스의 테이블을  연결해주는 것을 뜻한다.

**즉 ORM을 이용하면 query가 아닌 메서드로 데이터를 조작 가능하다.**

ORM의 장점

- 선언문, 할당문등 부수적인 코드를 급격히 줄여준다.
- 객체지향적인 접근으로인해 생산성이 증가한다.
- 독립적으로 작성되어 해당 객체들을 재활용 할 수 있다. —> 견고한 디자인패턴
- 객체와 관계를 바탕으로 SQL을 자동생성해서 RDBMS와 객체지향 모델 사이의 간격을 좁힐 수 있다.
- DBMS를 교체할 때 적은 리스크와 시간을 소모한다.

ORM의 단점

- 편리한 사용에 비해 설계가 어렵다.
- 복잡성이 증가할경우 난이도가 많이 증가한다.
- 프로시저, 생상성저하, 튜닝등 여러가지 리스크가 있다.

### ❓스프링에서 대표적으로 사용하는 ORM은 무엇이 있고, 
각각의 특징은 무엇이 있는가

JPA = Java Persistent API

JPA는 자바 ORM 기술에 대한 API 표준 명세를 모아둔 인터페이스다.

애플리케이션과 JDBC사이에서 동작하며 SQL을 호출해 DB와 통신한다.

따라서 JPA를 사용하기 위해서 JPA를 구현한 ORM framework를 사용해야 한다.

스프링에서도 Spring Data JPA의 provider로 ORM framework를 필요로 한다.

**Spring Data JPA** 

spring에서 JPA를 편리하게 사용하도록 도움을 주는 모듈

**ORM framework**

ORM framework는 Hibernate가 JPA를 주도하고 있다고 해도 과언이 아니다.

(사실상 표준)  ~~JPA = Hibernate라 부르는 사람도 많다.~~

~~그 외에도 DataNucleus, EclipseLink가 있지만 들어본적도 없는..~~

### **Hibernate 특징**

장점

- SQL대신 메서드 호출로 쿼리를 수행하기 때문에 **높은 생산성**
- 테이블 수정시 JPA가 대신 해주기 때문에 Mybatis보다 **편리하다**
- 특정 DBMS에 종속적이지 않다. (ORM의 장점)

단점

- 복잡한 동적 쿼리를 구현하기 힘들다 → JPQL지원
- 복잡성이 증가할수록 성능이 안좋아진다.

### ❓JPA/ MyBatis를 사용해봤다면, 그 이유와 경험담 소개 ( 개인의 경험 공유 )

**Mybatis**

마이바티스는 처음 시작할 때 다들 사용한다해서 사용해봤다. sql을 그대로 사용할 수 있다.

**JPA(Hibernate)**

김영한님 JPA강의를 보며 사용해 봤다. 데이터를 객체화 시키고 어노테이션과 메서드를 통해 CRUD는 정말 간편하게 사용했던 기억이 있다.

사실 둘 다 찍먹 수준이라서 큰 불편함 또는 벽을 느끼지 못했다.

장단점에서 알 수 있듯이 간단한 쿼리는 JPA의 압승이라고 생각한다.

스프링에서도 JPA를 권장하는만큼 JPA 공부를 열심히 해보자

SQL을 잘할수록 JPA를 잘쓴다고한다. 그냥 둘 다 열심히 하자

### ❓영속성(Persistence)이란 무엇인가

데이터를 생성한 프로그램을 종료해도 데이터가 사라지지 않는 특성.

메모리의 데이터를 데이터베이스를 활용하여 영구적으로 저장하면 영속성이 부여된다.

**스프링에는 영속성을 부여하는 방법**

1. JDBC
2. Spring JDBC
3. Persistence Framework (Hibernate, Mybatis ..)
    
    persistance framework는 SQL Mapper와 ORM으로 나눠진다.
    

### ❓영속성 컨텍스트와 그 특징은 무엇이 있는가

애플리케이션과 DB 사이에서 객체를 영구 보관하는 가상의 DB 같은 저장환경

EntityManager를 통해 엔티티를 저장하거나 조회하면 EM이 영속성 컨텍스트에 entity를 보관하고 관리한다.

특징

- 서비스별로 EntityManager Factory가 존재하며 발생하는 트랜잭션별로 EM을 생성하여 persist context에 접근한다.
- persist context는 EM 생성마다 하나씩 만들어진다.
- EM을 통해서 persist context를 접근/관리 할 수 있다.

스프링에서는 EM을 주입하여 같은 트랜잭션 범위에 있는 EM은 같은 persist context에 접근한다.

**EntityManager**

EM은 persist context내에서 Entity들을 관리한다.

EM은 JPA에 제공하는 인터페이스로 spring bean 에 등록되어있어 
@Autowired 어노테이션으로 사용 가능하다.

```java
@Autowired
private EntityManager em;
```

**Entity 생명주기**

- 비영속(new/transient) 
객체를 생성하고 영속성 컨텍스트에 저장하지 않은 상태 / 전혀 관계 없음
- 영속(managed)
    
    영속성 컨텍스트에 저장된 상태 / 커밋 시점에 db에 쿼리로 날리게 된다.
    
- 준영속(detached)
    
    영속성 컨텍스트였다가 분리된 상태 / em.detach()로 호출
    
- 삭제(removed)
    
    영속성 컨텍스트와 db에서 해당 엔티티를 삭제한 상태.
    

### ❓트랜잭션은 무엇인가

transaction이란 DB의 상태를 변화 시키기 위해 수행하는 작업의 단위를 뜻한다.

**transaction의 특징**

- Atomicity 
트랜잭션이 모두 데이터에 반영되던가 모두 반영되지 않던가 해야한다.
- Consistency
트랜잭션이 성공적으로 완료되면 언제나 일관성 있는 DB상태로 변환한다.
- Isolation
트랜잭션이 실행되는중 다른 트랜잭션은 끼어들 수 없다.
- Durability
트랜잭션이 성공적으로 완료된경우 결과는 영구적으로 반영된다.

**스프링 에서 트랜잭션이란?**

@Transactional 어노테이션으로 간편하게 사용 가능하다.

이 어노테이션은 begin, commit 기능을 수행해주고, 트랜잭션이 실패하면 rollback해준다.

Service/Repository 계층에서는 영속성 컨텍스트에 관리(managed)되지만, Controller/View에서는 준영속상태(detached)가 된다.

detached 상태에서는 Lazy Loading을 사용 할 수 없다.

**지연로딩 (Lazy Loading)**

A 객체와 연관관계가 있는 B 객체가 있을 때 A 객체의 정보만 필요한데 연관관계 떄문에 B 객체가 딸려오게 된다면 비효율 적이기 때문에 프록시(가짜) 객체를 만들어 조회하고, 필요할 때 B객체를 조회한다.

### ❓JPQL / queryDSL

Hibernate의 단점으로 복잡한 쿼리를  구현하기 힘들다.

이를 JPQL과 QueryDSL로 해결할 수 있다.

**JPQL**

JPA의 일부로 객체지향 쿼리언어라고 정의할 수 있다. 
**테이블이 아닌 객체가 대상이다**

JQPL은 String형태기 때문에 개발자 의존적이다. 따라서 컴파일 단계에서 type-check가 불가능하다. 

**QueryDSL**

JPQL의 단점을 보완하기위해 나온것인 QueryDSL이다.

쿼리 내용이 모두 함수로 되어있기 떄문에 컴파일 단계에서 에러를 찾을 수 있다.

**사용처**

JPQL은 @Query 어노테이션으로 바로 작성이 가능하기 때문에 간편하다.

QueryDSL은 JPA와 함께 사용하면 Custom Repository를 넣어줘야한다.

따라서 단순한 쿼리면 JPQL을 사용하고 복잡한 쿼리 또는 동적쿼리라면 QueryDSL을 사용한다.