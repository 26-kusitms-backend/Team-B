# 2주차

태그: Go, JPA, Spring

## Spring

- ORM은 무엇인가
    - 객체와 RDB의 데이터를 자동으로 매핑해주는 것
    - 클래스 ↔ 테이블 간의 불일치를 SQL 자동 생성으로 해결
    - SQL 쿼리를 사용하지 않고 객체 지향 프로그래밍을 통해 DB를 다룰 수 있음
    - DBMS에 종속적이지 않아 재사용성 및 유지보수성 증가
    - 완벽한 ORM으로만 서비스 구현이 어려운 단점이 있음
- 스프링에서 대표적으로 사용하는 ORM은 무엇이 있고, 각각의 특징은 무엇이 있는가
    - JPA
        - ORM을 사용하기 위한 인터페이스를 모아둔 것
        - RDBMS를 사용하는 방식을 정의한 인터페이스
        - 생산성과 유지보수성은 좋지만, 성능이 낮고 러닝커브가 높음
    - Hibernate
        - JPA를 구현한 ORM 프레임워크 중 하나로, JPA 인터페이스의 실제 구현부를 담당함
        - JPA를 실질적으로 주도 중
- JPA/ MyBatis를 사용해봤다면, 그 이유와 경험담 소개 ( 개인의 경험 공유 )
    - 입문을 JPA로 하게 되어 JPA만 사용해보았는데, Repository를 구현할 때 메소드명만으로 쿼리문이 작성되는 기분이라 처음엔 편하고 좋았다. Entity의 연관 관계를 매핑하는 것도 처음엔 어려웠지만 나중에 하고 나니 뿌듯하고 좋았는데, 복잡한 쿼리문이 필요할 때 왜 JPA를 안 쓰는 지 나중에 깨달아버려서.. JPQL, QueryDSL과의 적당한 혼용이 필요할 것 같다.
    
    ```java
    Helps findAllBySuccessIsFalseAndHelper_User_Username(String username);
    Helps findDistinctTopBySuccessIsFalseAndHelper_User_UsernameOrderByAcceptTimeDesc(String username);
    Helps findDistinctFirstByHelperOrderByAcceptTimeDesc(Optional<Helper> helper);
    Helps findDistinctFirstBySuccessIsTrueAndHelperOrderByAcceptTimeDesc(Optional<Helper> helper);
    List<Helps> findAllBySuccessIsFalseAndAcceptTimeBefore(Timestamp timestamp);
    ```
    
- 영속성이란 무엇인가
    - 데이터를 생성한 프로그램이 종료되어도 사라지지 않는 특성
    - 메모리 상의 데이터를 RDBMS 등에 저장하여 영속성을 부여할 수 있음 → JDBC, Spring JDBC, 영속성 프레임워크(SQL Mapper, ORM)
    - 해당 작업은 영속성 계층에서 이루어지고, JDBC 혹은 영속성 프레임워크를 이용한 개발이 많이 이루어짐
- 영속성 컨텍스트와 그 특징은 무엇이 있는가
    
    ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled.png)
    
    - 엔티티를 영구적으로 저장하는 환경 → APP과 DB 사이에서 객체를 보관하는 곳이며, 엔티티 매니저가 관리함
        
        ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%201.png)
        
    - 영속성 컨텍스트에서 엔티티는 생명주기를 갖고 있음
    - 비영속: 아직 영속성 컨텍스트에 저장되기 전 상태
    - 영속: persist()를 통해 영속성 컨텍스트에 저장되어 관리되고 있는 상태
    - 준영속: detach()를 통해 더 이상 영속성 컨텍스트에서 관리되지 않는 상태
    - 영속성 컨텍스트의 장점 5가지
        - 1차 캐시: 영속성 컨텍스트의 내부에 있는 캐시로, 영속 상태의 엔티티가 저장되는 곳. 키는 식별자 값(DB의 PK)이고 값은 엔티티 인스턴스이다. DB를 조회하는 과정은 다음과 같다.
            
            ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%202.png)
            
        - 동일성 보장: == 연산자로 실제로 인스턴스가 같은지 비교할 수 있다.
        - 쓰기 지연: 트랜잭션을 커밋하기 전까지 내부 쿼리 저장소에 SQL을 모아두고 해당 트랜잭션이 커밋될 때 모아둔 SQL문들을 DB로 보낸다.
        - 변경 감지: 엔티티 수정시 단순히 엔티티를 조회한 뒤 데이터를 변경하면 된다.
            
            ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%203.png)
            
        - 지연 로딩: 연관 관계가 매핑되어 있는 엔티티를 조회했을 때 실제 객체가 아니라 프록시를 반환한다. 이 **객체의 필드에 접근할 때** 쿼리가 나간다.
            
            ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%204.png)
            
    - 플러시: 영속성 컨텍스트의 변경된 내용을 데이터베이스에 반영하는 것으로, flush()를 통해 변경 내용을 데이터베이스에 동기화한다.
        
        ![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%205.png)
        
- 트랜잭션은 무엇인가
    - DB의 상태를 변화시키기 위해 수행하는 작업의 단위
    - ACID(원자성, 일관성, 독립성, 지속성)를 보장해야 함
    - Commit을 통해 트랜잭션을 DB에 반영하고, 이를 Rollback 또한 가능함
- QueryDSL
    - JPQL은 간단한 쿼리를 테스트할 때는 좋지만, 쿼리문이 길어질 경우 오타나 문법적인 오류가 존재할 경우 에러를 잡아내기 힘들 뿐더러 컴파일 타임에 에러를 체크할 수 없음
    - Entity에 대해서 Q클래스라는 QueryDSL 전용 객체를 만듦
    
    ```sql
    select m from Member m where m.age > 18;
    ```
    
    ```java
    JPAFactoryQuery query = new JPAQueryFactory(em);
    QMember m = QMember.member;
    
    List<Member> list = 
        query.selectFrom(m)
             .where(m.age.gt(18))
             .orderBy(m.name.desc())
             .fetch();
    ```
    
    - 쿼리문을 위와 같이 코드로 변경하여 사용할 수 있음
    - 동적 쿼리
    
    ```java
    String name = "member";
    int age = 9;
    
    QMember m = QMember.member;
    
    BooleanBuilder builder = new BooleanBuilder();
    if(name != null) {
        builder.and(m.name.contains(name));
    }
    if(age != 0) {
        builder.and(m.age.gt(age));
    }
    
    List<Member> list = 
        query.selectFrom(m)
             .where(builder)
             .fetch();
    ```
    
    - BooleanBuilder를 사용해 조건에 따라 쿼리문을 변화시킬 수 있음
    - CSR, SSR

---

## Go

- 출력 함수

![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%206.png)

- %v → 데이터 타입에 맞춰서 기본 형태로 출력, 모를 때 유용
- 입력 함수

![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%207.png)

```go
func Scan(a ...interface{}) (n int, err error)
```

```go
var a int // ❶ 값을 저장할 변수
var b int

n, err := fmt.Scan(&a, &b) // ❷ 입력 두 개 받기
if err != nil {            // ❸ 에러가 발생하면 에러 코드 출력
	fmt.Println(n, err)
} else { // ➍ 정상 입력되면 입력값 출력
	fmt.Println(n, a, b)
}
```

- Scan()을 여러 번 사용하는 경우

```go
import (
	"bufio"
	"fmt"
	"os"
)
```

```go
stdin := bufio.NewReader(os.Stdin)
/* stdin은 표준 입력을 읽는 객체 */
stdin.ReadString('\n')
/* 필요할 때 호출해서 버퍼 비우기 */
```

- 산술 연산자

![Untitled](2%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%2098b1501ecf434f45ac39552a6e1d2808/Untitled%208.png)

- ^A → ~A와 같음 (비트 반전)
- 여러 문자 대입 가능

```go
a, b = 2, 3
```
