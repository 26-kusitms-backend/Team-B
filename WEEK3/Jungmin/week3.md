## 스프링 빈 객체는 무엇인가

스프링 컨테이너에 의해 관리되는 자바 객체(POJO)를 말한다.

이전까지 자바에서 객체를 생성할 때는 new를 이용했다. 하지만 스프링에서는 new를 이용해 생성한 객체를 사용하지 않고, 스프링에 의하여 관리당하는 객체를 사용하는데 이 객체를 bean이라고 한다. 


### 💡제어의 역전?


이렇게 객체의 생성 및 제어권을 사용자가 아닌 스프링에게 맡기는 것을 ‘제어의 역전’이라고 한다.

[https://velog.io/@falling_star3/Spring-Boot-스프링-빈bean과-의존관계](https://velog.io/@falling_star3/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88bean%EA%B3%BC-%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84)

## 스프링에서 빈 객체를 관리하는 방법은 무엇인가

### 💡bean을 스프링 컨테이너에 등록하는 방법


### **컴포넌트 스캔 방식**

@Component 어노테이션을 통해 빈을 추가하는 방법이다.

어노테이션을 추가하면 스프링이 알아서 스프링 컨테이너에 빈을 등록한다.

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	String value() default "";

}
```

@Component는 ElementType.TYPE 설정이 있으므로 Class혹은 Interface에만 붙일 수 있다.

**컴포넌트 스캔의 대상**

@Controller,@Service,@Repository,@Configuration은 @Component의 상속을 받고 있으므로 모두 컴포넌트 스캔의 대상이 된다.

- @Controller
    - 스프링 MVC 컨트롤러로 인식된다.
- @Repository
    - 스프링 데이터 접근 계층으로 인식하고 해당 계층에서 발생하는 예외는 모두 DataAccessException으로 변환한다.
- @Service
    - 특별한 처리는 하지 않으나, 개발자들이 핵심 비즈니스 계층을 인식하는데 도움을 준다.
- @Configuration
    - 스프링 설정 정보로 인식하고 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다. (물론 스프링 빈 스코프가 싱글톤이 아니라면 추가 처리를 하지 않음.)

### **JAVA 코드로 등록**

@Configuration 어노테이션을 사용하고 , 빈을 등록하기 위해서 인스턴스를 생성하는 메소드 위에 @Bean을 명시하면 된다.

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

}
```

직접 스프링 빈을 등록하여 얻을 수 있는 장점은 인터페이스로 구현된 Repository 클래스가 다른 클래스로 변경될 때 간단히 Configuration의 리턴 클래스 객체만을 수정해주면 된다. 

[https://steady-coding.tistory.com/594](https://steady-coding.tistory.com/594)


### 💡 의존 관계 설정


스프링 빈을 등록한다고 해서 끝난 것이 아니다!! 싱글톤 객체로 생성되어 관리되는 클래스들의 의존관계를 설정해줘야 한다.

### 자동 의존관계

컴포넌트 스캔을 이용해 빈을 등록했을 경우 @Autowired를 명시해주면 된다.

여기에 DI 방법이 두가지로 나뉘는데 생성자 주입과 Setter를 이용한 의존관계 주입이 있다.

### 수동 의존관계

실제 클래스에 구현된 생성자의 형태와 동일하게 Configuration에서도 객체를 리턴해주면 된다. 

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemeverService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemberRepository();
    }

}
```

Repository 객체를 파라미터로 갖는 Service 클래스


### 💡 @Bean vs @Component



**@Bean**

→ 개발자가 **컨트롤이 불가능한** 외부 라이브러리들을 Bean으로 등록하고 싶을 때 사용된다

→ 메소드 또는 어노테이션 단위에 붙일 수 있다.

**@Component**

→ 개발자가 직접 컨트롤이 가능한 클래스들의 경우에 사용된다.

→ 클래스 또는 인터페이스 단위에 붙일 수 있다.

## 싱글톤 디자인 패턴은 무엇인가

생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이며 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.


## 💡스프링에서 빈을 싱글톤으로 설정하는 이유?



![image](https://user-images.githubusercontent.com/85864699/191985041-71a45318-7b51-4dab-8e03-84d2979855f2.png)


동일한 요청이 서로 다른 클라이언트로 부터 동시에 들어올 수 있다.

요청이 들어오면 객체를 만들어서 메모리를 사용하게 되는데, 위의 사진처럼 동일한 요청도 전부 상이한 메모리 공간에 할당시켜 각각 응답해주면 메모리 공간이 부족해질 것이다.

싱글톤 패턴을 적용시켜주면 해당 인스턴스에 대해서 static을 통해 최초 1번만 메모리를 할당시킴.

이후 해당 인스턴스 호출이 생길때마다 최초로 생긴 인스턴스를 사용한다. 


### 💡 싱글톤 패턴에 단점은 없을까?



- 싱글톤 패턴을 사용하는 다른 객체들간 의존성이 높아지기에 객체 지향 설계 원칙에 위배된다.
- 내부 설계 변경하거나 초기화하기 어려움
- private 생성자 사용하기 때문에 자식 클래스 만들기 어려움

→ 유연하지 않다!

### 💡 싱글톤 컨테이너



우리가 흔히 “스프링 컨테이너”에 등록한다고 하는데 이때 이 스프링 컨테이너를 “싱글톤 컨테이너”라고 한다. 

스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도(?) 객체 인스턴스를 싱글톤으로 관리한다!(???)

→ 이게 무슨말일까요…?

스프링 컨테이너는 싱글톤 패턴의 문제를 해결하면서도 객체 인스턴스를 싱글톤으로 관리한다.

[https://datajoy.tistory.com/112](https://datajoy.tistory.com/112)

[https://hongchangsub.com/springcore5/](https://hongchangsub.com/springcore5/)

## 스프링 MVC 기반 WAS의 아키텍처를 각자 그려보고 데이터 흐름 설명하기


### 💡 WAS가 뭘까요?


웹 어플리케이션 서버로 DB조회나 로직 처리를 요구하는 동적 컨텐츠를 제공하기 위해 만들어진 Application Server

Web Container또는 Servlet Container로 불린다.

Container란 jsp,Servlet을 실행시킬 수 있는 소프트웨어를 말한다.

**WAS의 기능**

- 프로그램 실행 환경과 DB 접속 기능 제공
- 여러개의 트랜잭션 관리기능
- 업무 처리하는 비즈니스 로직 수행

ex) TomCat,Websphere등등

![image](https://user-images.githubusercontent.com/85864699/191985165-15104c00-6413-4fed-bd9d-ec8aa198146b.png)



### 💡 Servlet이란 뭘까요?


웹 프로그래밍에서 클라이언트 요청을 처리하고 처리 결과를 클라이언트에 전송하는 기술

서블릿의 특징으로는

- HTML을 사용해서 요청에 응답한다.
- Java thread를 통해 동작한다.
- MVC 패턴중 Controller로 이용된다
- HTML 변경 시 서블릿을 재 컴파일 해야한다.

**cf) 서블릿과 JSP의 차이점**

서블릿은 **자바 소스코드** 속에 html이 들어가있는 형태라면 JSP는 JAVA 코드가 들어있는 **html 코드**이다. 

[https://velog.io/@developerjun0615/WEB-WAS-란-무엇일까](https://velog.io/@developerjun0615/WEB-WAS-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

[https://vixxcode.tistory.com/m/174](https://vixxcode.tistory.com/m/174)

[https://kohen.tistory.com/29](https://kohen.tistory.com/29)


### 💡 스프링 MVC 기반 WAS 아키텍쳐


![image](https://user-images.githubusercontent.com/85864699/191985234-d7d18e6c-5cc0-4f82-92a3-f3b8cd3b0497.png)


Spring MVC는 웹 어플리케이션을 만드는데 특화된 서블릿 구현체이다. 

스프링 어플리케이션이 내 웹서버에 요청을 보내게 되면 

Web Server → WAS(Tomcat) → Servlet Dispatcher 순으로 요청을 전달받게 된다.

Spring MVC는 Servlet Dispatcher라는 Servlet으로 요청이 오면 요청의 url을 분석하여 해당 요청을 수행할 수 있는 Spring Bean(Controller)에 요청을 보내준다.

**cf) Servlet Dispatcher**

HTTP 프로토콜로 들어오는 모든 요청을 받아 적합한 컨트롤러에 위임하는 프론트 컨트롤러라고 보면 된다.

클라이언트로 부터 어떤 요청이 오면 톰캣과 같은 서블릿 컨테이너가 요청을 받게된다. 이 모든 요청을 프론트 컨트롤러인 디스패처 서블릿이 받아서 공통적인 작업을 수행한 뒤 해당 요청을 처리할 컨트롤러 빈을 getBean()메소드로 호출해서 받아와 요청에 적합한 컨트롤러의 메소드를 실행시킨다. 

[https://sehun-kim.github.io/sehun/spring-short-story/](https://sehun-kim.github.io/sehun/spring-short-story/)

## JAVA 프로그램 실행과정 설명하기

![image](https://user-images.githubusercontent.com/85864699/191985288-d4d32f7a-1983-45d8-87dd-7db0a2bd144a.png)


JAVA로 프로그래밍된 파일을 java 컴파일러가 가상 기계어 파일인 Java클래스 파일로 만드다. 소스코드를 자바 바이트 코드로 번역한다. JAVA 바이트 코드를 JVM이 읽고 실행하게 된다. 


### 💡 자바 바이트 코드?



JVM이 이해할 수 있는 언어로 변환된 자바 소스코드를 의미한다. 자바 컴파일러에 의해 변환된 코드의 명령어의 크기가 1바이트라서 자바 바이트 코드라고 불린다. 

자바 바이트 코드의 확장자는 .class이고, 자바 바이트 코드는 자바 가상머신만 설치되어 있으면 어떤 OS에서라도 실행될 수 있다.


### 💡 JVM이란?



Java Virtual Machine의 줄임말로 OS마다 따로 코드를 작성해야하는 번거로움 없이 JAVA가 플랫폼에 독립적일 수 있게 만들어준다.

C언어 같은 경우는 바로 기계어로 컴파일 하므로 HW 기종에 맞게 컴파일되어야 한다. → 플랫폼에 종속적이라고 함.

JAVA프로그램은 중간 단계 언어로 컴파일하여 JVM만 각 OS에 설치되어 있다면 HW 기종에 상관없이 단 한번만 컴파일하면 된다 → 플랫폼에 독립적이라고 한다. 

[https://pienguin.tistory.com/entry/JAVA-자바-프로그램-실행-과정-및-기본-구조](https://pienguin.tistory.com/entry/JAVA-%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%8B%A4%ED%96%89-%EA%B3%BC%EC%A0%95-%EB%B0%8F-%EA%B8%B0%EB%B3%B8-%EA%B5%AC%EC%A1%B0)

### **📌 질문**

- 의존성과 객체지향의 관계?
- WAS에서 컨테이너는 도커에서 말하는 컨테이너와 같다고 이해해도 되나요?!
