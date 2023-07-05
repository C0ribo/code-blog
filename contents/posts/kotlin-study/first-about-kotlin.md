---
title: "🤔 1. 코틀린 기본형"
description: "코틀린 변수(var,val), 타입(Int,Float, Double,Char,Booleans, String, Array)"
date: 2023-07-05
update: 2023-07-05
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## 변수

```Kotlin
var/val 변수명 : 변수타입 = 초기화 값
```

### val

val은 `불변 변수`이다. 불변 변수는 한번 초기화하면 다시는 값을 대입할 수 없는 변수다. 즉, `읽기`만 가능한 변수이다. 자바에서는 final 변수에 해당한다.

### var

var은 `가변 변수`이다. 가변 변수는 원할 때 변수의 값을 얼마든지 바꿀 수 있다. 즉, `읽기`와 `쓰기`가 가능한 변수이다. 자바에서는 일반 변수와 같다.

> 주로 var보다는 val을 사용하며 전자를 사용할 경우 변경 가능성이 많아서 예상하지 못하는 경우가 생길 수 있기 때문이다.

## 타입

자바는 int와 같은 원시 타입과 String 같이 클래스를 기반으로 하는 참조 타입 사이에 명확한 구분이 있다. 코틀린은 똑같은 타입이 문맥에 따라 **원시 타입**과 **참조 타입**을 구분해서 사용하지 않는다. 자바에는 원시 타입을 감싸는 박싱 타입이 있지만, 코틀린은 필요할 때 암시적으로 박싱을 수행한다. 코틀린 기본 타입은 모두 객체이다.

### 정수 타입

```Kotlin
val one : Byte = 1 //Int
val threeBillion : Long = 3000000000 //Long
val oneLong : Long = 1L //Long
val oneByte : Byte = 1
```

`Int` 범위를 초과하지 않는 경우 유형은 `Int`이다. 범위를 초과하면 유형은 `Long`이다. `Long`값을 명시적으로 지정하려면 값에 접미사 `L`을 추가한다.

```Kotlin
val num1 : Int = 12345 //12345
val num2 : Int = 12_345 //12345
```

큰 수를 나타낼 때 `_`를 넣어서 가독성을 높일 수 있다.

```Kotlin
val zero = 0
val zeroOne = 01 //Error
```

수 리터럴의 경우 0을 표현하는 경우가 아니라면 맨 앞에 0이 올 수 없음에 유의해야 한다.

```Kotlin
val neg = -10
val negHex = -0xFF
```

-10과 같은 음수는 기술적으로는 리터럴이 아니다. 음수는 단항 음수 연산자 (`-`)를 리터럴에 적용 한 식이다.

```Kotlin
Short.MIN_VALUE // -32768
Short.MAX_VALUE // 32767
Int.MAX_VALUE + 1 // -2147483648(정수 오버플로)
```

각 정수 타입에는 최솟값(`MIN_VALUE`)과 최댓값(`MAX_VALUE`)을 포함하는 상수 정의가 들어있다. 이런 상수를 사용하려면 앞에 타입 이름을 붙여야 한다.

### 부동소수점 수

부동 소수점 유형 `Float` 및 `Double`을 제공한다.

```Kotlin
val pi = 3.14 //Double
val one = 1.0 //Double
val one : Double = 1 //Error
```

정수 부분이 비어있는 경우 정수 부분을 0으로 간주한다. 하지만 소수점을 남기면서 소수 부분을 생략할 수는 없다.

```Kotlin
val quarter = .25 // 0.25
val one = 1. // Error
val two = 2 // 오류는 아니지만, 정수 리터럴이다.
```

디폴트로 부동소수점 리터럴은 `Double` 타입이다. `f`나 `F`를 리터럴 뒤에 붙이면 `Float` 타입이 된다.

```Kotlin
val pi = 3.14f
val one = 1f
```

> 자바에서는 `D`나 `d`를 리터럴 뒤에 붙여서 강제로 `double` 타입으로 만들 수 있다.(예: 1.25d) 하지만 코틀린에서는 이런 접미사를 허용하지 않는다.

`Float` 리터럴이 `Double` 타입으로 자동 변환되지 않는다는 점 유의해야 한다.

```Kotlin
val pi : Double = 3.14f // Error
```

### 문자 타입

문자는 `Char` 유형으로 표시하며 이 타입의 리터럴은 작은따옴표(`'`) 사이에 문자를 넣으면 된다.

```Kotlin
val z : Char = 'z'
val alpha : Char = 'a'
```

새줄 문자와 같은 특수 문자를 위해 코틀린은 이스케이프를 제공한다. 특수 문자는 백슬래시 `\`로 시작합니다.

- `\t` : 탭
- `\b` : 백스페이스
- `\n` : 새줄
- `\r` : 캐리지 리턴
- `\'` : 작은따옴표
- `\"` : 큰따옴표
- `\\` : 역슬래시
- `\$` : 달러 표시
- `\u` : 유니코드 시퀀스 구문

```Kotlin
val quote = '\''
val newLine = '\n'
val pi = '\u03C0' // 파이
```

코틀린은 Char 자체를 수 타입으로 취급하지 않는다. 하지만 유니코드 문자 집합 내에서의 몇 가지 산술 연산을 허용한다.

### 불 타입

코틀린은 참이나 거짓 중 하나로 판명되는 불 타입을 제공한다. 비교 연산자나 조건식을 사용해 불이 아닌 값에서 불 값을 만들어야 한다.

- `||` : 논리합(OR)
- `&&` : 논리곱(AND)
- `!` : 논리 부정(NOT)
- `or`, `and`, `xor` : 논리합, 논리곱, 논리배타합

`||`와 `&&`은 지연 계산 방식이고 `or`, `and`, `xor`은 즉시 계산 방식이다.

> 자바와 달리 코틀린은 `&`와 `|` 연산자를 제공하지 않는다. `and`와 `or`가 각각 `&`와 `|`를 대신한다.

### 문자열

코틀린의 문자열은 `String` 유형으로 표시한다. 일반적으로 문자열은 큰따옴표(`"`) 안에 넣어서 나타낸다. 자바와 마찬가지로 코틀린 문자열도 불변이다. 문자열을 초기화하면 값을 변경하거나 새 값을 할당할 수 없고 문자열을 읽기만 할 수 있다. 문자를 바꾸고 싶으면 기존 문자열을 바탕으로 새로운 문자열을 만들어야 한다.

```Kotlin
val n : String = "Hello 123"

val n = "Hello"
println(n.uppercase()) // HELLO
println(n[1]) // e
```

#### 문자열 템플릿

템플릿 표현식은 달러 기호(`$`)로 시작하며 이름 중 하나로 구성된다. 기본적으로 `${}`의 중괄호 사이에 어떤 문자열을 넣을 수 있지만 간단한 변수 참조인 경우 중괄호를 생략이 가능하다.

#### 로우 문자열

이스케이프 시퀀스를 사용하지 않고도 문자열을 작성할 수 있다. 이러한 리터럴은 큰따옴표 세 개(`"""`)로 둘러 싸여 있고, 새줄을 포함한 임의의 문자를 포함할 수 있다. 특수 문자를 추가하고 싶은 경우에는 `${}` 안에 넣어주면 된다.

```Kotlin
val message = """
  Hello, $name!
  Today is ${Date()}
  """.trimIndent()
```

- `trimIndent()` : 여러 줄에 공통된 최소 들여쓰기를 제거해주는 표준 코틀린 함수
- `trimMargin()` : 선행 공백을 제거해주는 코틀린 함수

#### 문자열 연산

String 인스턴스는 문자열에 든 문자 수를 표현하는 `length`와 문자열의 마지막 문자 인덱스를 표현하는 `lastIndex` 프로퍼티를 제공한다.

```Kotlin
"Hello!".length // 6
"Hello!".lastIndex // 5 [0][1][2][3][4][5]
```

- `isEmpty`/`isNotEmpty` : 문자열이 비어있는지 검사한다.
- `substring` : 문자열을 추출한다.
- `endsWith` : 접두사나 접미사인지 검사한다.
- `indexOf` : 인자로 받은 문자나 문자열이 수신 객체인 문자열에 나타나는 첫번째 인덱스를 반환한다.

### 배열

배열 구조를 구현하는 가장 일반적인 코틀린 타입은 `Array<T>`다. `arrayOf()`함수를 이용하여 배열을 선언할 수 있으며 `[]` 또는 `get`/`set` 함수를 이용해 접근할 수 있다. `arrayOf(1, 2, 3)`이 배열 `[1, 2, 3]`을 생성한다.

```Kotlin
val a = emptyArray<String>() // Array<String> (원소 0개)
val b = arrayOf("hello", "world") // Array<String> (원소 2개)
val c = arrayOf(1, 4, 9) // Array<Int> (원소 3개)
```

코틀린의 배열은 불변이다. 즉, 코틀린에서는 `Array<String>`을 `Array<Any>`에 할당할 수 없으므로 런타임 오류 가능성을 방지할 수 있다. 모든 다른 배열 타입과 서로 하위 타입 관계가 성립하지 않는다고 간주한다. `String`은 `Any`의 하위 타입이지만 `Array<String>`은 `Array<Any>`의 하위 타입이 아니다.

```kotlin
val strings = arrayOf("one", "two", "three")
val objects : Array<Any> = strings // 예외
```

#### get/set

- `get()` : 특정 인덱스의 엘리먼트를 리턴하며 인덱스 연산자 []로 호출할 수 있다.

```kotlin
operator fun get(index: Int) : T
```

- `set()` : 특정 인덱스의 특정 값을 set하며 인덱스 연산자 []로 호출할 수 있다.

```kotlin
operator fun set(index: Int, value: T)
```

#### 기본 유형 배열

배열은 `Array<Int>`, `Array<Long>`처럼 값을 사용하여 순회하면 박싱 현상으로 인해 메모리 누수 및 오버헤드가 추가적으로 발생할 가능성이 있다. 코틀린은 `ByteArray`, `ShortArray`, `IntArray`, `LongArray`, `FloatArray`, `DoubleArray`, `CharArray`, `BooleanArray`라는 특화된 배열 타입을 제공한다.

```Kotlin
val operations = charArrayOf('+', '-', '*', '/', '%')
val squares = IntArray(10) { (it + 1) * (it + 1)}
```

> 코틀린에는 new 연산자가 없다. 배열 원소를 명시적으로 초기화해야 한다는 점에 유의해야 한다.

이러한 클래스는 `Array`클래스와 상속 관계가 없지만 동일한 메서드 및 프로퍼티 집합을 갖는다.

#### 배열 사용하기

배열 타입은 문자열 타입과 비슷하다. `size`(문자열의 `length`)와 `lastIndex` 프로퍼티가 있다.

```kotlin
val squares = arrayOf(1, 4, 9, 16)
squares.size // 4
squares.lastIndex // 3
squares[3] // 16
suqares[1] // 4
```

(1) 문자열과 달리 배열에서는 원소를 변경할 수 있다.

```kotlin
squares[2] = 100 // 1, 4, 100, 16
squares[3] += 9 // 1, 4, 100, 25
squares[0]-- // 0, 4, 100, 25
```

(2) 자바와 마찬가지로 배열 타입의 변수 자체에는 실제 데이터에 대한 참조를 저장. 배열 변수에 다른 배열 대입하면 같은 데이터 집합을 공유한다.

```kotlin
val num = squares
num[0] = 1000 // 바뀐 데이터가 squares와 num에 공유
println(squares[0]) // prirnts 1000
```

(3) `copyOf()`는 필요시 다른 크기의 배열을 만들어낸다.

```kotlin
val num = squares.copyOf()
num[0] = 1000  // squares에 영향이 없다
squares.copyOf(2) // 1, 4
squares.copyOf(5) // 1, 4, 9, 16, 0 (부족한 부분에 0이 채움)
```

(4) 배열 타입 변수에 타입이 다른 배열을 대입할 수 없다.

```kotlin
var a = arrayOf(1, 4, 9, 16)
a = arrayOf("one", "two") // Error: Array<String> -> Array<Int>
```

## 참고

- 코틀린 완벽 가이드
- 코틀린 공식 레퍼런스