# 5 주차


- **Rest API는 무엇인가**
    
    Representational State Transfer (REST) api - http 설계를 최대한으로 활용하기 위해 발표된 아키텍쳐
    
    = URI를 통해 자원을 지정하고, HTTP method를 통해 행위를 표현하여 주고받는 API 아키텍쳐
    
    Rest api의 구성요소
    
    - 자원 - URI(Uniform **Resource** Identifier) 을 사용하여 자원을 지정 후 요청 (명사)
    - 행위 - HTTP method(get, post, put, delete) 을 사용하여 요청의 행위를 표현
    
           - form request의 경우, get,post만 지원하기 때문에 update or delete 요청의 경우 컨트롤uri 사용
    
              ex) /member/1/**update**
    
    - 표현 - client에 대한 응답을 json,xml,text등의 형태로 표현
    
    장점
    
    1. HTTP 의 인프라를 그대로 사용하므로 Rest api를 위한 별도의 인프라 구축 필요 x
    2. HTTP 표준을 따르는 모든 플랫폼에서 적용 가능
    3. Rest api 메시지가 의도하는 바를 명확히 알 수 있음
    4. 서버와 클라이언트를 명확히 구분 - 서버와 클라이언트간의 역할이 명확하기 때문에 서로의 의존성 감소
        
         + stateless 특성(클라이언트는 서버의 행위에 대해 신경쓰지 않아도 된다!)        
        
    
    단점
    
    1. 표준에 대한 정의가 필요
    
- **스프링에서 Restful API 개발을 위해 지원하는 것은 무엇이 있고, 그 특징은 무엇인가**
    
    스프링 MVC 내에서 @Get @Post @Put @Delete 등의 어노테이션 제공
    
    → http method를 지정하여 api 구축 가능
    
    @Pathvariable 어노테이션을 통해 uri에 id 값 표현 가능
    
    → uri를 통한 자원의 표현 가능
    
- **HTTP 1.0 vs HTTP 2.0 vs HTTPS 각각의 차이는 무엇인가**
    
    
    HTTP 1.0
    
    - 가장 초기의 HTTP
    - TCP 방식
    - GET, HEAD, POST
    - 3-way handshake(연결) → 요청-응답 → 4-way-handshake(연결해제)
    
            ⇒ connection당 하나의 요청 처리 가능
    
            ⇒ network latency 발생!
    
    HTTP 1.1
    
    - 현재 가장 많이 사용
    - TCP 방식
    - GET, HEAD, POST, PUT, PATCH, DELETE, TRACE 등
    - 3-way-handshake(연결) → 요청-여러응답(img,html,json등등) → 4-way-handshake(연결해제)
    
            ⇒ connection 안에서 여러요청-응답 가능 (일정시간동안)
    
            ⇒ 지속연결
    
            ⇒ but, TCP특성상 요청에 대한 응답 순서 보장 → 첫번째 응답 이미지파일 용량이 커서 지연이 발생 →             
    
                          두세번째 응답 지연 → network latency 발생!
    
    HTTP 2.0
    
    - TCP 방식
    - HTTP 1.1의 문제점 해결
    - Multiplexed Streams - 요청 순서에 상관없이 응답 가능
    - Stream Prioritization - 응답 우선순위 지정 가능
    - Server Push - 클라이언트의 요청 없이 응답 가능 (html요청 하나 → html, js, img, css 모두 응답)
    
             ⇒ HTTP 1.1 에서 발생한 network latency 해결!
    
    HTTP 3.0
    
    - UDP 방식 - 비연결형 방식
    
    HTTPS (secure)
    
    - (HTTP + 데이터 암호화) 프로토콜
    - 대칭키 암호화 방식, 비대칭 암호화 방식

```
1) 클라이언트(브라우저)가 서버로 최초 연결 시도를 함
2) 서버는 공개키(엄밀히는 인증서)를 브라우저에게 넘겨줌
3) 브라우저는 인증서의 유효성을 검사하고 세션키를 발급함
4) 브라우저는 세션키를 보관하며 추가로 서버의 공개키로 세션키를 암호화하여 서버로 전송함
5) 서버는 개인키로 암호화된 세션키를 복호화하여 세션키를 얻음
6) 클라이언트와 서버는 동일한 세션키를 공유하므로 데이터를 전달할 때 세션키로 암호화/복호화를 진행함
   -> 세션키 = 대칭키
```

출처:

[https://mangkyu.tistory.com/98](https://mangkyu.tistory.com/98)
