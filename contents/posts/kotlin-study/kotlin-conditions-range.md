---
title: "🥲 5. 코틀린 조건문"
description: "코틀린의 조건문(if, when) 그리고 범위"
date: 2023-07-07
update: 2023-07-07
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## if

코틀린에서 `if`식은 표현식이다. 그래서 `if`는 반환값을 가진다. `if`식은 조건이 참일 때 첫 번째 문장을 실행하고, 조건이 거짓일 때 `else` 다음의 문장을 실행한다.

```kotlin
if(조건식){ }
    else if(조건식){ }
        else{ }
```

위 코드는 `if/else`의 기본적인 형태이다. 조건이 거짓일때 아무것도 없다면 `else`부분은 생략할 수 있으며 조건은 항상 `Boolean` 타입의 식이어야 한다.

```kotlin
fun max(a : Int, b : Int) = if(a > b) a else b
```

코틀린 `if`식을 사용하여 더 단순화하게 나타낼 수 있다.

코틀린은 자바와 달리 **삼항 연산자(조건 ? 참일 때 식 : 거짓일 때 식)가 없다.** 하지만 `if`를 식으로 쓸 수 있을 만큼 단점을 커버해준다. 단, 처리구문이 한 줄이어야 한다.

```kotlin
val max = if(a > b) a else b
```

## when

자바에서 `switch`가 있다면 코틀린에는 그와 동일한 역할을 하는 `when`이 있다.
`when`문은 `when`키워드 다음에 블록이 오고 블록 안에는 `조건 -> 문 `형태로 된 여러개의 가지와 `else -> 문` 형태로 된 한 가지가 있다.

```kotlin
when (변수){
    값 -> { TODO }
    값, 값 -> { TODO }
    in 값..값 -> { TODO }
    !in 값..값 -> { TODO }
    else -> { TODO }
}
```

코틀린은 `switch`에서의 `break`는 필요없는 대신에 `{}`중괄호로 범위를 지정해줄 수 있다. `when`에서의 `else`는 `switch`에서의 `default`와 동일한 역할을 한다.

```kotlin
when(x){
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```

`when`이 표현식으로 사용되는 경우, 컴파일러가 `enum class` 및 `sealed class` 서브타입과 같이 가능한 모든 경우가 분기 조건에 포함된다는 것을 증명할 수 없는 한 `else` 분기는 필수이다.

많은 경우가 같은 방식으로 처리하고 싶다면 쉼표(``,``)로 한 줄에 조건을 결합한다.

```kotlin
when(x){
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

분기조건에 상수 뿐만 아니라 임의의 표현식을 사용할 수 있다.

```kotlin
when (x) {
    s.toInt() -> print("s encodes x")
    else -> print("s does not encode x")
}
```

또한 분기조건으로 범위 또는 컬렉션에 `in` 또는 `!in`되는지 확인할 수 있다.

```kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```

`when`은 `if-else if` 체인을 대체할 수 있다. 인수가 주어지지 않으면 분기조건은 단순히 부울 표현식이며 조건이 참일 때 분기가 실행된다.

```kotlin
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is odd")
}
```

## 범위, 진행, 연산

### ``..``

범위를 만드는 가장 간단한 방법이다. `<=`와 `>=`를 쓸 수 있는 타입이라면 `..`를 사용해 범위를 만들 수 있다.

### `in`

어떤 값이 범위 안에 들어있는지 알 수 있다.

```kotlin
val num = readLine()!!.toInt()
println(num in 10..99) // num >= 10 && num <= 99
```

### `!in`

`in`의 반대 연산으로 `!in`을 사용하면 `!(a in b)`를 간단하게 쓸 수 있다.

```kotlin
println(num !in 10..99) //(num in 10.99)
```

### `until`

`until`은 크기 **-1**만큼을 뜻한다.

```kotlin
val twoDigits = 10 until 100 // 10..99와 같고 100은 포함되지 않음
```

### `step`

증가 값을 지정해서 대입한다.

```kotlin
1..10 step 3 // 1, 4, 7, 10 : 1~10사이 3을 더해 증가시킨다.
```

### `downTo`

값을 감소시키면서 반복

```kotlin
15 downTo 9 step 2 // 15, 13, 11, 9 => 15~9까지 내려가면서 2씩 감소시켜준다.
```

## 참고

- 코틀린 공식 레퍼런스
- 코틀린 완벽 가이드
- [범위, 진행, 연산](https://alpharodun.tistory.com/42)