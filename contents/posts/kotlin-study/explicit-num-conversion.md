---
title: "😳 2. 코틀린 수 변환과 연산자"
description: "코틀린의 자료형들의 형 변환, 연산자"
date: 2023-07-05
update: 2023-07-05
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## 숫자 변환

각 수 타입마다 값을 다른 수 타입으로 변환하는 연산이 정의되어 있다.
- `toByte()` : Byte
- `toShort()` : Short
- `toInt()` : Int
- `toLong()` : Long
- `toFloat()` : Float
- `toDouble()` : Double

> 자바와 달리 코틀린에서는 범위가 큰 타입이 사용되야 하는 문맥에 범위가 작은 타입을 쓸 수 없다. 예를 들어 `int`값을 `Long` 변수에 대입할 수 없다.

```kotlin
fun main(){
    val str : String = "123"
    val a : Int = str.toInt()
    val b : Long = str.toLong()
}
```

## 연산자

### 산술 연산자

숫자에 대한 표준 산술 연산 집합을 지원합니다.: `+`, `-`, `*`, `/`, `%`
```kotlin
fun main(){
    println(1 + 2) // 3
    println(1 - 3) // -2
    println(3 * 4) // 12
    println(7 / 4) // 1
    println(7 % 4) // 3
}
```

### 대입 연산자

특정 변수에 값을 넣을 넣을때 사용하는 연산자
```kotlin
fun main(){
    var f : Int
    f = 5
    println("$f\n")
}
```


### 복합 대입 연산자, 증감 연산자

#### 복합 대입 연산자

산술 연산자와 대입 연산자를 합친 연산자이다. `+=`, `-=`, `*=`, `/=`, `%=`

#### 증감 연산자

변수의 값을 증가하거나 감소시키는 연산자이다. 
- `A++(A--)` : A의 값을 먼저 반환하고 증가
- `++A(--A)` : A의 값을 증기시키고 값을 출력
> A++ 형태를 후위 증감 연산자, ++A를 전위 증감 연산자라고 한다.


### 비교 연산자

- `==` : 같다
- `!=` : 같지 않다
- `<` : 작다
- `<=` : 작거나 같다
- `>` : 크다
- `>=` : 크거나 같다

```kotlin
val a = 1 // Int
val b = 2L // Long
println(a == b) // error
println(a.toLong() == b) //ok
```
두 인자가 모두 같은 타입일 때만 `==`와 `!=` 허용한다.

### 비트 연산자 

`Int`와 `Long`은 비트 수준의 연산을 제공한다.

- `shl` : 왼쪽 시프트 (자바: <<)
- `shr` : 오른쪽 시프트 (자바: >>)
- `ushr` : 부호 없는 오른쪽 시프트 (자바: >>>)
- `and` : 비트 곱(AND) (자바: &)
- `or` : 비트 합(OR) (자바: |)
- `xor` : 비트 배타합(XOR) (자바: ^)
- `inv` : 비트 반전(NOT) (자바: ~)

> 자바에서는 비트 연산자 &, |, ^, ~, <<, >>, >>> 를 사용하지만 코틀린은 이런 연산자를 지원하지 않는다.

### 논리 연산자

비트 연산자가 비트의 논리에 대해 다뤘으면 논리 연산자는 논리식에 대해 참, 거짓을 다룬다. 보통 `Boolean`과 같이 쓴다.

- `&&` : `AND`, 모두가 true이면 결과값 true , 둘 중 하나라도 false면 false
- `||` : `OR`, 둘 중 하나라도 true이면 true, 둘다 false이면 false
- `!` : `NOT`, true이면 false, false이면 true

```kotlin
println("true && true : ${true && true}\n") // true
println("true && false : ${true && false}\n") // false

println("false || false : ${false || false}\n") // false
println("true || false : ${true || false}\n") // true

println("true && !false : ${true && !false}\n") // true
```


## 참고

- 코틀린 완벽 가이드
- 코틀린 공식 레퍼런드(Operator)
- [참고 블로그](https://blog.skylightqp.kr/269)