# 큐시즘 백엔드 스터디 3주차 개인정리

# Spring Study

## 1. 스프링 빈 객체는 무엇인가?

**Spring IoC 컨테이너가 관리하는 자바 객체를 빈(Bean)** 이라고 부른다. 

IoC는 Inversion of Control 이다. 네이버에 번역해보면 제어 반전을 뜻하고 있다. **IoC(제어 반전)**이란, 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미한다. 

컴포넌트 의존관계 설정(Component dependency resolution), 설정(Configuration) 및 생명 주기(Life Cycle)을 해결하기 위한 디자인 패턴이다.

스프링 프레임워크도 객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 컨테이너가 있는데, 그것이 바로 **IoC 컨테이너**다.

인스턴스 생성부터 소멸까지의 인스턴스 생명주기 관리를 개발자가 아닌 컨테이너가 대신 해준다. 그렇게 되면 객체관리 주체가 프레임워크(Container)가 되기 때문에 개발자는 로직에 집중할 수 있는 장점이 있다.

<aside>
💡 IoC의 특징을 좀 더 자세히 설명하자면, 처음에 배우는 자바 프로그램에서는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 조작하는 작업(객체를 직접 생성하여 메소드 호출)을 했다. 즉, 모든 작업을 제어하는 구조였다. 예를 들어 A 객체에서 B 객체에 있는 메소드를 사용하고 싶으면, B 객체를 직접 A 객체 내에서 생성하고 메소드를 호출한다.

</aside>

<aside>
💡 하지만 IoC가 적용된 경우, 객체의 생성을 특별한 관리 위임 주체에게 맡긴다. 이 경우 사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 다른  주체가 된다. 즉, 사용자의 제어권을 다른 주체에게 넘기는 것을 IoC(제어 반전) 라고 한다.

</aside>

다시 Bean 으로 돌아오자면, 우리가 알던 기존의 자바 프로그래밍에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용했다. 하지만 Spring에서는 직접 new를 이용하여 생성한 객체가 아니라, Spring에 의하여 관리당하는 자바 객체를 사용한다. 이렇게 Spring에 의하여 생성되고 관리되는 자바 객체를 Bean 이라고 한다. 

bean 을 등록하는 방법에는 크게 두가지가 있다.

1. **Compoenet Scan**
2. **직접 자바로 등록하기**

### Component Scan

Component Scan은 **annotation인 @Component 를 명시하여 Bean에 추가**하는 방법이다. 

@Controller, @Service, @Repository, @Configuration는 @Component의 상속을 받고 있으므로 모두 컴포넌트 스캔의 대상이다

- @Controller
    - **스프링 MVC 컨트롤러**로 인식된다.
        
        ```java
        @Controller
        public class MemberController{
        
        	private final MemberService memberService;
        	@Autowired
        	//멤버 컨트롤러가 생성이 될때 스프링빈에 등록되어있는 멤버서비스를 주입
        	public MemberController(MemberService memberService) {
        		this.memberService = memberService;
        	}
        ```
        
- @Repository
    - **스프링 데이터 접근 계층으로 인식한다.**
        
        ```java
        @Repository
        public class MemoryMemberRepository implements MemberRepository {
        }
        ```
        
        이렇게 설정하면, 스프링 컨테이너에 Controller → Service → Repository 구조로, MemberService와 Repository가 스프링 빈으로 등록된다.
        
    
- @Service
    - 특별한 처리는 하지 않으나, 개발자들이 핵심 비즈니스 계층을 인식하는데 도움을 준다.
        
        ```java
        @Service
        public class MemberService{
        	private final MemberRepository memberRepository;
        	@Autowired
        	public MemberService(MemberRepository memberRepository) {
        		this.memberRepository = memberRepository;
        	}
        }
        ```
        
        생성자에 @Autowired를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 @Autowired는 생략할 수 있다.
        
        <aside>
        💡 스프링 MVC란 무엇인가? MVC(Model View Controller)란 하나의 디자인 패턴이다. 스프링에서 제공하는 웹 애플리케이션 구축 전용 MVC 프레임워크이다. 모델(Model)은 애플리케이션이 무엇을 할지 정의하고, 뷰(View)는 화면에 표현을 한다. 컨트롤러(Controller)는 위 두가지를 분리하기 위해 양측 사이에 배치된 인터페이스다. 간단하게 말하면 모델이 어떻게 처리할 지를 알려주는 역할을 한다.
        
        </aside>
        

### 직접 자바로 등록하기

자바 코드로 직접 등록할때도 Controller는 컨포넌트 스캔으로 올라가기 때문에 @Controller를 추가해야 한다.

```java
@Controller
public class MemberController{

	private final MemberService memberService;
	@Autowired
	//멤버 컨트롤러가 생성이 될때 스프링빈에 등록되어있는 멤버 서비스를 주입
	public MemberController(MemberService memberService){
		this.memberService = memberService;
	}
}
```

다음으로 Service와 Repository에 이전에 추가했던 어노테이션을 추가하는 대신에 [SpringConfig.java](http://SpringConfig.java) 라는 클래스를 하나 생성한다.

```java
@Configuration
public class SpringConfig{

	@Bean
	public MemberService memberService(){
		return new MemberService(memberRepository());
	}

	@Bean
	public MemberRepository memberRepository(){
		return new MemoryMemberRepository();
	}
}
```

@Configuration 어노테이션을 추가하고, 스프링 빈으로 등록하고자 하는 Service와 Repository에 @Bean 어노테이션을 추가하면, 스프링 빈으로 등록된다.

최근에는 어노테이션 하나로 해결되는 앞의 방법이 더 많이 사용되고 있지만, 상황에 따라 2번 방법도 사용되고 있다고 한다. 개발을 진행하다가 Repository를 변경해야 한다고 가정 했을때, 2번째 방법을 사용하면 Service나 Controller를 건드릴 필요 없이 SpringConfig에 등록된 Repository Bean만 수정해주면 간편하기 때문이라고 한다.

## 2. 스프링에서 빈 객체를 관리하는 방법은 무엇인가?

스프링 컨테이너를 사용하면 컨테이너에 등록되는 빈들을 알아서 **싱글톤**으로 관리해준다. 싱글톤이란 무엇인가?

### 싱글톤 컨테이너(Singleton Container)

싱글톤 컨테이너란 클래스의 인스턴스가 JVM 내의 단 하나만 존재하는 것을 뜻한다. 

웹 애플리케이션은 수많은 클라이언트에서 서비스를 요청받게 되는데 서버에서 클라이언트의 요청을 받을때마다 클래스 인스턴스를 생성하게 되면 JVM 메모리의 사용량이 증가하게 되고 서버는 부하를 감당할 수 없게 될 것이다.

간단한 예제 소스를 통해 위의 상황을 테스트 해보겠다.

```java
//Appconfig.java
@Configuration
public class AppConfig{

	@Bean
	public MemberRepository getMemberRepository(){
		return new MemoryMemberRepository();
	}

	@Bean
	public DiscountPolicy getDiscountPolicy(){
		return new RateDiscountPolicy();
	}

	@Bean
	public OrderService orderService(){
		return new OrderServiceImpl(getMemberRepository(), getDiscountPolicy());
	}
}
```

```java
//SingletonTest.java
public class SingletonTest{

	@Test
	void pureContainer(){
		AppConfig appConfig = new AppConfig()'

		//1. 조회: 호출할때마다 객체를 생성
		MemberService memberService1 = appConfig.memberService();

		//2. 조회: 호출할때마다 객체를 생성
		MemberService memberService2 = appConfig.memberService();

		System.out.println("memverService1 = " + memberService1);
		System.out.println("memberService2 = " + memberService2);

		Assertions.assertThat(memberService1).isNotSameAs(memberService2);
	}
}
```

Appconfig는 MemberService 객체를 반환해주는 클래스이고 SingletonTest 클래스의 pureContainer 메서드에서는 Appconfig 객체를 생성하여 MemberService 클래스의 인스턴스를 만들고 있다. Appconfig를 통해 만들어진 2개의 Memberservice 인스턴스의 참조값이 다른 것을 확인할 수 있다.

즉, 스프링이 아닌 **순수 Java 코드를 이용한 AppConfig는 사용자의 요청이 들어올 때마다 새로운 MemberService 클래스의 인스턴스를 생성하게 되고 그만큼 JVM의 메모리 사용량이 증가**하게 되는 것이다. 

해결 방법은 **AppConfig에서 생성한 MemberService 객체를 공유**하는 것이다.

스프링에서는 사용자가 이렇게 일일이 구현하지 않고 싱글톤 패턴의 문제점들도 보완해주면서 **싱글톤 패턴으로 클래스의 인스턴스를 사용하게 해 주는데 이것을 싱글톤 컨테이너**라고 한다.

스프링 컨테이너는 싱글톤 컨테이너의 역할을 하며 싱글톤 객체를 생성, 관리하는 주체를 싱글톤 레지스트리라고 부른다. 스프링 컨테이너의 기능으로 인해 순수 Java에서 싱글톤 패턴을 구현하기 위한 코드를 사용하지 않아도 된다.

테스트 코드로 확인해보겠다.

```java
//SingletonTest.java

@Test
void springContainer(){
	
	ApplicationContext ac = new AnnotaionConfigApplicationContext(AppConfig.class);

	MemberService memberService1 = ac.getBean("memberService", MemberService.class);
  MemberService memberService2 = ac.getBean("memberService", MemberService.class);

  System.out.println("memberService1 = " + memberService1);
  System.out.println("memberService2 = " + memberService2);

  Assertions.assertThat(memberService1).isSameAs(memberService2);
}
```

<aside>
💡 JVM(Java Virtual Machine)이란? 가자 가상 머신 JVM은 자바 프로그램 실행환경을 만들어 주는 소프트웨어이다. 자바 코드를 컴파일하여 .class 바이트 코드로 만들면 이 코드가 자바 가상 머신 환경에서 실행된다.

</aside>

## 3. 싱글톤 디자인 패턴은 무엇인가?

싱글톤 패턴(Singleton Pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다. 즉, 객체의 인스턴스가 1개만 생성되는 것을 보장하는 디자인 패턴이다. 

싱글톤 패턴은 왜 사용하는 것일까? 코드로 설명해보겠다.

```java
public class A{
	private String name;

	public A(String name){
		this.name=name;
	}
}
```

우선 아주 간단한 클래스 A를 설정했다. 싱글톤 없이 객체를 생성해보겠다.

```java
public class ATest{

	@Test
	@DisplayName("싱글톤 없이 객체 생성")
	void notSingleton(){
		A name1 = new A("AAA");
		A name2 = new A("AAA");

		Assertions.assertThat(name1).isEqualTo(name2);
	}
}
```

우선 싱글톤 없이 객체를 생성했다. AAA라는 이름이 담긴 name1과 AAA라는 이름이 담긴 name2를 생성했다.

과연, name1과 name2는 같을까?

아니다. name1은 A@5276d6ee에 저장되었고 name2는 A@5c7bfdc1에 저장되었다.

지금은 2개지만, 만약 이름을 1만개 이상 입력한다면, 컴퓨터는 똑같이 1만개의 객체를 생성한다. 같은걸 재사용하면 되는데 쓸데없이 1만개가 넘는 객체를 생성하고 엄청난 메모리 낭비가 발생한다. 이런 **메모리 낭비를 방지하기 위해 싱글톤 디자인 패턴**을 사용한다.

그렇다면, 위 코드에 싱글톤 패턴을 적용해보겠다.

```java
public class A {
    private String name; //공유 필드를 쓰면 사용자먀다 값이 똑같아서 사용하면 안됨
    private static final A instance = new A();

    private A(){

    }

    public static A getInstance(){
        return instance;
    }

    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
}
```

이 코드에서 핵심은 **private static final A instance = new A();** 부분이다. 생성할 객체를 딱 1개만 사용하겠다라는 의미를 static final을 사용하여 전역변수로 만들어버린다. 그리고, getInstance() 메소드로 생성한 객체를 재활용하는것이다

```java
public class ATest {

    @Test
    @DisplayName("싱글톤 객체없이 생성")
    void notSingleton(){
        A name1 = A.getInstance();
        A name2 = A.getInstance();

        name1.setName("AAA");
        name2.setName("AAA");

        assertThat(name1).isEqualTo(name2);
    }
}
```

name1과 name2를 생성하고 getInstance로 객체를 불러온다.

그리고 테스트로 두 객체가 같은건지 테스트해보겠다.

한번 생성한 객체를 getInstance를 사용해서 재사용하는 패턴이 싱글톤 패턴이다.

## 4. 스프링 MVC 기반 WAS의 아키텍처를 각자 그려보고 데이터 흐름 설명


순서는 이렇다.

> 브라우저 → DispatcherServlet → HandlerMapping 역할 분배 → HandlerAdapter 조회 → HandlerAdapter 실행 → Controller 실행 → HandlerAdapter에서 ModelAndView 반환 → ViewResolver 호출 → ViewResolver에서 View 반환 → View를 통해서 View 랜더딩 → 응답
> 

### DispatcherServlet

DispatcherServlet의 Dispatch는 “보내다”라는 뜻을 가지고 있다. 그리고 이러한 단어를 포함하는 DispatcherServlet은 브라우저에서 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)라고 정의할 수 있다. 

이것을 보다 자세히 설명하자면, 클라이언트로부터 어떠한 요청이 오면 Tomcat(톰캣)과 같은 서블릿 컨테이너가 요청을 받게 된다. 그리고 이 모든 요청을 프론트 컨트롤러인 DispatcherServlet이 가장 먼저 받게 된다. 그러면 DispatcherServlet은 공통작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 컨트롤러를 찾아서 작업을 위임한다.

여기서 Front Controller(프론트 컨트롤러)라는 용어가 사용되는데, Front Controller는 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 치리해주는 컨트롤러로써, MVC 구조에서 함께 사용되는 디자인 패턴이다.

### HandlerMapping

HandlerMapping은 request의 URL과 매칭되는 handler를 선택하는 역할을 수행한다. 즉, HandlerMapping은 원하는 handler를 찾아오는 역할을 수행한다. 스프링 프레임워크에서 제공하는 HandlerMapping 클래스는 4가지가 있다.

- BeanNameUrlHandlerMapping: URL과 일치하는 이름을 갖는 빈의 이름을 Controller로 매핑
- ControllerClassNameHandlerMapping: URL과 일치하는 클래스 이름을 갖는 빈을 Cpntroller로 사용
- SimpleUrlHandlerMapping: URL 패턴에 매핑되는 지정된 Controller를 사용
- DefaultAnnotationHandlerMapping: 어노테이션을 이용해 URL과 Controller를 매핑

### HandlerAdapter

HandlerMapping을 통해 찾은 컨트롤러를 직접 실행하는 기능을 수행한다. HandlerAdapter 인터페이스를 구현해서 생성한다.

```java
public interface HandlerAdapter{
				boolean supports(Object handler);
	
				ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception;

				long getLastModified(HttpServletRequest request, Object handler);
}
```

HandlerMapping으로 찾은 오브젝트(컨트롤러)를 등록된 HabdlerAdapter들의 support 메서드에 대입하여 지원 여부를 살핀다. 부합할 경우 handler 메서드를 실행하여 ModelAndView 를 리턴한다.

### Controller

스프링 프레임워크의 Controller는 사용자가 화면(View) 단에서 입력이나 어떤 이벤트를 했을 경우, 그 이벤트에 맞는 화면(View)이나 비즈니스 로직(Model)을 실행할 수 있도록 업데이트를 해주는 역할을 하고 있다.

즉, 스프링 프레임워크의 Controller의 역할은 아래와 같다.

- Data receive
- Interpret
- Validate input data
- Update View
- Modify Model

단순히 말하자면 스프링 프레임워크의 Controller 역할은 Model과 View를 이어주는 다리 역할이라고 보면 된다.

스프링 프레임워크에서 Controller는 @Controller 어노테이션을 추가하여 사용한다. 이 @Controller 어노테이션은 어노테이션이 적용된 클래스를 컨트롤러임을 나타내고, 자동으로 bean이 등록되어 @Controller 어노테이션이 적용된 클래스를 Controller로 사용할 수 있다.

### View

View는 모델이 가진 정보를 어떻게 표현해야 하는지에 대한 로직을 갖고 있는 컴포넌트이다. 일반적인 View의 결과물은 브라우저에서 볼 수 있는 HTML이다.

View는View Interface를 구현해서 생성한다.

```java
public interface View{
    void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse resposne) throws Exception;
}
```

### ViewResolver

View를 선택하는 것도 Controller의 역할이다. 하지만 Controller에서 매번 View를 생성하는 것은 비효율적이므로, 스프링에서는 이 작업을 적절히 분리하였다. Controller는 View의 논리적인 이름만을 리턴한 뒤 역할을 종료하고, 이를 DispatcherServlet의 ViewResolver가 받아 사용할 View Object를 찾고 생성하는 작업을 진행하준다.

게다가 ViewResolver는 보통 View Object를 캐싱하므로 같은 URL의 View가 반복적으로 만들어지지 않는 장점도 있다. ViewResolver도 하나 이상 등록해서 사용할 수 있는데, 이때는 HandlerMapping 처럼 order 프로퍼티를 이용해 적용 순서를 적용해주는 것이 좋다.

ViewResolver는 ViewResolver Interface를 구현해서 생성한다.

```java

public interface ViewResolver{
    View resolveViewName(String viewName, Locale locale) throws Exception;
}
```

## 5. Web Server와 WAS의 차이는 무엇인가?

Web Server와 WAS의 차이점을 먼저 살펴보기 전에 정적 페이지(Static Pages)와 동적 페이지(Dynamic Pages)를 먼저 이해해보려고 한다.

### 정적 페이지(Static Pages)와 동적 페이지(Dynamic Pages)

우리가 보는 웹페이지는 위의 이미지처럼 웹 서버는 주소(url)을 가지고 통신 규칙(http)에 맞게 요청하면, 알맞은 내용(html)을 응답받는다. 그러나 이처럼 단순한 클라이언트(웹 브라우저)와 웹 서버로는 정적(static)인 페이지밖에 처리하지 못한다는 한계를 가진다. 이러한 html의 태생적인 한계를 극복하기 위해 application을 활용한 것이 Web Application 이다. 따라서 정적인 html의 한계를 극복하고 동적인 페이지를 제공하고자 하는 목적, 더 나아가 보안 강화와 장애 극복을 가능하게 만드는 것이 WAS이다.

### 정적 페이지(Static Pages)

- Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
- 항상 동일한 페이지를 반환한다.
- html, css, js, image 파일과 같이 컴퓨터에 저장되어 있는 파일들이다.

### 동적 페이지(Dynamic Pages)

- 인자의 내용에 맞게 동적인 Contents를 반환한다.
- 즉, 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물이다.
- 아래 이미지에서 개발자는 Servlet에 doGet()을 구현한다.

그럼 이제 Web Server와 WAS의 차이점을 알아보자.

### Web Server

웹 서버는 클라이언트로부터 HTTP 요청을 받아 HTML 문서나 각종 리소스를 전달하는 컴퓨터다.

요청에 따라 아래의 두 가지 기능 중 적절하게 선택하여 수행한다.

**기능 1**

- 정적인 컨텐츠를 제공한다.
- WAS를 거치지 않고 바로 자원을 제공한다.

**기능 2**

- 동적인 컨텐츠 제공을 위한 요청을 전달한다.
- 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달한다.
- 클라이언트는 일반적으로 웹 브라우저를 의미한다.

### Web Application Server, WAS

WAS는 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 미들웨어 프레임워크이다.

위 이미지와 같이 WAS는 Web Server 와 Web Container(JSP< Servlet)으로 이루어져 있다. Web Server와의 차이점은 Web Container를 가진다는 점이며 WAS는 HTML 같은 정적인 페이지에서 처리할 수 없는 비즈니스 로직이나 DB 조회 같은 동적인 컨텐츠를 제공한다.

# Go Study

## 1. 함수 정의

함수는 함수 키워드, 함수명, 매개변수, 변환 타입, 함수 코드 블록으로 구성된다.

```go
func Add(a int, b int) int{
			//a 와 b를 더한 결과를 반환한다.
			return a + b
}
```

func 키워드를 사용해서 함수 정의를 알린다. 그 뒤에 함수명(Add)이 온다. 함수명의 명명 규칙은 변수명과 같다. 첫 글자가 대문자인 함수는 패키지 외부로 공개되는 함수이다.

소괄호(a int, b int) 안에 매개변수를 넣는다. 매개변수는 함수 코드 수행 시 필요한 입력값이다. 만약 매개변수가 필요하지 않으면 비워둔다. 반환타입(int)이 온다. 반환하는 값이 있으면 적고, 아니면 비워둔다. 중괄호로 함수 코드 블록을 표시한다. Go 언어에는 함수 코드 블록의 시작을 알리는 중괄호 { 가 함수를 정의하는 라인과 항상 같은 줄에 있어야 한다.

정수 타입 매개변수 2개를 입력으로 받아서 그 합을 반환하는 함수를 구현해보자.

```go
package main

import "fmt"

func Add(a int, b int) int { //1
		return a + b             //2
}
func main(){
		c := Add(3, 6)  //3
		fmt.Println(c)  //4
}
```

1. Add() 함수를 정의한다. 두 정수 타입 매개변수 a 와 b의 입력을 받는다.
2. 입력받은 a 와 b의 합을 반환한다.
3. Add() 함수를 호출한다. 반환값을 c에 저장한다.
4. c를 출력한다.

## 2. 함수 사용

평균 점수를 출력하는 함수를 Golang으로 만든 예제다.

```go
package main

import "fmt"

func PrintAvgScore(name string, math int, eng int, history int){
	total := math + eng + history
	avg := total / 3
	fmt.Println(name, "님 평균 점수는", avg, "입니다.")
}

func main(){
	PrintAvgScore("김일등", 80, 74, 95)
	PrintAvgScore("송이등", 88, 92, 53)
	PrintAvgScore("박삼등", 78, 73, 78)
}
```

변수 적는 방법은 아직도 적응이 안된다…

```go
package main

import "fmt"

func Divide(a, b int)(int, bool){ //1 함수 선언
	if b == 0{ 
		return 0, false               //2 제수가 0일 때 반환
	}
	return a / b, true              //3 제수가 0이 아닐 때 반환
}

func main(){
	c, success := Divide(9, 3) //4 제수가 0이 아닌 경우
	fmt.Println(c, success)
	d, scuuess := Divide(9, 0) //5 제수가 0인 경우
	fmt.Println(d, success)
}

```

1. Divide() 함수를 정의한다. 이 함수는 int 타입 a, b를 매개변수로 받고 int 타입과 bool 타입을 반환한다.(엥…?) (a int, b int) 같이 매개변수 타입이 같으면 간단히 (a, b, int)처럼 표현할 수 있다.
2. 나눗셈 제수가 0이면 0과 false를 반환한다. 3번에서 제수가 0이 아닐 때는 나눗셈 결과와 true를 반환한다. 4, 5번에서 Divide() 함수를 호출하고, 그 결과를 변수 c와 success로 받는다. 첫 번째 반환값은 c에, 두 번째 반환값은 success에 대입된다.

Divide() 함수를 호출한 결괏값이 c와 success에 대입되는 과정은 다음과 같다.

```go
c, success := Divide(9, 3) //함수 호출

c, success := 3, true //함수 호출 후 반환값이 3, true
                      //3은 c에, true는 success에 대입
```

위의 코드를 이해하는데 시간이 좀 걸렸다. (int, bool)이 왜 저기에 쓰이는지, 그리고 출력값이 왜 저렇게 나오는지 처음엔 이해가 잘 안되서 코드를 긴 시간동안 봤다. 좀 더 직관적인 코드로도 바꿀 수 있는것 같다.

```go
package main

import "fmt"

func Divide(a, b int) (result int, success bool) { //반환할 변수명 명시
	if b == 0 {
		result = 0
		succsee = false
		return 
	}
	result = a / b
	success = true
	return 
}

func main() {
	c, success := Divide(9, 3)
	fmt.Println(c, success)
	d, success := Divide(9, 0)
	fmt.Println(d, success)
}
```

함수 선언부에서 첫 번째로 반환할 변수로 result를, 두 번째는 success를 지정했다. 이렇게 지정한 result, success는 함수 내부에서 변수로 동작한다. 함수 결과를 반환할 때 명시적으로 result, success를 지정하지 않았지만 두 값이 반환된다.

재귀 호출에 대해서도 알아보려고 한다. 재귀 호출이란 함수 안에서 자기 자신 함수를 다시 호출하는 것을 말한다. 

```go
package main

import "fmt"

func printNo(n int){ //1
	if n == 0{
		return
	}
	fmt.Println(n)
	printNo(n-1) //2
	fmt.Println("After", n) //3
}

func main(){
	printNo(3) //4
}
```

4번에서 printNo() 함수를 호출한다, 그리고 1번에서 탈출 조건인지 확인한다. n이 0이 아니면 2번 문장의 printNo() 함수를 다시 호출한다.

이렇게 함수 안에서 같은 함수를 다시 호출하는 것을 재귀 호출이라고 한다. 호출 순서를 보면 printNo(3)이 먼저 호출되고 이어서 printNo(2), printNo(1), printNo(0) 순으로 호출된다. 최종적으로 printNo(0)이 호출됐을 때 1번 코드에서 탈출 조건인 n == 0을 만족해 return 되어 종료된다.
