# 큐시즘 스프링 스터디 6주차

### ❓Oauth 2.0은 무엇인가

타사의 사이트에 대한 접근 권한을 얻고 그 권한을 이용하여 개발할 수 있도록 도와주는 프레임워크다.

네이버, 카카오, 구글같은 사이트에서 로그인을 하면 구현된 사이트(클라이언트)에서도 인증을 받을 수 있다.

물론 로그인을 했다고 ID, PW을 직접 넘겨주면 보안에 위배되기 때문에 Access Token을 통해 기능구현을 해야한다.

- Access Token
    
    로그인하지 않고 인증을 할 수 있는 인증토큰이다.
    

### ❓Spring Security 는 무엇이고, 사용 목적은 무엇인가

일반적인 공격으로부터 보호를 해주고, 인증과 인가 기능을 제공하는 프레임워크이다.

Spring Security는 인증과 권한에 대한 부분을 Dispatcher Servlet 이전에 FIlter 흐름에 따라 처리하고 있다.

보안과 관련해서 체계적으로 많은 옵션을 제공하고 있기 때문에 개발자가 일일히 보안로직을 작성하지 않아도 된다.

### ❓Spring Security 를 사용해봤다면, 개발 프로세스와 아키텍처를 그려서 설명하기

사용한 경험은 없다.

<img src="./Images/goseungwon_1.png">

1. 로그인 정보와 함께 인증 요청
2. AuthenticationFilter가 요청을 가로채 UsernamePasswordAuthenticationToken 객체를 생성
3. AuthenticationManager의 구현체 ProviderManager에게 UsernamePasswordAuthenticaionToken 객체를 전달
4. AuthenticationProvider에 UsernamePasswordAuthenticationToken 객체 전달
5. UserDetailsService에 사용자 정보를 넘겨준다. UserDetailsService는 DB로부터 사용자 인증 정보를 가져오는 역할이다.
6. 넘겨 받은 정보를 통해 DB에서 찾아 사용자 정보인  UserDetails 객체를 생성한다.
7. AuthenticationProvider에서 UserDetails를 넘겨 받아 사용자 정보를 비교한다.
8. 인증이 완료 되면 Authentication 객체에 사용자 정보를 담아 반환.
9. 2번의 AuthenticationFilter에 Authentication 객체 반환.
10. Authentication 객체를 SecurityContext에 저장.

UserDetailsService에 loadUserByUsername()이라는 메서드가 UserDetails객체를 반환하는데, 이 반환되는 UserDetails를 비교함으로서 동작된다. 따라서 UserDetailsService와 UserDetails를 어떻게 구현하느냐에 따라 인증의 세부과정이 달라지게 된다.

### ❓Spring Security 를 사용해보지 않았다면, 어떻게 프로젝트에 적용할 것인지 개발 프로세스와 아키텍처를 그려서 설명하기

이번 밋업데이에서 Spring Security + JWT + OAuth2.0을 사용해 google로부터 정보를 받아올 계획이다. 아키텍처는 아직 모르겠다.

### ❓JWT 는 무엇인가

JSON Web Token의 약자로 정보를 JSON형식으로 안전하게 전송하기 위한 개방형 표준이다. JWT는 디지털 서명이 되어 있기 때문에 신뢰할 수 있다.

JWT는 대개 권한부여에 사용되어 사용자가 해당 토큰으로 서버 및 리소스에 엑세스 할 수 있게 한다. 오버헤드가 적고 여러 도메인에서 쉽게 사용할 수 있기 때문에 널리 사용된다.

그 외에도 정보교환을 위해서 사용할 수 있다. 공개/개인키를 통하여 JWT에 서명할 수 있기 때문에 발신자를 알 수 있으며, 콘텐츠가 변조되었는지 알 수 있다.

**❗ JWT구조**

- 헤더
    
    일반적으로 토큰 유형과 사용중인 암호화 알고리즘으로 구성된다.
    
- 페이로드
    
    클레임을 포함하는 페이로드입니다. 클레임은 일반적으로 사용자의 데이터를 담은 엔티티이며 등록된 클레임, 공개/비공개 클레임으로 나뉜다.
    
- 서명
    
    서명을 생성하기 위해서는 인코딩된 헤더, 인코딩된 페이로드, 암호, 헤더에 지정된 알고리즘으로 서명한다.
    

위 세가지를 모두 합쳐 인코딩하여 “ . ” 으로 구분한다.

질문하고싶은것 

spring security + jwt + oauth2 위 세가지를 사용한 사람에게 플로우를 물어보고 싶다.
그리고 각 프레임워크에서 구현해야 할 부분이 무엇이 있는지 어느정도 커스텀 되는지?

### ❓SSL은 무엇인가

- SSL
    
    Secure Sockets Layer의 약자로 암호화 기반 인터넷 보안 프로토콜을 뜻한다.
    
    전달되는 모든 데이터를 암호화하고 특정 유형의 공격도 차단한다. SSL은 TLS 암호화의 전신이기도 하다.
    

- TLS
    
    Transport Layer Security의 약자로 SSL의 업데이트 버전이라고 생각하면 편하다.
    
    데이터를 암후화하고 handshake을 통해 인증이 이루어진다. 또한 데이터 무결성을 위해 데이터에 디지털 서명을 하여 데이터가 도착 전에 조작된지 확인한다.
    

### ❓SSL을 적용했을 때의 아키텍처를 그려서 설명하기

사실 이 부분에 관하여 여러 블로그를 봤지만 적용하는법만 많이 나오지 개념적인 부분은 하나도 모르겠다.

[https://velog.io/@code12/Spring-Security-SSLHTTPS-사용하기](https://velog.io/@code12/Spring-Security-SSLHTTPS-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)