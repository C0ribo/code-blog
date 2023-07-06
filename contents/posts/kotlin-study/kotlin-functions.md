---
title: "😎 3. 코틀린 함수"
description: "코틀린 기본 함수 "
date: 2023-07-06
update: 2023-07-06
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## 함수

### 함수의 정의

함수를 나타낼 때 `fun` 키워드를 사용하여 선언한다.
```kotlin
fun 함수명(인자 : 타입, 인자 : 타입) : 반환타입 {
    return 반환값
}
```

#### 파라미터

코틀린 함수 파라미터는 무조건 불변이다. 함수의 파라미터는 파스칼 표기법을 사용한다. 보통 파라미터는 쉼표(`,`)로 구분되며 파라미터를 선언할 때 후행 쉼표로 나타낼 수 있다.
```kotlin
fun powerOf(number: Int, exponent: Int) : Int {...} // 쉼표

fun powerOf( // 후행 쉼표
  number: Int,
  exponent: Int,
) {...}
```

### 단일 표현식 함수(Single-expression functions)

`return` 키워드와 블록을 만드는 중괄호(`{}`)를 생략하고 반환 타입과 함수 본문 식 사이에 `=`가 들어있다.
```kotlin
fun sum(a : Int, b : Int) : Int = a + b
```

컴파일러가 반환 유형을 유추할 수 있는 경우 반환 타입을 생략할 수 있다.
```kotlin
fun sum(a : Int, b : Int) = a + b
```

블록이 본문인 함수를 정의할 때 `{}` 앞에 `=`를 넣으면 이 블록이 람다로 해석되기 때문에 원하는 결과를 얻을 수 없다.
```kotlin
fun sum(a : Int, b : Int) = { a + b }
```

### 단위 반환 함수(Unit-returning functions)

`Unit`은 자바 `void`에 해당하는 코틀린 타입으로, 함수가 유용한 값을 반환하지 않는 경우의 반환 유형이다. `Unit`은 단 하나의 값만 있는 유형으로 이 값은 명시적으로 반환할 필요가 없다.
```kotlin
fun printHello(name : String?) : Unit {
  if (name != null)
    println("Hello $name")
  else
    println("Hi there!")
}
```

`Unit`도 선택 사항이다.
```kotlin
fun printHello(name : String?) { ... }
```

### 위치 기반 인자와 이름 붙은 인자
#### 위치 기반 인자(positional  argument)

함수 호출 인자는 순서대로 파라미터에 전달한다. 
```kotlin
fun rectangleArea(width : Double, height : Double) : Double {
  return width*height
}
fun main(){
  val w = readLine()!!.toDouble()
  val h = readLine()!!.toDouble()
  println("Rectangle area: ${rectangleArea(w, h)}")
}
```

`width : Double`이 첫번째 파라미터, `height : Double`가 두번째 파라미터이다.

#### 이름 붙은 인자(named argument)

이름 붙은 인자는 위치가 아니라 파라미터의 이름을 명시함으로써 인자를 전달하는 방식이다. 
```kotlin
rectangleArea(width = w, height = h)
```
위의 예제처럼 나타낼 수 있다.
```kotlin
rectangleArea(height = h, width = w)
```
첫번째 예제와 두번째 예제는 `rectangleArea(w, h)`로 동일하다. 한 호출 안에서 위치 기반 인자와 이름 붙은 인자를 함께 사용할 수도 있다.

### vararg

`vararg`은 가변 인자로 매개변수의 개수를 동적으로 지정해줄 수 있게 하는 변수이다. 또한, 사용하면 여러값을 전달할 수 있으며 전달받은 데이터 타입은 `배열`이다. `vararg`을 사용시에 파라미터에서 가장 뒤에 선언해서 사용해야 한다.
```kotlin
fun printSorted(vararg items : Int){
  items.sort() // sort() 배열 정렬 함수
  println(items.contentToString()) 
  // contentToString(): 배열에 저장된 데이터를 출력할때 사용한다.
}
fun main(){
  printSorted(6, 2, 10, 1) // [1, 2, 6, 10]
}
```

#### 가변 인자

배열 이름 앞에 (`*`)을 붙여주면 배열이 가변 인자로 취급한다. 

```kotlin
fun main(){
  val a = intArrayOf(6, 2, 10, 1)
  printSorted(*a) // [1, 2, 6, 10]
  println(a.contentToString()) // [6, 2, 10, 1]
}
```

참고로 스프레드는 배열을 복사한다. 파라미터 배열의 내용을 바꿔도 원본 원소에는 영향을 미치지 않는다. 둘 이상을 `vararg` 파라미터로 선언하는 것은 금지한다. 하지만 콤마로 분리한 여러 인자와 스프레드를 섞어서 사용하는 것은 괜찮다. 
```kotlin
printSorted(6, 1, *intArrayOf(3, 8), 2) // [6, 1, 3, 8, 2] -> [1, 2, 3, 6, 8]
```
디폴트 값이 있는 파라미터와 `vararg`를 섞어 쓰는 것은 어렵다. 디폴트를 `vararg`보다 앞에 두면 `vararg` 파라미터에 첫 번째로 전달되야 하는 값이 디폴트가 지정된 파라미터에 전달될 값으로 간주한다. 
```kotlin
fun printSorted(prefix: String = "", vararg items:Int){ }
fun main() {
  printSorted(6, 2, 10, 1) //Error
  printSorted(items = *intArrayOf(6, 2, 10, 1)) // ok
}
```

반대로 `vararg` 파라미터 뒤에 디폴트 파라미터가 있는 경우에는 디폴트 파라미터를 이름 붙은 인자로 호출해야 사용할 수 있다.
```kotlin
fun printSorted(vararg items: Int, prefix: String = ""){ }
fun main(){
  printSorted(6, 2, 10, 1, "!") // error
  printSorted(6, 2, 10, 1, prefix = "!") // ok
}
```

#### 예제(1)
```kotlin
fun st(vararg a : String){ ... }
fun main(){ 
  val test = arrayOf("A", "B", "C") // ok
}
```

#### 예제(2)
```kotlin
fun num(vararg a : Int){ ... }
fun main(){ 
  val num = arrayOf(1, 2, 3) // error
}
```

예제(1)은 `String형`으로 나타냈고 예제(2)는 `Int형`으로 나타내었다. 위의 예제의 결과와 같이 `Int형`에 오류가 난 이유는

> `arrayOf()`함수를 사용하면 `Array<Int>`가 아니라 `Array<Integer>`로 변환되기 때문에 컴파일 되지 않고 오류라 판단한다.

`Int`와 `Integer`는 다르다. 그래서 `intArrayOf()`함수를 사용하고 `arrayOf()`함수 사용 후 `toIntArray()`함수로 `Array<Integer> -> Array<Int>`로 형변환 해야한다.

## 참고
- 코틀린 완벽 가이드
- 코틀린 공식 레퍼런스