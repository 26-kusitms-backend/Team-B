- **스프링 빈 객체는 무엇인가**
    
    스프링 컨테이너(Application Context) 가 관리하는 객체를 말한다. ioc 또는 di 를 수행할 때 스프링 컨테이너가 직접 객체를 생성하여 의존성을 주입해준다.
    
- **스프링에서 빈 객체를 관리하는 방법은 무엇인가**
    
    스프링에서 빈객체를 등록하는 방법은 2가지가 있다.
    
    우선 첫번째, 컴포넌트 방식이다. @Component 어노테이션이 붙은 클래스에 대하여 어플레케이션이 로딩되는시점에 스프링 컨테이너가 객체를 생성하여 스프링 빈으로 등록한다. 의존성 주입이 필요한 시점에 해당 스프링 빈을 스프링 컨테이너가 찾아서 주입해준다. (컴포넌트 스캔)
    
    두번째, 설정클래스를 사용한 등록방법이다. @Configuration 이 붙은 클래스는 스프링컨테이너가 스프링 설정 클래스로 인식한다. 이 설정클래스 내에서 @Bean이 붙은 메소드의 반환값을 스프링 빈으로 등록한다. 이때 @Bean이 붙은 스프링 빈객체들은 모두 싱글톤으로 관리된다. 
    
- **싱글톤 디자인 패턴은 무엇인가**
    
    싱글톤 디자인 패턴은 특정 객체에 대한 인스턴스를 단 하나 만 생성하여 관리하는 것을 말한다. 동일한 요청이 서로 다른 클라이언트로부터 동시에 들어올 수 있다. 이 때, 각 요청마다 새로운 인스턴스를 생성한다면 메모리 낭비가 심할 것이다. 이를 해결하기 위해 스프링 컨테이너는 모든 스프링빈 객체를 싱글톤으로 관리한다. 
    
    ```java
    public class SingletonService {  //싱글톤으로 설계
        private static final SingletonService instance = new SingletonService();  //static 이라 메모리에 하나만 올라간다.
    
        public static SingletonService getInstance(){
            return instance;
        }
    
        private SingletonService(){  //private 생성자를 통해 외부에서 new 로 객체생성을 방지!
    
        }
    
        public void logic(){
            System.out.println("싱글톤 객체 로직 호출");
        }
    
        public static void main(String[] args) {
    
        }
    }
    ```
    
    객체 생성을 static으로 하여 메모리에 하나만 올라가도록 설계하고, 생성자를 private으로 설정하여 외부에서 객체를 생성할 수 없도록 설계되었다. 이 처럼 싱글톤으로 디자인된 객체는 어느시점에 호출되던간에 동일한 참조값을 가지고 있다.
    
- **스프링 MVC 기반 WAS의 아키텍처를 각자 그려보고 데이터 흐름 설명하기**
    
    <img src="/images/taemin-1.png">
    

**MVC : model view controller**

1. **Dispatcher servlet** → http 요청을 처리할 컨트롤러를 지정하는 front controller (super controller)

       (고객의 여러 요청을 한 곳에서 받고, 해당 요청에 해당하는 컨트롤러(핸들러)와의 매핑

        핸들러 어댑터를 사용하여 컨트롤러 호출)

1. 핸들러(컨트롤러) → http 요청을 처리하기 위한 데이터를 model에 담고, view와 함께 반환
2. ModelAndView를 받은 Dispatcher Servlet이 viewResolver를 호출하여 해당 view를 반환
3. model(data)가 담긴 view → http 응답

- **Web server와 WAS의 차이는 무엇인가**
    
    
    **Static Pages(정적 페이지)**
    
    - 항상 동일한 페이지를 반환한다
    - image, html, css 파일 등과 같이 컴퓨터에 저장되어 있는 파일들
    
    **Dynamic Pages(동적 페이지)**
    
    - 동적인 contents를 반환한다
    - 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물
    
    **Web Server**
    
    - 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 정적인 컨텐츠를 제공하는 컴퓨터 프로그램
    - HTTP프로토콜을 기반으로 하여 클라이언트의 요청을 서비스하는 기능을 담당
    - 기능
        - 정적인 컨텐츠를 제공하고, WAS를 거치지않고 바로 자원을 제공한다
        - 동적인 컨텐츠 제공을 위한 요청을 전달한다. 클라이언트의 요청(Request)을 WAS에 전달하고 WAS의 응답(Response)을 클라이언트에게 전달한다.
    
    **Web Application Server(WAS)**
    
    - DB 조회 등과 같이 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server
    - HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어이다
    - Web Server + Web Container의 역할을 한다
    - Container란 JSP, Servlet등을 실행시킬 수 있는 소프트웨어를 말하고, 현재는 WAS가 가진 Web Server도 정적인 컨텐츠를 처리하는데 성능상 큰 차이가 없다.
    - 기능
        - 프로그램 실행 환경과 DB접속 기능 제공
        - 여러개의 트랜잭션 관리 가능
        - 업무를 처리하는 비즈니스 로직 수행
    
    **Web Server 와 WAS를 분리하는 이유**
    
    - 기능을 분리하여 서버 부하 방지
        - WAS는 DB조회 등과 같이 다양한 로직 처리를 수행하는 역할이 크기 때문에 정적 컨텐츠는 Web Server에서 관리하는 것이 좋다
    - 물리적으로 보완하여 보완 강화
    - 여러대의 WAS를 연결 가능
    - Web Server를 WAS앞에 두면 효율적인 분산 처리가 가능하다
    - 정리하면, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 둘을 분리한다
