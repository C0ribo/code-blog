---
title: "🥸 6. 코틀린 루프"
description: "코틀린의 루프, while, do-while, for loop, Iterable, break, continue"
date: 2023-07-08
update: 2023-07-08
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## while과 do-while 루프

`while`과 `do-while` 루프는 자바와 같은 구조이다. 이 두 루프는 조건이 충족되는 동안 지속적으로 실행된다. 

### while

`for` 반복문이 범위를 정해서 하도록 강제한다면, `while` 반복문의 경우 **조건식이 참일 경우에만 반복한다.** 하지만 실행하기 전에 루프 실행 전 조건을 먼저 검사하고 처음부터 조건이 거짓이면 루프 조건이 한번도 실행되지 않는다.
```kotlin
while(조건식){
    // 반복 수행될 작업
}
```
예를 들어 1부터 10까지 출력한다고 했을때 마지막에 11까지 출력이 되는데 i의 깂이 10이 되더라도 조건식이 참이므로 반복 수행될 작업이 이루어지고 11에서 거짓이 되어 끝이 나는 것이다.
```kotlin
var i = 1
while(i <= 10){
    println(i)
    ++i
}
println(i)
```

`while`은 무한 반복을 하는 경우에 사용한다. 예를 들면 디지털시계나 채팅 프로그램의 채팅 메시지 수신처럼 어떤 대상을 반복적으로 검사할 때 사용한다. 
```kotlin
while(true){
    // 반복 수행될 작업
}
```

### do-while
앞의 `while`과 다르게 `do-while`의 경우에는 조건이 달라도 최소 한번은 실행하야 할 때 사용한다. 
```kotlin
do {
    // 반복 수행될 작업
} while(조건식)
```
사용자로부터 어떤 입력을 받았을 때 입력으로 특정한 문자열이 들어오면 종료하는 경우에 `do-while` 구문을 이용한다.


## for루프와 이터러블
`for`루프는 자바 `for-each`와 비슷하다. 코틀린의 모든 루프는 식이 아니고 문이기 때문에 어떤 값으로 평가되지 않으며 부수 효과를 발생시킬 수만 있다.

### for loop
`for`루프를 사용하면 컬렉션과 비슷하게 여러 값이 들어있을 수 있는 값에 대한 루프를 수행할 수 있다. 
코틀린의 경우 자바 `for`루프 `for(int i = 0; i< length; i++)`에 해당하는 루프가 없어서 `while` 루프로 표현하거나 범위 `..`와 진행을 사용하는 `for` 루프를 사용한다.

`..`의 경우 우항을 포함하는데 포함하고 싶지 않으면 `until`을 사용한다.
```kotlin
for(x in 0 until size) == for(x in 0..size - 1)
```

```kotlin
for(bar in (min)..(max)){
    // commands
}
```
`bar`는 `for` 구문이 반복되며 증감하는 값을 가지는 변수이다. 루프 변수에서는 val이나 var를 붙이지 않고 자동으로 불변 값이 된다. `for`문에서도 반복하는 명령문이 하나라면 다음과 같이 중괄호를 제거할 수 있다.
```kotlin
for(i in 1..10) println(i)
```
자바에서는 문자열의 각 문자에 대해 루프를 직접 수행할 수 없다. 범위 또는 배열에 대한 for루프는 

### Iterable
`Iterable`은 컬렉션(List, Set, Map)에 저장되어 있는 것을 일관성있게 꺼내 오는 방법이다. `Iterator()` 함수를 호출하여 `Iterable` 인터페이스의 상속자에 대해 `Iterator`를 얻을 수 있다.

**자세한 것은 컬렉션 단원에서 나온다**

## 제어 흐름(control flow)
일반적인 루프의 흐름을 바꾸고 싶을때 제어 흐름을 사용한다.
`break`랑 `continue`가 그 예이다. 자바와 동일한 문장과 같은 기능을 제공한다.
### break
코틀린에서 `break`는 즉시 루프를 종료시키고 실행 흐름이 루프 바로 다음 문으로 이동하게 만든다.
예를 들어 1에서 5까지 더했을 때 코드이다.
```kotlin
var sum = 0
for(i in 1..10){
    if(i > 5) break
    sum += i
}
println(sum)
```
위의 예제는 1부터 10까지 중에 1-5까지만 더했을 때를 출력하는 코드이다. 범위를 넣지 않았을때는 1-10까지 그대로 더하겠지만 조건을 넣게 된다면 i가 6이 되었을 때 `break` 걸리고 종료가 된다.

### continue
코틀린에서 `continue`는 현재 루프 이터레이션을 마치고 조건 검사로 바로 진행하게 만든다. 즉 말해서 `break`와는 달리 반복 구간을 완전히 끝내지 않고 해당 회차를 넘어가는 대신 다음 회차는 그대로 진행한다는 말이다.
```kotlin
for(i in 1..10){
    if(i % 2 == 0) continue
    println(i)
}
```
위의 예제는 1부터 10 중에 홀수만 출력하도록 나타내는 코드이다. 

## break and continue labels

코틀린의 모든 표현식은 `label`로 표시할 수 있다. `label`은 식별자 뒤에 `@`기호가 붙은 형식이다.(ex `abc@`또는 `fooBar@`) 이 문법은 `break`, `continue`, `return` 키워드와 함께 사용한다.
```kotlin
loop@ for(i in 1 .. 100){
    // ...
}
```
`label`의 선언은 반복문 `for`앞에서 사용할 수 있다. 
`label`을 사용하여 `break` 또는 `continue`을 지정할 수 있다.
```kotlin
loop@ for(i in 1..100){
    for(j in 1..100){
        if(...) break@loop
    }
}
```
`label`이 지정된 `break`은 해당 `label`이 표시된 `loop` 바로 뒤에 있는 실행 지점으로 이동한다. `continue`은 해당 `loop`의 다음으로 반복 진행한다. 

코틀린에서 어느 문장 앞에든 레이블을 붙일 수 있지만 `break`와 `continue`에는 구체적으로 루프 앞에 붙은 레이블만 사용할 수 있다. 레이블 이름은 배열이나 함수와 마찬가지로 임의의 식별자가 될 수 있다.

> 코틀린과 자바는 레이블 정의와 사용 문법이 다르다.   
`loop@ while(true) break@loop`   // 코틀린  
`loop: while(true) break loop`   // 자바

레이블을 사용하면 `break`와 `continue`를 `when`식안에서 쓰면서 제어를 옮길 대상 루프를 지정할 수 있다. 

```kotlin
import kotlin.random.*
fum main(){
    val num = Random.nextInt(1, 101)

    loop@ while(true){
        val guess = readLine()!!.toInt()
        val message = when {
            guess < num -> "Too small"
            guess > num -> "Too big"
            else -> break@loop //정상
        }
        println(message)
    }
    println("Right: it's $num")
}
```

## return to labels

코틀린에서 함수는 함수 리터럴, 지역함수, 객체 표현식을 사용하면서 중첩될 수 있다. `return`은 바깥의 함수로 반환되도록 한다. 주로 람다 표현식으로 반환한다.

아래의 코드는 `return`은 `forEach()`내에서 수행되지만 `foo()` 함수를 종료시킨다.
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return
        print(it)
    }
    println("this point is unreachable")
}
>> 12
```

이러한 `non-local` 반환은 오직 인라인 함수를 통과하는 람다 표현식에서만 유효하다는 것에 유의해야 한다. 람다 표현식으로 반환되기 위해서 라벨링하고 `return@lit`을 사용한다.
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit
        print(it)
    }
    print(" done with explicit label")
}
>> 1245 done with explictit label
```

`implicit labels`를 사용하는 것이 람다가 통과하는 함수명과 라벨의 이름이 일치하도록 하면 되기 때문에 종종 더 편리하게 사용할 수 있다.
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach
        print(it)
    }
    print(" done with implicit label")
}
```
또는 람다 표현식을 익명 함수로 대체할 수 있다. 익명 함수에서의 `return`은 익명 함수 그 자체로부터 반환될 것이다.
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return
        print(value)
    })
    print(" done with anonymous function")
}
>> output: 1245 done with anonymous function
```

이전에 예시로 들었던 것은 모두 `local return`을 사용하는 것을 볼 수 있다. 반면 `break`와 같은 기능을 제공하지 않는데 아래와 같이 다른 람다식으로 감싸는 방식을 통해 우회하여 구현할 수 있다.
```kotlin
fun foo() {
    run loop@{
        listOf(1, 2, 3, 4, 5).forEach {
            if (it == 3) return@loop
            print(it)
        }
    }
    print(" done with nested loop")
}
>> output: 12 done with nested loop
```

값을 반환할 때 파서는 정규화된 반환에 우선 순위를 부여한다.
```kotlin
return@a 1
```

`@a 1`이라고 라벨링된 곳으로 리턴하는 것이 아니라 `1`을 `@a label`에 반환하는 의미이다.

## 참조

- 코틀린 완벽 가이드
- [코틀린 공식 레퍼런스(Returns and jumps)](https://kotlinlang.org/docs/returns.html)