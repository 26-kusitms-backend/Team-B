# 큐시즘 백엔드 스터디 4주차 개인정리

# Spring Study

## 1. 프로세스와 스레드는 각각 무엇이고, 둘의 차이는 무엇인가

먼저 프로세스와 스레드를 알아보기 전에 **프로그램** 이라는 개념에 대해 알아야 한다.

### 프로그램

프로그램이란, 파일이 저장 장치에 저장되어 있지만 **메모리에는 올라가지 않은 정적인 상태**를 말한다. 즉, 메모리에 올라가지 않는 상태이기 때문에 **아직 운영체제로부터 독립적인 메모리 공간을 할당받지 않은 상태**이며 정적인 상태이기 때문에 아직 실행되지 않고 가만히 있는 상태이다.

즉, 그냥 코드 덩어리라고 할 수 있다.

### 프로세스

프로세스란, **컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램**이다. 컴퓨터에서 실행되고 있다는 것은 **운영체제로부터 독립적인 메모리 공간을 할당**받았다는 것이며, 실행되고 있기 때문에 **동적인 상태**이다.

아래 그림은 프로세스를 나타낸 모습이다.


프로세스의 각각의 영역에 대한 설명을 해보겠다.

- **Code**: 코드 자체를 구성하는 메모리 영역(프로그램 명령)
- **Data**: 전역 변수, 정적, 변수, 배열 등의 데이터(초기화된 데이터)
- **Stack**: 지역 변수, 매개 변수, 리턴 값 등의 임시 데이터(임시 메모리 영역)
- **Heap**: 동적 할당 시 사용(new(), malloc() 등)


즉, **프로그램**은 코드 덩어리 파일이며 이 프로그램을 실행한 것이 바로 **프로세스** 이다.

프로세스의 특징은 다음과 같다.

- 프로세스는 **각각 독립된 메모리 영역**(Code, Data, Stack, Heap)을 가진다.
- 프로세스당 **최소 1개의 스레드**(메인 스레드)를 가지고 있다.
- 각각의 프로세스는 별도의 주소 공간에서 실행되고, 기본적으로 **서로 다른 프로세스의 자원에 접근할 수 없다.**(운영체제에서 안정성을 위해 이렇게 설계됨)
- 다른 프로세스의 자원에 접근하기 위해서는 **프로세스 간 통신(IPC)**을 이용해야 한다.

### 스레드

스레드란, **프로세스 내에서 실행되는 여러 흐름의 단위**이다. 즉, 프로세스의 특정한 수행 경로이며, 프로세스가 할당받은 자원을 이용하는 실행의 단위이다.

- 프로세스 내에서 **Stack**만 따로 할당받고, Code, Data, Heap 영역은 공유하는 스레드이다.


**스레드**는 프로세스의 공유적인 한계점을 극복하기 위해 만들어지 개념이기 때문에, 프로세스와는 달리 **서로 메모리를 공유**한다. 즉, 스레드 끼리는 프로세스의 자원을 공유하며 프로세스 실행 흐름의 일부가 되는 것이다.

스레드의 특징은 다음과 같다.

- 스레드는 프로세스 내에서 **Stack**만 따로 할당받고 **Code, Data, Heap** 영역은 공유한다.
- 프로세스 내의 주소 공간이나 자원들을 **같은 프로세스 내의 스레드 끼리 서로 공유**한다.
- 한 스레드가 프로세스 자원을 변경하면, 다른 스레드도 그 변경된 결과를 즉시 볼 수 있다.

지금까지 살펴본 **프로그램**과 **프로세스**와 **스레드**의 개념을 코드에 비유하여 정리해보면

- **프로그램**은 아직 실행되지 않은 실행 파일로써 코드 덩어리라고 볼 수 있고
- **프로세스**는 이러한 프로그램(코드 덩어리)을 실행한 것이라고 볼 수 있고
- **스레드**는 이 코드의 일부분(main문, 함수 등)으로 볼 수 있다.

프로세스와 스레드의 차이를 설명하자면 다음과 같이 말할 수 있을것 같다.

프로세스는 **운영체제로부터 자원을 할당받는 작업의 단위**이며, 스레드는 **할당 받은 자원을 이용하는 실행의 기본 단위**이고 프로세스 내에 여러개 생길 수 있다. 어플리케이션 하나가 프로세스라면, 그 안에서의 분기 처리가 스레드가 되는 셈이다.

## 2. 스프링은 멀티 스레딩 방식으로 동작하는데, 이들 간의 Thread-safe 할 수 있는 이유는 무엇인가

일반적으로 하나의 프로세스는 하나의 스레드를 가지고 작업을 수행하게 된다. 하지만 스프링은 멀티 스레딩 방식으로 동작하는데, 멀티 스레딩은 싱글 스레딩과 무엇이 다를까?

### 멀티 스레드(Multi Thread)

멀티 스레드는 CPU의 최대 활용을 위해 프로그램의 둘 이상을 동시에 실행하는 기술이다. 즉, 여러 개의 CPU를 사용하여 여러 프로세스를 동시에 수행하는 것을 의미한다.


이러한 작업은 컨텍스트 스위칭(Context Switching)을 통해서 이뤄진다. 하나의 스레드에서 다음 스레드로 이동을 하면서, 컨텍스트 스위칭이 일어난다. 그리고, 스위칭이 일어나면서 부분적으로 조금씩 조금씩 각각의 스레드에 대한 작업을 끝나게 된다.

다시 말해서, 컨텍스트 스위칭이 엄청 빠르게 일어나면서, 유저의 시선에서는 프로그램들이 동시에 수행되는 것처럼 보인다.

### 멀티 스레드의 장점

- 응답성: 프로그램의 일부분(스레드 중 하나)이 중단되거나 긴 작업을 수행하더라도 프로그램의 수행이 계속 되어 사용자에 대한 응답성이 증가한다.
- 경제성: 프로세스 내 자원들과 메모리를 공유하기 때문에 메모리 공간과 시스템 자원 소모가 줄어든다.
- 멀티프로세서의 활용: 다중 CPU 구조에서는 각각의 스레드가 다른 프로세서에서 병렬로 수행될 수 있으므로 병렬성이 증가한다.

### 멀티 스레드의 장점

- 컨텍스트 스위칭, 동기화 등의 이유 때문에 싱글 코어 멀티 스레딩은 스레드 생성 시간이 오히려 오버헤드로 작용해 단일 스레드보다 느리다.
- 공유하는 자원에 동시에 접근하는 경우, 프로세스와는 달리 스레드는 데이터와 힙 영역을 공유하기 때문에 어떤 스레드가 다른 스레드에서 사용 중인 변수나 자료구조에 접근하여 엉뚱한 값을 읽어오거나 수정할 수 있다. 따라서 동기화가 필요하다.
- 멀티 스레딩을 위해서는 운영체제의 지원이 필요하다.

### Spring에서의 Thread-safe(???)

Spring은 기본적으로 Thread-safe 하지 않은 환경이라고 한다. 즉, Spring 자체는 Thread-safe 하지 않지만, 우리가 흔히 널리 퍼져 알고 있는 Spring의 사용 방법이 바로 Thread-safe 한 방법이기 때문에, 이에 대한 문제를 느낄 수 없었다는 것이다.

## 3. 스프링 프로젝트를 실행 Run 하면 그 시작점은 어디인가

스프링 부트의 핵심은 아래와 같다.

<aside>
💡 build.gradle 파일 내용에 따라 클래스 패스, 어노테이션, 기타 자바 구성 클래스를 보고 적합한 앱으로 맞춤하는 자동구성

</aside>

스프링부트의 시작점은 [Application.java](http://Application.java) 파일이다.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringProjectApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringProjectApplication.class, args);
    }
}
```

먼저 @SpringBootApplication 어노테이션에 주목해야 한다.

### @SpringBootApplication

이 어노테이션은 @Configuration, @EnableAutoConfiguration, @ComponentScan 이 합쳐진 것이다. 이를 통해 전체 Application Component를 식별한다.

따라서 순서를 보면

1. classPath를 조사해서 spring-boot-starter-seb 스타터가 선언된 것을 인지한 스프링부트는 웹애플리케이션을 구성한다.
2. 어노테이션(@RestController, @Server, @Repository, …)를 스캔한다.
3. spring-boot-starter-web 의존체 중 하나인 톰캣으로 앱을 띄운다.

# Go Study

## 1. 상수 선언

상수는 변하지 않는 값을 말한다. 변수는 대입문을 통해서 값을 수시로 바꿀 수 있지만 상수는 초기화된 값이 변하지 않는다. 정수, 실수, 문자열 등 기본 타입값들만 상수로 선언될 수 잇다. 

Go 언어에서 상수로 사용될 수 있는 타입은 다음과 같다.

- 불리언
- 정수
- 복소수
- 룬(rune)
- 실수
- 문자열

상수 선언 방식은 변수와 비슷하다. 변수를 뜻하는 var 대신 상수를 뜻하는 const 키워드를 사용한다는 점이 다르다.

```go
const ConstValue int = 10
```

상수명 규칙은 변수명과 같다. 함수 외부에 선언되어 있고 첫 글자가 대문자인 상수는 패키지 외부로 공개되는 상수이다. 상수는 한 번 선언되면 그 값을 바꿀 수 없다. 상수는 값으로만 동작하기 때문에 대입문의 좌변에 올 수 없다.

## 2. 상수는 언제 사용하나?

상수로 변하지 않는 값에 이름을 부여하면 매번 값을 쓰지 않고 편리하게 이용할 수 있다. 

파이값을 상수와 변수로 선언해서 사용하는 예제를 살펴보자.

```go
package main

import "fmt"

func main() {
	const PI1 float64 = 3.1415926535
	var PI2 float64 = 3.1415926565

	PI2 = 4

	fmt.Println("원주율: %f\n", PI1)
	fmt.Println("원주율: %f\n", PI2)

}
```

상수를 코드값으로 사용할 수 있다. 코드값이란 어떤 숫자에 의미를 부여하는 것을 말한다. 

동물 코드값에 따라서 다른 문자열을 출력하는 예제를 살펴보자.

```go
package main

import "fmt"

const Pig int = 0
const Cow int = 1
const Chicken int = 2

func PrintAnimal(animal int)  {
	if animal == Pig{
		fmt.Println("꿀꿀")
	} else if animal == Cow{
		fmt.Println("음메")
	} else if animal == Chicken{
		fmt.Println("꼬끼오")
	}else {
		fmt.Println("...")
	}
}
func main() {
	PrintAnimal(Pig)
	PrintAnimal(Cow)
	PrintAnimal(Chicken)

}
```

## 3. 타입이 없는 상수

상수 선언 시 타입을 명시하지 않을 수 있다. 그러면 타입 없는 상수가 된다. 타입 없는 상숫값은 타입이 정해지지 않은 상태로 사용된다. 다음 예제를 보겠다.

```go
package main

import "fmt"

const PI = 3.14
const FloatPI float64 = 3.14

func main() {
	var a int = PI * 100
	var b int = FloatPI * 100
	
	fmt.Println(a)
	fmt.Println(b)

}
```

이 코드는 오류가 난다.

PI 값은 타입이 없기 때문에 3.14 숫자로만 동작한다. FloatPI는 float64 타입 상수이다. 값은 3.14이다. PI  * 100 은 3.14 * 100 으로 치환되어 계산된 결과 314이다. 314는 정수 타입 변수에 대입할 수 있다.

FloatPI는 float64 타입이기 때문에 타입 오류가 발생한다.

타입 없는 상수는 변수에 복사될 때 타입이 정해지기 때문에 여러 타입에 사용되는 상숫값을 사용할 때 편리하다.

## 4. if 문

if…else 예제를 살펴보자.

```go
package main

import "fmt"

func main() {
	light := "red"

	if light == "green" {
		fmt.Println("길을 건던다")
	} else {
		fmt.Println("대기한다.")
	}
}
```

else문은 생략 가능한다. else if 구문까지 사용하는 온도 조절장치 예제를 살펴보자.

```go
package main

import "fmt"

func main() {
	temp := 33

	if temp > 28 {
		fmt.Println("에어컨을 켠다")
	} else if temp <= 3 {
		fmt.Println("히터를 켠다")
	} else {
		fmt.Println("대기한다")
	}
}
```

## 5. 중첩 if 문

if문 안에 if문을 중접해 사용할 수도 있다. 복잡한 경우를 표현할 때 사용한다. 예를 들어 아래 예문을 if문 하나로 표현하기가 조금 어렵다.

```go
package main

import "fmt"

func HasRichFriend() bool {
	return true
}

func GetFriendCount() int {
	return 3
}

func main() {
	price := 35000

	if price > 50000 {
		if HasRichFriend() {
			fmt.Println("앗 신발끈이 풀렸네")
		} else {
			fmt.Println("나눠내자")
		}
	} else if price >= 30000 && price <= 50000 {
		if GetFriendCount() > 3 {
			fmt.Println("어이쿠 신발끈이..")
		} else {
			fmt.Println("사람도 얼마 없는데 나눠 내자")
		}
	} else {
		fmt.Println("오늘은 내가 쏜다")
	}
}
```

## 6. if 초기문; 조건문

if문 조건을 검사하기 전에 초기문을 넣을 수 있다. 초기문은 검사에 사용할 변수를 초기화할 때 주로 사용한다. 형식은 다음과 같다.

```go
if 초기문; 조건문 {
	문장
}
```

초기문 자리에 하나의 구문이 올 수 있으며 끝에 ; 를 붙여서 구문이 끝남을 표시한다. 그리고 조건문을 넣는다.

```go
if filename, success := UploadFile(); success {
	fmt.Println("Upload success", filename)
} else {
	fmt.Println("Failed to upload")
}
```

먼저 UploadFile() 함수를 실행하고 filename과 success 변수에 반환값을 저장한다. 그리고 그 함수 성공 여부에 따라 다른 메시지를 출력한다. 이렇게 if 초기문은 어떤 함수를 실행하고 그 함수의 결과를 검사할 때 주로 사용한다.

초기문에서 선언한 변수의 범위는 if문 안으로 한정된다는 사실에 주의해야 한다.

```go
package main

import "fmt"

func getAge() (int, bool) {
	return 33, true

}

func main() {

	if age, ok := getAge(); ok && age < 20 {
		fmt.Println("You are young", age)
	} else if ok && age < 30 {
		fmt.Println("Nice age", age)
	} else if ok {
		fmt.Println("You are beautiful", age)
	} else {
		fmt.Println("Error")
	}
}
```

일반적으로 변수는 변수가 선언된 중괄호를 벗어나면 소멸되지만 if 초기문에 선언된 변수는 if문이 종료되기 전까지 유지된다.

## 7. switch 문

```go
package main

import "fmt"

func main() {

	a := 3

	switch a {
	case 1:
		fmt.Println("a==1")
	case 2:
		fmt.Println("a==2")
	case 3:
		fmt.Println("a==3")
	case 4:
		fmt.Println("a==4")
	default:
		fmt.Println("a>4")
	}
}
```

a 값에 따라서 다른 메시지를 출력한다.

```go
package main

import "fmt"

func main() {
	
	day := 3
	
	if day == 1{
		fmt.Println("첫째 날입니다.")
		fmt.Println("오늘은 팀미팅이 있습니다.")
	}else if day == 2{
		fmt.Println("둘째 날입니다.")
		fmt.Println("새로운 팀원 면접이 있습니다.")
	}else if day == 3{
		fmt.Println("셋째 날입니다.")
		fmt.Println("설계안을 확정하는 날입니다.")
	}else if day == 4{
		fmt.Println("넷째 날입니다.")
		fmt.Println("예산을 확정하는 날입니다.")
	}else if day == 5{
		fmt.Println("다섯째 날입니다.")
		fmt.Println("최종 계약하는 날입니다.")
	}else {
		fmt.Println("프로젝트를 진행하세요")
	}
}
```

if-else문을 사용하여 날짜별로 다른 메시지를 출력하는 이 예제에 else-if가 너무 많아서 동작이 한 눈에 보이지 않는 문제가 있다.

switch문을 사용하면 다음과 같이 깔끔하게 변경할 수 있다.

```go
package main

import "fmt"

func main() {
	
	day := 3

	switch day {
	case 1:
		fmt.Println("첫째 날입니다.")
		fmt.Println("오늘은 팀미팅이 있습니다.")
	case 2:
		fmt.Println("들째 날입니다.")
		fmt.Println("오늘은 면접이 있습니다.")
	case 3:
		fmt.Println("섯째 날입니다.")
		fmt.Println("설계안을 확정하는 날입니다.")
	case 4:
		fmt.Println("넷째 날입니다.")
		fmt.Println("예산을 확정하는 날입니다.")
	case 5:
		fmt.Println("다섯째 날입니다.")
		fmt.Println("최종 계약하는 날입니다.")
	default:
		fmt.Println("프로젝트를 진행하세요")
	}
}
```

위의 두 코드는 같인 동작을 하지만 if문을 사용한 예제보다 switch문을 사용한 예제가 가독성이 더 좋다. switch문을 이용하면 복잡한 if문을 깔끔하게 정리할 수 있다.

## 8. 다양한 switch문 형태

### 8.1 한 번에 여러 값 비교

하나의 case는 하나 이상의 값을 비교할 수 있다. 각 값은 쉼표 , 로 구분한다.

```go
package main

import "fmt"

func main() {

	day := "thursday"

	switch day {
	case "monday", "tuesday":
		fmt.Println("월, 화요일은 수업 가는 날입니다.")
	case "wednesday", "thursday", "friday":
		fmt.Println("수, 목, 금요일은 실습 가는 날입니다.")
	}
}
```

하나의 case에 여러 값을 검사할 수 있다.

### 8.2 조건문 비교

```go
package main

import "fmt"

func main() {
	
	temp := 18

	switch true {
	case temp < 10, temp >30:
		fmt.Println("바깥 활동하기 좋은 날씨가 아니다.")
	case temp >= 100 && temp < 20:
		fmt.Println("약간 추울 수 있으니 가벼운 겉옷을 준비하세요.")
	case temp >= 15 && temp < 25:
		fmt.Println("야외 활동하기 좋은 날씨입니다.")
	default:
		fmt.Println("따뜻합니다.")
	}
}
```

switch문은 비굣값과 case의 값이 같아지는 경우를 찾는 구문이기 때문에 비굣값을 true로 할 경우 case의 조건문이 true가 되는 경우가 실행된다.

### 8.3 switch 초기문

if문과 마찬가지로 switch문에서 초기문을 넣을 수 있다. 형식은 다음과 같다.

```go
switch 초기문; 비굣값{
case 값1:
	...
case rkqt2:
	...
default:
}
```

예제를 살펴보며 알아보겠다.

```go
package main

import "fmt"

func getMyAge() int {
	return 22
}

func main() {
	switch age := getMyAge(); age { //getMyAge() 결괏값 비교
	case 10:
		fmt.Println("Teenager")
	case 33:
		fmt.Println("Pair 3")
	default:
		fmt.Println("My age is", age)
	}
}
```

먼저 age := getMyAge() 가 실행되어 age가 초기화된다. 그리고 switch문의 비교값으로 age가 사용된다. age는 switch문에서 선언된 변수이기 때문에 switch문이 종료되기 전까지 접근할 수 있다. 하지만 switch문이 종료되면 age도 사라진다.

초기문으로 age 변수를 초기화하고 비굣값을 true로 하여서 조건문을 통해 age에 해당하는 세대를 표시하는 예제를 살펴보자.

```go
package main

import "fmt"

func getMyAge() int{
	return 22
}

func main() {

	switch age := getMyAge(); true {
	case age < 10:
		fmt.Println("Child")
	case age < 20:
		fmt.Println("Teenager")
	case age < 30:
		fmt.Println("20s")
	default:
		fmt.Println("My age is", age)
	}
}
```

## 9. const 열거값과 switch

const 열거값에 따라 수행되는 로직을 변경할 때 switch문을 주로 사용한다.

```go
package main

import "fmt"

type ColorType int

const (
	Red ColorType = iota
	Blue
	Green
	Yellow
)

func colorToString(color ColorType) string {
	switch color {
	case Red:
		return "Red"
	case Blue:
		return "Blue"
	case Green:
		return "Green"
	case Yellow:
		return "Yellow"
	default:
		return "Undefined"
	}
}
func getMyFavoriteColor() ColorType {
	return Blue
}

func main()  {
	fmt.Println("My favorite color is", colorToString(getMyFavoriteColor()))
}
```

## 10. break와 fallthrough 키워드

```go
package main

import "fmt"

func main() {
	
	a := 3
	switch a {
	case 1:
		fmt.Println("a == 1")
		break
	case 2:
		fmt.Println("a == 2")
		break
	case 3:
		fmt.Println("a == 3")
	case 4:
		fmt.Println("a == 4")
	default:
		fmt.Println("a > 4")
	}
}
```

case 1과 case 2의 경우 break문을 사용해서 switch문을 빠져나갔다. case 3과 case 4는 사용하지 않았다. Go 언어에서는 break를 사용하든 사용하지 않든 상관없이 case 하나를 실행 후 switch문을 빠져나간다.

그런데 만약 하나의 case문 실행 후 다음 case문까지 같이 실행하고 싶을 땐 fallthrough 키워드를 사용한다. case 마지막에 fallthrough 키워드를 사용하면 다음 case 문까지 같이 실행된다.

fallthrough 키워드 동작을 확인해보겠다.

```go
package main

import "fmt"

func main() {
	a := 3

	switch a {
	case 1:
		fmt.Println("a == 1")
		break
	case 2:
		fmt.Println("a == 2")
	case 3:
		fmt.Println("a == 3")
		fallthrough
	case 4:
		fmt.Println("a == 5")
	default:
		fmt.Println("a > 5")
	}
}
```

case 3 구문 마지막에 fallthrough 키워드를 사용했다. 그 결과 case 4까지 실행됐다.
