# 2주차 스프링 스터디

## 💡 ORM이란 무엇인가?

- Object-Relational-Mapping(객체관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터 베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

## 💡 JPA?

- 애플리케이션과 JDBC사이에서 동작
- entity분석,Insert API사용,Result Set매핑 **(쿼리문에 따라서 달라진다)**
- 패러다임 불일치 해결(객체지향과&관계형 데이터베이스)
- 인터페이스의 모음
- Hibernate를 추상화한 인터페이스

## 💡 JPA는 어떻게 객체지향과 관계형 데이터베이스의 패러다임 불일치를 해결하는가?

- JPA와 상속
    - 저장

    ```
    // 개발자가 하는 일
    jpa.persist(album)
    
    // JPA가 처리하는 부분
    INSERT INTO ITEM
    INSERT INTO ALBUM 
    ```

- JPA와 연관관계
- JPA와 객체 그래프 탐색

```
// 연관관계 저장
member.setTeam(team);
jpa.persist(member);
// 객체 그래프 탐색
Member member = jpa.find(Member.class,memberId);
Team team = member.getTeam();
```

- JPA와 비교

→ JPA는 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장 (자바 컬렉션)

```
String memberID="100";
Member member1=jpa.find(Member.class,memberId); //SQL
Member member2=jpa.find(Member.class,memberId); //캐시
member1 == member2;
```

## 💡 Hibernate?

- 자바 언어를 위한 ORM 프레임워크
- JPA의 구현체로, JPA 인터페이스 구현하며 내부적으로는 JDBC API를 사용.

[https://livenow14.tistory.com/70](https://livenow14.tistory.com/70)

## 💡 MyBatis

자바 SQL Mapper를 지원해주는 프레임워크이다.(DTO와 엔티티간의 매핑 , DTO에서 엔티티에 바로 연결할 수 없기 때문)

SQL문을 이용해서 RDB에 접근하고, 데이터를 객체화 시켜준다.

SQL을 직접 작성하여 쿼리 수행결과를 객체와 매핑한다.

객체와 쿼리문을 모두 관리해야하고, CRUD메소드를 직접 다 구현해야한다.

## 💡JPA/ MyBatis를 사용해봤다면, 그 이유와 경험담 소개 ( 개인의 경험 공유 )

JPA만 찍먹해봤다.

왜 JPA를 사용해야하는가에 대해 생각해보면 좋을 것 같아 정리해보았다.

1. **생산성**

   CRUD를 손쉽게 구현가능하다.

   저장: jpa.persist(member)

   조회: jpa.find(memberID)

   수정:member.setName(”변경할 이름”)

   삭제:jpa.remove(member)

2. **유지보수성**

   만약 데이터를 추가할 때 SQL을 열어서 모든 코드를 수정해야한다. DAO를 통해 SQL을 숨겨도 어쩔 수 없이 SQL을 열어야 하기에 엔티티를 신뢰할 수 없다.

3. **패러다임의 불일치 해결**

1. **성능**

   애플리케이션과 DB 사이에서 다양한 성능 최적화 기회 제공

    - 1차 캐시와 동일성 보장

      → 같은 트랜잭션 안에서 같은 엔티티 반환함으로써 약간의 조회 성능 향상

    - 트랜잭션을 지원하는 쓰기 지연

      → 트랜잭션 커밋할 때까지 INSERT SQL모음

      → JDBC BATCH SQL기능을 사용해서 한번에 SQL 전송

        ```
        transaction.begin(); // [트랜잭션] 시작
        em.persist(memberA);
        em.persist(memberB);
        em.persist(memberC);
        // 여기까지 INSERT SQL을 DB에 보내지 않음
        
        //커밋하는 순간 DB에 INSERT SQL을 모아서 보냄
        transaction.commit(); // [트랜잭션] 커밋
        ```

    - 지연로딩
        - 지연로딩: 객체가 실제 사용될 때 로딩

      (멤버 쓸 때 team을 안쓰고 어쩌다 쓰면 지연로딩이 더 유리함)

      엔티티를 쓸 때  가져옴

        ```sql
        SELECT * FROM MEMBER
        SELECT * FROM TEAM
        ```

        - 즉시로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회

          → Member를 가져오는 순간 Team을 같이 가져오는 경우

            ```sql
            SELECT M.*,T.*
            FROM MEMBER
            JOIN TEAM 
            ```


        즉시로딩은 join x 

        따로따로 여러개 날려서 문제인 것

2. **데이터 접근 추상화와 벤더 독립성**

JPA는 애플리케이션과 DB 사이에 추상화된 데이터 접근 계층 제공해서 특정 데이터베이스 기술에 종속되지 않도록 함

## 💡 영속성이란 무엇인가

데이터를 생성한 프로그램의 실행이 종료되더라도 사라지지 않는 데이터의 특성

데이터가 영속성을 가지기 위해 3가지 특성이 있는데

1. JDBC
2. Spring JDBC
3. Persistence Framework (JDBC를 사용하기 위한 복잡하고 번거로운 작업 없이 DB에 연동할 수 있게 하는 프레임워크 Persistence Framework에는 ORM과 SQL Mapper가 있다)

## 💡영속성 컨텍스트와 그 특징은 무엇이 있는가

[https://velog.io/@neptunes032/JPA-영속성-컨텍스트란](https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80)

영속성 컨텍스트란 엔티티를 영구저장하는 환경을 의미한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/555ba2b3-7443-4f78-8eba-a15807185027/Untitled.png)

엔티티 매니저와 영속성 컨텍스트는 1:1관계를 맺고 있다.

엔티티의 생명주기는 4가지로 분류될 수 있는데

| 비영속 | 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태를 의미한다.  |
| --- | --- |
| 영속 | 영속성 컨텍스트에 의해 관리되는 상태를 의미한다. |
| 준영속 | 영속성 컨텍스트에 저장되었다가 분리된 상태를 의미한다.  |
| 삭제 | 삭제된 상태를 의미한다. |

```
// 비영속
Member member = new Member();
member.setID("member1");
memeber.setUsername("회원1");
//영속 
EntityManange em=emf.createEntityManager();
em.getTransaction.begin();
//객체 저장(영속)
em.persist(member);
```

영속이라고 해서 쿼리가 날아가는게 아니고 트랜잭션이 실행됐을 때 실행된다.

### 영속성 컨텍스트의 이점

1. 1차캐시 (영속성 컨텍스트 내부에는 엔티티 보관하는 저장소가 있는데 이를 1차캐시라 함 → 트랜잭션 시작하고 종료할 때만 1차 캐시 유효)

[https://willseungh0.tistory.com/77](https://willseungh0.tistory.com/77)

1. 동일성 보장 → 동일성 비교 가능(==)
2. 쓰기 지연 (트랜잭션 커밋전까지 SQL 바로 보내지 않고 모아서 가능)
3. 변경 감지 → 커밋되는 시점에 Entity와 스냅샷 비교해서 updateSQL 생성
4. Lazy 로딩 → 해당 엔티티 불러올 때 그 때 SQL 날려 해당 데이터 가져옴

(@Transactional  붙이면 해결됨)

## 💡트랜잭션은 무엇인가

데이터베이스에서 트랜잭션이란 클라이언트의 요청 시 시스템이 응답하기 위한 상태 변환과정의 작업단위이다. 하나의 트랜잭션은 Commit되거나 Rollback된다.

JPA의 모든 데이터 저장,수정에 @Transactional을 사용한다. 그래야 지연로딩이 된다.

Repository나 Service 계층에서 사용되고, Runtime Exception이나 Error가 발생하면 해당 트랜잭션을 롤백한다.

트랜잭션을 시작하면 영속성 컨텍스트를 생성하고, 트랜잭션이 끝날 때 영속성 컨텍스트를 종료한다.

트랜잭션이 같으면 같은 영속성 컨택스트를 사용하고, 트랜잭션이 다르면 다른 영속성 컨텍스트를 사용한다.

### 📌 생각해볼 것

1. 실무에서는 @ManyToMany를 쓰면 안된다는데 왜 그런건지? → 관계 테이블을 만들어주면 상관 없음
2. 연관관계에서  fetchType을 Lazy로 설정해야하는 이유
3. JPA에서 엔티티 객체에 기본 생성자가 필요한 이유 (Reflection API와 연관지어서) → 스프링부트와 JPA 노션

### 📌 진짜 궁금한거

1. DTO에 Res는 생성자 만들어주는데 Req은 생성자 왜 안만들어주는지?(Res는 생성해서 올려줘야 하지만, Req는 어디에 보내주는게 아님)


## JPA의 문제점은?

JPA는 고정된 형태의 값을 가진다는 단점이 있다.

복잡한 조합을 이용하는 경우의 수가 많은 상황에서는 **동적으로 쿼리를 생성**해서 처리할 수 있는 기능이 필요

## JPQL과의 차이점

JPQL은 문자로 작성되면 파라미터 바인딩을 직접해줘야하는데 QueryDSL은 쿼리 메서드와 쿼리 타입을 통해 자바코드로 작성되며 파라미터 바인딩도 자동으로 처리한다.

### 💡JPQL

```
public void startJPQL() {
        // memeber1을 찾아라
        Member result = em.createQuery(
                "select m from Member m " + 
                "where m.username = :username", Member.class)
                .setParameter("username", "member1")
                .getSingleResult();
    }
출처: https://ykh6242.tistory.com/107 [경호의 공부방:티스토리]
```

→ 그리고 빡치는게 string이여서 내가 잘못쓰면 IDE가 에러도 안잡아줌,,,

### 💡QueryDSL

```
public void startQueryDsl() {
        // memeber1을 찾아라
        Member result = queryFactory
                .select(member)
                .from(member)
                .where(member.username.eq("member1"))
                .fetchOne();
    }
출처: https://ykh6242.tistory.com/107 [경호의 공부방:티스토리]
```

## QueryDSL이 제공하는 기본적인 쿼리 메소드

select()

from()

selectFrom() → select하는 엔티티와 from의 엔티티가 일치할 경우, 합칠 수 있다.

where()

update()

set()

delete()

## QueryDSL 기본문법 정리해놓은 글들

[https://ykh6242.tistory.com/107](https://ykh6242.tistory.com/107)

[https://velog.io/@simgyuhwan/쿼리-메소드-JPQL-Querydsl-요약](https://velog.io/@simgyuhwan/%EC%BF%BC%EB%A6%AC-%EB%A9%94%EC%86%8C%EB%93%9C-JPQL-Querydsl-%EC%9A%94%EC%95%BD)
