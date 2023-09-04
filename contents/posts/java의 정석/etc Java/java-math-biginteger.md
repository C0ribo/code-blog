---
title: "[Java] java.math.BigInteger"
description: "BigInteger"
date: 2023-08-14
update: 2023-08-14
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## java.math.BigIntger란?

- 과학적 계산과 같은 큰 값을 다뤄야할 때 쓰이는 것이 `BigInteger`이다.

- BigInteger는 내부적으로 int배열을 사용해서 값을 다루기 때문에 long타입보다 훨씬 큰 값을 다룰 수 있지만, 성능은 떨어지는 편이다. 

- Number 클래스를 상속하고 있다.

### BigInteger 생성

문자열로 숫자를 표현하는 것이 일반적이다.

#### BigInteger(String val)

```java
BigInteger val = new BigInteger("123456789");
```

long형 을 벗어나는 값을 표현해야 하기 때문에 문자열로 생성된다.

#### BigInteger(String val, int radix)

```java
BigInteger val = new BigInteger("FFFF", 16);
```

n진수(radix)의 문자열로 생성한다. 16진법으로 표현한 FFFF를 출력하면 10진법 65535와 같다.

#### BigInteger.valueOf()

```java
BigInteger val = BigInteger.valueOf(123456789);
```

값을 숫자로 넘겨주기 위해 `valueOf` 사용하며, 이때 앞에 `new`가 붙지 않는다.

### 다른 타입으로 변환

| 반환값 | 메서드명 | 설명 |
| --- | ---- | ----|
|String| toString() | 문자열로 변환 | 
|String |toString(int radix) | 지정된 진법(radix)의 문자열로 변환 |
| byte[] | toByteArray() | byte배열로 변환 |
|int<br>long<br>float<br>double | intValue()<br>longValue()<br>floatValue()<br>doubleValue() | BigInteger도 Number로부터 상속받은 기본형으로 변환하는 메서드를 가지고 있다.|
|byte<br>int<br>long | byteValueExact()<br>intValueExact()<br>longValueExact()|정수형으로 변환하는 메서드 중 이름 끝에 Exact가 붙은 것은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생 |

### BigInteger 연산

|메서드 | 설명 | 
| --- | --- |
|BigInteger add(BigInteger val) | 덧셈(this + val) |
|BigInteger subtract(BigInteger val) | 뺄셈(this - val) |
|BigInteger multiply(BigInteger val) | 곱셈(this * val) |
|BigInteger divide(BigInteger val) | 나눗셈(this / val) |
|BigInteger remainder(BigInteger val) | 나머지(this % val) |

> remaunder와 mod는 나머지를 구하는 메서드이지만, mod는 나누는 값이 음수면 ArithmeticException을 발생

### 비트 연산 메서드

and, or, xor, not과 같이 비트연산자를 구현한 메서드는 물론 다음과 같은 메서드들도 제공

| 리턴값 | 메서드명 | 설명 | 
| :---: | :---: | --- |
| int | bitCount() | 2진수로 표현했을 때, 1의 개수 (음수는 0의 개수)를 반환 |
| int | bigLength() | 2진수로 표현했을 때, 값을 표현하는데 필요한 bit수 |
| boolean | testBit(int n) | 우측에서 n+1번째 비트가 1이면 true, 0이면 false |
| BigInteger | setBit(int n)  | 우측에서 n+1번째 비트를 1로 변경 | 
| BigInteger | clearBit(int n) | 우측에서 n+1번째 비트를 0으로 변경 | 
| BigInteger | flipBit(int n) | 우측에서 n+1번째 비트를 전환 (1->0, 0->1)| 
