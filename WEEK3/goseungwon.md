# 큐시즘 스프링 스터디 3 주차

날짜: 2022년 9월 24일

## ❓스프링 빈 객체

스프링 빈은 자바 빈과 큰 차이가 있다.

**자바 빈**

반복적인 작업을 효율적으로 하기 위해 사용하는 클래스

private필드 getter/setter 생성자를 가지고 있음 (DTO와 거의 동일한 개념)

**스프링 빈**

스프링 컨테이너가 관리하는 Java 객체를 뜻한다.

- **IoC**
    
    일반적으로 자바에서는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 호출했다. 즉 사용자가 제어하는 방식
    
    하지만 IoC가 적용되면 객체의 생성을 특별한 주체에게 맡긴다. 사용자는 객체를 직접 생성하지 않고,  다른 주체가 객체의 생명주기를 컨트롤 한다. 즉 제어권이 다른 주체(스프링 컨테이너)에게 넘어가는 것을 뜻한다.
    

- **스프링 컨테이너의 역할**
    1. 객체의 생성, 의존성 관리
    2. POJO ( 위에서 말한 자바 빈과 비슷한 개념 ) 생성, 초기화, 서비스, 소멸 권한
    3. 개발자가 비즈니스 로직에 집중하게 도와준다.

- **스프링 빈을 컨테이너에 등록하는 방법**
    1. 어노테이션
        
        @Component 어노테이션을 사용하면 스프링이 자체적으로 등록한다.
        
        ```java
        @Controller
        @RequestMapping("/basic")
        public class BasicController {
        ...
        }
        ```
        
        Controller, Service등 다른 어노테이션은 Component어노테이션을 상속받고 있음
        
        ```java
        @Target(ElementType.TYPE)
        @Retention(RetentionPolicy.RUNTIME)
        @Documented
        @Component
        public @interface Controller {
        
        	@AliasFor(annotation = Component.class)
        	   String value() default "";
        }
        ```
        
    
    1. Bean Configuration에 직접 등록
    
    @Configuration과 @Bean 어노테이션을 이용해 등록할 수 있다.
    
    ```java
    @Configuration
    public class BasicController{
        @Bean
        public BasicController sampleController() {
            return new SampleController;
        }
    }
    ```
    
    1. XML파일에 설정
        
        claass = 자바 Class이름 (필수)
        
        id = bean 의 id
        
        scope = 객체의 범위 (singleton, prototype 등등)
        
        constructor-arg = 생성 시 생성자에 전달할 인수
        
        property = 생성 시 bean setter에 전달할 인수
        
        init-method, destroy-method
        
        ```xml
        <bean id="아이디" class="클래스이름" scope="스코프종류 ....>
        		<property name="이름" value="내용"/>
        </bean>
        ```
        
    
    Bean은 개발자가 **컨트롤 할 수 없는** 외부라이브러리를 등록하고 싶은 경우에 사용할 수 있으며 메소드 또는 어노테이션 단위에 붙일 수 있다.
    Component는 개발자가 **컨트롤 할 수 있는** 경우에 사용하며 클래스 또는 인터페이스에 붙일 수 있다.
    

## ❓스프링에서 빈 객체를 관리하는 방법

- **스프링이 스캔하는법**
    1. 어노테이션
        
        스프링 Container를 만들어 Bean을 등록하는데 사용되는 interface를 Life Cycle Callback이라 한다. 
        
        Life Cycle Callback은 @Component가 붙은 모든 Class의 interface를 만들어 Bean으로 등록한다.
        
        이 후 @Component가 붙은 클래스는 component scan 대상이 되어 스캔 후 등록된다.
        
    2. Bean Configuration에 등록
        
        @Configuration어노테이션은 @Component를 사용하기 떄문에 component scan의 대상이 되고, @Bean을 등록한다.
        
    3. XML
        
        XML을 읽어서 BeanDefinition을 만든다.
        

1. 빈 등록
    
    스프링 컨테이너는 파라미터로 넘어온 Config정보를 사용해서 스프링 빈을 등록한다.
    
    [ bean 메서드 이름 / bean 객체 ]형식으로 저장한다. 이 때 객체 인스턴스를 싱글톤으로 관리한다.
    
    ❗ bean이름이 같으면 무시되거나 덮어버리는 오류가 발생하기 때문에 bean이름은 항상 다른이름을 부여해야 한다.
    

1. 의존관계 설정
    
    스프링 컨테이너는 설정 정보를 참고해 의존관계를 주입한다(DI) 
    
    상속관계가 있는 빈을 조회하면 자식타입은 모두 조회된다.
    

**스프링 컨테이너의 인터페이스들**

1. BeanFactory
    
    스프링 컨테이너의 최상위 클래스
    
    스프링 빈을 관리하고 조회하는 역할
    

1. ApplicationContext
    
    BeanFactory를 모두 상속해서 사용한다.
    
    BeanFactory기능 외에도 애플리케이션을 개발할 때 수 많은 부가기능을 제공한다.(환경변수, 국제화, 이벤트 등등)
    

## ❓싱글톤 디자인 패턴

요청이 생길 때 마다 객체를 생성하면 트래픽마다 객체가 생성되고 소멸된다.  메모리 낭비가 심하기 때문에 싱글톤패턴을 사용한다.

싱글톤은 클래스의 인스턴스가 딱 1개만 생성되는걸 보장하는 디자인 패턴이다.

ㄴprivate 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록  막는다.

싱글톤 적용 객체 코드

```java
//static영역에 객체를 final로 딱 1개만 생성
private static final SingletonService instance = new SingletonService();

//객체 인스턴스가 필요할 시 static 메서드를 통해서만 조회하도록 허용한다.
public static SingletonService getInstance() {
		return instance;
}

//생성자를 private로 선언해서 외부에서 new키워드를 통해 생성하지 못하게 한다.
private SingletonService(){  }
```

싱글톤 객체 사용 코드

```java
//생성자가 private이기 때문에 오류 발생
new SingletonService();

//호출할 때 마다 같은 객체 반환.
SingletonService singleton1 = SingletonService.getInstance();
SingletonService singleton2 = SingletonService.getInstance();

//같은 값을 반환한다.
System.out.println("singletonService1 = " + singletonService1);
System.out.println("singletonService2 = " + singletonService2);
```

## ❓스프링 MVC 기반 WAS의 아키텍처 / 데이터 흐름

**WAS와 스프링의 통신 아키텍처**(출처 : 김영한님 강의)

![Untitled](%E1%84%8F%E1%85%B2%E1%84%89%E1%85%B5%E1%84%8C%E1%85%B3%E1%86%B7%20%E1%84%89%E1%85%B3%E1%84%91%E1%85%B3%E1%84%85%E1%85%B5%E1%86%BC%20%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%84%83%E1%85%B5%203%20%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%201e5cbd752ed34d28b72530969277cc64/Untitled.png)

클라이언트에서 요청을 보내면 WAS를 통해 스프링 컨테이너의 Controller로 전달한다. Controller에서 요청을 받아 로직을 처리하고 다시 WAS를 통해 클라이언트에게 반환한다.

그렇다면 스프링 컨테이너에서는 요청을 어떻게 처리할까?

**스프링 MVC구조** (출처 : 김영한님 강의)

![Untitled](%E1%84%8F%E1%85%B2%E1%84%89%E1%85%B5%E1%84%8C%E1%85%B3%E1%86%B7%20%E1%84%89%E1%85%B3%E1%84%91%E1%85%B3%E1%84%85%E1%85%B5%E1%86%BC%20%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%84%83%E1%85%B5%203%20%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%201e5cbd752ed34d28b72530969277cc64/Untitled%201.png)

스프링 컨테이너에 요청이 들어오면 Dispatcher Servlet에서 요청된 URL에 따라 핸들러 매핑에 있는 핸들러를 조회한다.

핸들러 조회가 끝나면 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.

핸들러 어댑터를 찾은 뒤 핸들러 어댑터를 통해 핸들러를 호출하고 반환된 값을 뷰 리졸버에서 뷰로 변환한다.

반환된 뷰는 Dispatcher Servlet에서 뷰를 통해 반환한다.

## ❓Web server와 WAS의 차이

**Web Server**

클라이언트가 서버에 페이지 요청을 하면 요청을 받아 **정적 컨텐츠**를 제공하는 서버이다.

클라이언트에서 요청이 올 때 가장 앞에서 요청에 대한 처리를 한다.

**WAS**

Web Server의 역할 뿐만아니라 웹 컨테이너의 역할도 한다.

따라서 클라이언트가 DB조회같이 로직 처리가 요구되는 **동적 컨텐츠**를 제공하는 서버이다.

차이점은 웹 컨테이너의 유무이다 !

## ❓자바 프로그램의 실행 과정

자바는 JVM 덕분에 OS에 독립적으로 수행될 수 있다.

**JVM( JavaVirtual Machine)**

자바파일을 작성하면 확장자가 .java로 생성된다.

java파일은 컴파일러를 통해 컴파일 된 후 확장자가 .class인 자바 바이트 코드로 변환하게 된다.

컴파일 된 파일은 Class Loader를 통해 로드되고 Execution Engine을 통해 해석된다.

해석된 프로그램은 Runtime Data Area에 배치되어 수행이 이뤄진다.

- **Runtime Data Area**
    
    
    Method Area
    
    모든 Thread에 공유되며 클래스, 변수, method, static변수 등의 정보를 저장한다.
    
    Heap Area
    
    모든 Thread에 공유되며 new 명령어로 생성된 인스턴스 객체가 저장되는 구역이다.
    
    공간이 부족하면 Garbage Collection이 실행된다.
    
    Stack Area
    
    각 Thread마다 하나씩 생성되며 method안에서 사용되는 매개변수, 지역변수, 리턴값 등을 저장하는 구역
    
    PC Register
    
    각 Thread마다 하나씩 생성되며 현재 수행중인 JVM명령의 주소값이 저장된다.
    
    Native Method Stack
    
    각 Thread마다 하나씩 생성되며 다른 언어의 method 호출을 위해 할당되는 스택 구역.