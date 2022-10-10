# 큐시즘 백엔드 스터디 5주차 개인 정리

# 1. Rest API는 무엇인가

일단 Rest의 정의를 알아보자.

## REST의 정의

- “Representational State Transfer”의 약자로, 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다. 즉, **자원(resource)의 표현(representation)**에 의한 **상태 전달**이다.
- **자원(resource)의 표현(representation)**에서 **자원**은 해당 소프트웨어가 관리하는 모든것(문서, 그림, 데이터, 해당 소프트웨어 자체 등)을 의미하고, **자원의 표현**은 그 자원을 표현하기 위한 이름(DB의 학생 정보가 자원일 때, ‘students’를 자원의 표현)이라고 한다.
- 상태 전달은 데이터가 요청되어지는 시점에서 자원의 상태를 전달한다. JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
- REST는 네트워크상에서 Client와 Server 사이의 통신 방식 중 하나이다.

## REST의 구체적인 개념

- HTTP URI을 통해 자원을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
- 즉, REST는 자원 기반의 구조 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍처를 의미한다.
- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URL을 부여한다.

<aside>
💡 CRUD Operation 이란

CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인
Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로
REST에서의 CRUD Operation 동작 예시는 아래와 같다.

Create: 데이터 생성(POST)
Read: 데이터 조희(GET)
Update: 데이터 수정(PUT, PATCH)
Delete: 데이터 삭제(DELETE)

</aside>

## REST 구성 요소

REST는 다음과 같은 3가지로 구성이 되어있다.

1. 자원(Resource): HTTP URI
    - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
2. 자원에 대한 행위(Verb): HTTP Method
    - HTTP 프로토콜의 Method를 사용한다.
    - HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공한다.
3. 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load
    - Client가 자원의 상태에 대한 조작을 요청하면 Server는 이에 적절한 응답을 보낸다.
    - REST에서 하나의 자원은 JSON, XML 등 여러 형태의 Representation으로 나타내어 질 수 있다.
    - JSON, XML을 통해 데이터를 주고 받는 것이 일반적이다.

## REST의 장단점

### 장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

### 단점

- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.

## REST가 필요한 이유

- 애플리케이션 분리 및 통합
- 다양한 클라이언트의 등장
- 최근의 서버 프로그램은 다양한 브라우저와 안드로이드폰, 아이폰과 같은 모바일 디지바이스에서도 통신을 할 수 있어야 한다.
- 이러한 멀티 플랫폼에 대한 지원을 위해 서비스 자원에 대한 아키텍처를 세우고 이용하는 방법을 모색한 결과, REST에 관심을 가지게 되었다.

## REST의 특징

1. Seerver-Client(서버-클라이언트 구조)
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Uniform Interface(인터페이스 일관성)

## REST API의 개념

### API(Application Programming Interface)란?

- API란 클라이언트가 리소스를 요청할 수 있도록 서버측에서 제공된 인터페이스(Interface)를 말한다.
- 이러한 API로 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며 서로 정보를 교환가능 하도록 한다.
- 서버를 개발한다는 것은 API에 대한 마스터 권한이 생기는 것이다.
- API와 함께 항상 메뉴얼고 제공되어야 한다. URI를 모르면 클라이언트는 사용할 수 없다.

## REST API의 특징

- 사내 시스템들도 REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
- 즉, REST API를 제작하면 델파이 클라이언트 뿐 아니라 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.

## REST API 설계

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
2. 마지막에 슬래시(/)를 포함하지 않는다.
3. 언더바 대신 하이폰을 사용한다.
4. 파일확장자는 URI에 포함하지 않는다.
5. 행위를 포함하지 않는다.

# 2. 스프링에서 Restful API 개발을 위해 지원하는 것은 무엇이 있고, 그 특징은 무엇인가?

스프링에서는 @RestController 와 @X-Mapping 을 지원한다.

@RestController는 기존의 @Controller와 @ResponseBody 어노테이션을 함께 사용한 것과 같다. @RestController는 Restful API에 최적화된 Controller이다. 대부분의 REST API에서 Data를 응답할 땐 JSON 형태로 처리를 한다. 위처럼 단순 문자열을 return 할 때는 일반 문자열로 처리를 하지만, return을 Java Object로 할 경우 JSON 형태와 동일하기 때문에 자동으로 타입 변환을 하여 처리하게 된다. 결과적으로 JSON으로 변환된 Data를 확인할 수 있다.

@GetMapping은 @Requestmapping(value=”/”, method=RequestMethod.GET) 어노테이션을 사용한 것과 같다.

# 3. 클라이언트-서버 구조의 프로젝트 아키텍처를 그려보고, 데이터 흐름 살펴보기

클라이언트 서버 아키텍처는 서버간의 통신을 **서버와, 클라이언트로 분리시킨 설계방식**으로, 이러한 설계방식을 **2티어 아키텍처, 또는 클라이언트-서버 아키텍처**라고 부른다.

또한 이러한 2티어 아키텍처에 **데이터베이스가 추가된 설계방식을 3티어 아키텍처**라 부른다.

## 2-Tier 아키텍처

클라이언트와 서버는 요청과 응답을 주고 받는 관계이며, **반드시 요청 후에 응답이 오며**, 요청하지도 않았는데 응답이 오는 경우는 없다.

![images-estell-post-82f932ad-c278-42b9-b602-c4084e5bd7af-스크린샷 2022-01-11 오전 11.15.51.png](%E1%84%8F%E1%85%B2%E1%84%89%E1%85%B5%E1%84%8C%E1%85%B3%E1%86%B7%20%E1%84%87%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A6%E1%86%AB%E1%84%83%E1%85%B3%20%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%84%83%E1%85%B5%205%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20%E1%84%80%E1%85%A2%E1%84%8B%E1%85%B5%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20ba672e38bfef476abc8fc6d7170bcdeb/images-estell-post-82f932ad-c278-42b9-b602-c4084e5bd7af-%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-01-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.15.51.png)

## 3-Tier 아키텍처

 

![images-estell-post-ed29991c-c513-465c-8219-4f330d780aad-스크린샷 2022-01-11 오전 11.15.04.png](%E1%84%8F%E1%85%B2%E1%84%89%E1%85%B5%E1%84%8C%E1%85%B3%E1%86%B7%20%E1%84%87%E1%85%A2%E1%86%A8%E1%84%8B%E1%85%A6%E1%86%AB%E1%84%83%E1%85%B3%20%E1%84%89%E1%85%B3%E1%84%90%E1%85%A5%E1%84%83%E1%85%B5%205%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20%E1%84%80%E1%85%A2%E1%84%8B%E1%85%B5%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20ba672e38bfef476abc8fc6d7170bcdeb/images-estell-post-ed29991c-c513-465c-8219-4f330d780aad-%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-01-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.15.04.png)

# 4. HTTP 1.0 vs HTTP 2.0 vs HTTPS 각각의 차이는 무엇인가

## HTTP 1.0

- 브라우저에 친화적인 프로토콜
- 요청 및 응답에 대한 메타 데이터를 포함하는 헤더 필드 제공
- Response: Content-Type에 Http 파일 외에도 스크립트, 스타일 시트, 미디어 등을 전송 가능
- Method: GET, HEAD, POST
- Connection 특성: 응답 직후 종료

## HTTP 2.0

- 한 Connection으로 동시에 며러 개 메시지를 주고 받을 수 있으며, Response는 순서에 상관없이 stream 으로 주고 받는다.
- 리소스간 우선순위를 설정해 클라이언트가 먼저 필요한 리소스부터 보내준다.(Server Push)
- 서버는 클라이언트의 요청에 대해 요청하지 않은 리소스를 마음대로 보내줄 수 있다.
- 즉, 클라이언트가 요청하기 전에 필요하다고 예상되는 리소스를 Server에서 먼저 요청한다.

# Go Study

## 1. for 문

예제를 통해서 for문의 동작을 살펴보겠다.

```go
package main

import "fmt"

func main() {
	for i := 0; i < 10; i++ { //1
		fmt.Println(i, ", ")
	}
}
```

0부터 9까지의 숫자를 for문 이용해 출력한다.

1초마다 한 번씩 1씩 증가하는 숫자를 무한히 출력하는 예문을 하나 살펴보자.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	i := 1
	for {
		time.Sleep(time.Second)
		fmt.Println(i)
		i++
	}
}
```

## 2. continue와 break

continue와 break는 반복문을 제어하는 키워드이다. continue는 이후 코드 블록을 수행하지 않고 곧바로 후처리를 하고 조건문 검사부터 다시 하고, break는 for문에서 곧바로 빠져나온다.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	stdin := bufio.NewReader(os.Stdin)
	for { 								//무한루프
		fmt.Println("입력하세요.")
		var number int
		_, err := fmt.Scanln(&number) 	//한 줄 입력을 받는다.
		if err != nil {               	//숫자가 아닌 경우
			fmt.Println("숫자를 입력하세요.")

			//키보드 버퍼를 비운다.
			stdin.ReadString('\n') //키보드 버퍼를 지워준다.
			continue               		 //무한 루프로 돌아간다.
		}
		fmt.Println("입력하신 숫자는 %d 입니다.\n", number)
		if number%2 == 0 { 				//짝수 검사를 한다.
			break 						//for문을 종료한다.
		}
	}
	fmt.Println("for문이 종료됐습니다.")
}
```

## 3. 중첩 for문

for문을 중첩해서 사용할 수 있다. 한 번 이상 중첩해 사용한 for문을 중첩 for문이라고 한다. 중첩된 for문 예제를 살펴보겠다.

```go
package main

import "fmt"

func main() {
	for i := 0; i < 3; i++ {
		for j := 0; j < 5; j++ {
			fmt.Println("*")
		}
		fmt.Println()
	}
}
```

i 가 3보다 작으면 반복하는 for문이다. j가 5보다 작으면 반복하는 for문이다. j를 0으로 초기화하므로 총 5회 반복되어 *를 5번 출력한다. 5회가 실행되면 j가 5가 되어서 안쪽 반복문이 종료되고 다음 행인 fmt.Println() 함수를 호출하여 줄바꿈을 한다.

조금 복잡하므로 이중 for문을 사용한 다른 예제를 살펴보겠다.

```go
package main

import "fmt"

func main() {
	for i := 0; i < 5; i++ {
		for j := 0; i < i+1; j++ {
			fmt.Print("*")
		}
		fmt.Println()
	}
}
```

1개부터 5개까지 각 줄마다 하나씩 증가하는 별을 출력한다.

중첩 for문에서 break와 continue를 사용하면 continue와 break가 속한 코드 블록의 for문 끝으로 가거나 곧바로 for문을 빠져나가게 된다. 예제를 살펴보겠다.

```go
package main

import "fmt"

func main() {
	dan := 2
	b := 2
	for {
		for {
			fmt.Printf("%d * %d = %d\n", dan, b, dan*b)
			b++
			if b == 10 {
				break
			}
		}
		b = 1
		dan++
		if dan == 10 {
			break
		}
	}
	fmt.Println("for문이 종료됐습니다.")
}
```

for문을 중첩해서 구구단을 출력했다.

## 4. 중첩 for문과 break, 레이블

앞서 예제에서 보면 중첩 for문에서 break를 사용하면 break가 속한 for문에서만 빠져나온다. 모든 for문을 빠져나가고 싶을 때는 어떻게 해야할까? 첫 번째 방법은 불리언 변수를 사용하는 것이다. 예제를 통해 보겠다.

```go
package main

import "fmt"

func main() {
	a := 1
	b := 1
	found := false
	for ; a <= 9; a++ {
		for b = 1; b <= 9; b++ {
			if a*b == 45 {
				found = true
				break
			}
		}
		if found {
			break
		}
	}
	fmt.Printf("%d * %d = %d\n", a, b, a*b)
}
```

1~9 사이의 두 수를 곱했을 때 45가 되는 수를 찾는다. 안쪽 for문에서 두 수의 곱이 45가 되는 경우를 찾았다면 found 변숫값을 true로 바꾸고 break해서 안쪽 for문을 종료한다. 이때 바깥쪽 for문은 종료되지 않은 상태이기 때문에 found가 true인지 검사해서 바깥쪽 for문까지 거사해줘야 한다. 이런 형태로 불리언 변수를 사용하는 것을 깃발처럼 올라갔는지 내려갔는지를 표시한다고 해서 플래스 변수라고 한다.

플래그 변수를 사용하는 게 때로는 번거로울 수 있다. 이중 for문이 아니라 삼중 사중이면 더 복잡할 것이다. 다른 방법을 찾는다면 바로 레이블을 이용한 방법이다. for문을 시작할 때 레이블을 정의하고 break할 때 앞서 정의한 레이블을 적어주면 그 레이블에서 가장 먼저 속한 for문까지 모두 종료하게 된다. 예제로 살펴보겠다.

```go
package main

import "fmt"

func main() {
	a := 1
	b := 1

OuterFor:
	for ; a <= 9; a++ {
		for b = 1; b <= 9; b++ {
			if a*b == 45 {
				break OuterFor
			}

		}
	}
	fmt.Printf("%d * %d = %d\n", a, b, a*b)
}
```

레이블을 사용하는 방법이 편리할 수 있으나 혼동을 불러일으킬 수 있고 자칫 잘못 사용하면 예기치 못한 버그가 발생할 수 있다. 그래서 되도록 플래그를 사용하고 레이블은 꼭 필요한 경우에만 사용하기를 권장한다. 클린 코드를 지향하려면 중첩된 내부 로직을 함수로 묶어 중첩을 줄이고, 플래그 변수나 레이블 사용을 최소화해야 한다.

함수를 사용해 반복문 중첩과 레이블이 없는 깔끔한 코드로 수정하자.

```go
package main

import "fmt"

func find45(a int) (int, bool) {
	for b := 1; b <= 9; b++ {
		if a*b == 45 {
			return b, true
		}
	}
	return 0, false
}
func main() {
	a := 1
	b := 0

	for ; a <= 9; a++ {
		var found bool
		if b, found = find45(a); found {
			break
		}
	}
	fmt.Printf("%d * %d = %d\n", a, b, a*b)
}
```

## 5.  배열

배열은 같은 타입의 데이터들로 이루어진 타입이다. 배열을 이루는 각 값은 요소라고 하고 요소를 가리키는 위치값을 인덱스라고 한다.

예를 들어 최근 5일간 기온 데이터가 있다고 하자.

날짜마다 변수 하나를 사용해도 되겠지만 배열을 사용하면 변수를 하나만 할당해도 된다. 배열은 데이터 타입 앞에 [ ] 를 붙여 만든다.

```go
var 변수명 [요소 개수]타입
```

날짜별 온도가 소수점까지 제공되므로 최근 5일 온도 데이터를 저장하는 배열 변수 t를 다음과 같이 선언할 수 있다.

```go
var t [5]float64
```

예제를 살펴보며 사용법을 익혀보겠다.

```go
package main

import "fmt"

func main() {
	var t [5]float64 = [5]float64{24.0, 25.9, 27.8, 26.9, 26.2}

	for i := 0; i < 5; i++ {
		fmt.Println(t[i])
	}
}
```

배열 변수의 선언, 초기화, 각 요소의 읽기, 쓰기 등에 대해 알아보자.

배열 요소에 접근하여 값을 읽고 쓰려면 배열 변수에 대괄호 [ ]를 쓰고 그 사이에 접근하고자 하는 요소의 인덱스를 적는다.

```go
package main

import "fmt"

func main() {
   nums := [...]int{10, 20, 30, 40, 50}

   nums[2] = 300

   for i := 0; i < len(nums); i++ {
      fmt.Println(nums[i])
   }
}
```

for 반복문에서 range 키워드를 이용하면 배열 요소를 순회할 수 있다.

```go
package main

import "fmt"

func main() {
   var t [5]float64 = [5]float64{24.0, 25.9, 27.8, 26.9, 26.2}

   for i, v := range t {
      fmt.Println(i, v)
   }
}

```

## 6. 배열 복사

```go
package main

import "fmt"

func main() {
   a := [5]int{1, 2, 3, 4, 5}
   b := [5]int{500, 400, 300, 200, 100}

   for i, v := range a {
      fmt.Printf("a[%d] = %d\n", i, v)
   }

   fmt.Println()
   for i, v := range b {
      fmt.Printf("b[%d] = %d\n", i, v)
   }

   b = a
   fmt.Println()
   for i, v := range b {
      fmt.Printf("b[%d] = %d\n", i, v)
   }
}
```

이중 배열을  초기화하는 예제를 살펴보자.

```go
package main

import "fmt"

func main() {

   a := [2][5]int{
      {1, 2, 3, 4, 5},
      {5, 6, 7, 8, 9},
   }
   for _, arr := range a {
      for _, v := range arr {
         fmt.Print(v, " ")
      }
   }
}

```

## 7. 구조체

여러 필드를 묶어서 하나의 구조체를 만든다. 배열이 같은 타입의 값들을 변수 하나로 묶어줬던 것과 달리 구조체는 다른 타입의 값들을 변수 하나로 묶어주는 기능이다.

구조체는 다음과 같은 형식으로 정의한다.

```go
type 타입명 struct {
	필드명 타입
	...
	필드명 타입
}
```

학생 구조체를 한번 정의해보겠다.

```go
type Student struct {
	Name string
	Class int
	No int
	Score float64
}
```

Student 구조체를 정의했다. 이제 Student 타입을 int나 float64 같은 내장 타입처럼 선언해 사용할 수 있다.

Student 타입 구조체 변수를 선언해보겠다.

```go
var a Student
```

구조체를 정의하고 사용하는 예제를 살펴보겠다.

```go
package main

import "fmt"

type House struct {
   Address string
   Size    int
   Price   float64
   Type    string
}

func main() {
   var house House
   house.Address = "서울시 강동구..."
   house.Size = 28
   house.Price = 9.8
   house.Type = "아파트"

   fmt.Println("주소:", house.Address)
   fmt.Printf("크기: %d평\n", house.Size)
   fmt.Printf("가격: %.2f억 원\n", house.Price)
   fmt.Println("타입:", house.Type)
}
```

구조체 변수를 선언하고 각 필드를 초기화하는 방법을 알아보겠다. 초깃값 생략, 모든 필드 초기화, 일부 필드 초기화 방법이 있다. 앞에서 살펴본 예제를 계속 활용해보겠다.

초깃값을 생략하면 모든 필드가 기본값으로 초기화된다.

```go
var house House
```

모든 필드값을 중괄호 사이에 넣어서 초기화한다. 모든 필드가 순서대로 초기화한다.

```go
var house House = House("서울시 강동구", 28, 9.80. "아파트"}
```

첫 필드는 Address 이다. “서울시 강동구” 가 입력된다. 나머지도 필드 순서와 입력한 순서에 맞춰 일대일 매칭되어 초기화된다.

아래와 같이 여러 줄에 걸쳐서 초기화할 수 있다.

```go
var house House = House {
	"서울시 강동구",
	28,
	9.80
	"아파트",
}
```

일부 필드값만 초기화할 때는 ‘필드명:필드값’ 형식으로 초기화했다. 초기화되지 않은 나머지 변수에는 기본값이 할당된다.

```go
var house House = House{Size : 28, Type:"아파트"}
```

Size와 Type 필드값만 초기화했다. Address 와 Price는 기본값인 빈 문자열과 0.0이 할당되었다. 아래처럼 여러 줄에 걸쳐서 초기화할 수 있다.

```go
var house House = House {
	Size: 28,
	Type: "아파트",
}
```

## 8. 구조체를 포함하는 구조체

구조체의 필드로 다른 구조체를 포함할 수 있다. 일반적인 내장 타입처럼 포함하는 방법과 포함된 필드 방식이 있다.

예를 들어보겠다.

```go
type User struct { //일반 고객 정보
	Name string
	ID string
	Age int
}

type VIPUser struct { //VIP 고객 정보
	UserInfo User
	VIPLevel int
	Price int
}
```

위와 같이 일반 고객 정보를 나타내는 User 구조체와 VIP 고객 정보를 나타내는 VIPUser 구조체가 있다고 하자. VIP 고객도 고객이므로 이름, ID, 연령 정보를 입력할 변수를 각각 선언하지 않고 이미 만들어 사용하는 일반 고객 정보용 User 구조체를 활용하는 방법이 더 깔끔해 보인다. 방법은 간닪다. VIPUser 구조체 필드로 User 구조체를 포함하도록 정의하면 그만이다. 

일반 고객과 VIP용 고객 정보 구조체를 만들고 출력해보자.

 

```go
package main

import "fmt"

type User struct {
	Name string
	ID   string
	Age  int
}

type VIPUser struct {
	UserInfo User
	VIPLevel int
	Price    int
}

func main() {
	user := User{"송하나", "hana", 23}
	vip := VIPUser{
		User{"화랑", "hwarang", 40},
		3,
		250,
	}

	fmt.Printf("유저: %s ID: %s 나이: %d\n", user.Name, user.ID, user.Age)
	fmt.Printf("VIP 유저: %s ID: %s skdl: %d VIP 레벨 %d VIP 가격: %d만 원\n ",
		vip.UserInfo.Name,
		vip.UserInfo.ID,
		vip.UserInfo.Age,
		vip.VIPLevel,
		vip.Price,
	)
}
```

vip에서 Name 이나 ID와 같이 UserInfo 안에 속한 필드를 접근하려면 vip.UserInfo.Name 과 같이 두 단계를 걸쳐 접근해야 한다. 구조체에서 다른 구조체를 필드로 포함할 때 필드명을 생략하면 .을 한 번만 찍어 접근할 수 있다. 이것을 이용해서 앞의 예제를 다시 써보겠다.

 

```go
package main

import "fmt"

type User struct {
	Name string
	ID   string
	Age  int
}

type VIPUser struct {
	User
	VIPLevel int
	Price    int
}

func main() {
	user := User{"송하나", "hana", 23}
	vip := VIPUser{
		User{"화랑", "hwarang", 40},
		3,
		250,
	}

	fmt.Printf("유저: %s ID: %s 나이: %d\n", user.Name, user.ID, user.Age)
	fmt.Printf("VIP 유저: %s ID: %s skdl: %d VIP 레벨 %d VIP 가격: %d만 원\n ",
		vip.Name,
		vip.ID,
		vip.Age,
		vip.VIPLevel,
		vip.Price,
	)
}
```

## 9. 구조체 크기

구조체 변수가 선언되면 컴퓨터는 구조체 필드를 모두 담을 수 있는 메모리 공간을 할당한다. 구조체가 차지하는 메모리 크기는 어떻게 알 수 있을까? 구조체 크기를 구하는 방법을 알아보자.

```go
type User struct {
	Age int
	Score float64
}
```

위와 같은 구조체 User가 정의되어 있다고 하자.

```go
var user User
```

User 구조체의 user 변수가 선언되면 컴퓨터는 Age와 score 필드를 연속되게 담을 수 있는 메모리 공간을 찾아 할당한다. int 타입 Age는 8바이트, float64 타입 Score 역시 8바이트이므로 총 16바이트 크기가 필요하다. 즉, 구조체 변수 user의 크기는 16바이트가 된다. 

구조체 변숫값을 다른 구조체에 대입하면 모든 필드값에 복사된다. 예제를 살펴보겠다.

```go
package main

import "fmt"

type Student struct {
	Age   int
	No    int
	Score float64
}

func PrintStudent(s Student) {
	fmt.Printf("나이:%d 번호:%d 점수:%.2f\n", s.Age, s.No, s.Score)
}

func main() {
	var student = Student{15, 23, 88.2}

	student2 := student

	PrintStudent(student2)
}
```

구조체 크기에 대해 더 알아보겠다.

```go
package main

import (
	"fmt"
	"unsafe"
)

type User struct {
	Age int32
	Score float64
}

func main() {
	user := User{23, 77.2}
	fmt.Println(unsafe.Sizeof(user))
}
```

## 10. 메모리 패딩

메모리 패딩으로 인해 생길 수 있는 문제를 알아보겠다.

```go
package main

import(
	"fmt"
	"unsafe"
)

type AA struct {
	A int8
	B int
	C int8
	D int
	E int8
}

func main()  {
	aa := AA{1, 2, 3, 4, 5}
	fmt.Println(unsafe.Sizeof(aa))
}
```

AA 구조체는 1바이트짜리 필드 3개와 8바이트짜리 필드 2개로 구성되어 있기 때문에 19바이트 크리를 차지합니다. 하지만 실제 구조체 크기는 메모리 패딩 때문에 40바이트가 된다. 1바이트 변수 A, C, E 모두 7바이트씩 패딩됐기 때문이다.
