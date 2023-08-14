---
title: "[Java] java.math.BigDecimal"
description: "BigDecimal"
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

## java.math.BigDecimal이란? 

- BigDecimal은 실수형과 달리 정수를 이용해서 실수를 표현한다. 
    - 실수를 정수와 10의 제곱으로 표현
- BigInteger처럼 **불변(immutable)**이다.
- double의 정밀도 한계가 있어서 사용하는 클래스이므로 double타입으로 생성 시 오차 발생 가능성이 있다.

### BigDecimal 생성

문자열로 숫자를 표현하는 것이 일반적이다.

```java
import java.math.BigDecimal;

BigDecimal val = new BigDecimal("123.4567890"); // 문자열로 생성

BigDecimal val = new BigDecimal(123.456); // double타입으로 생성 (비추천)

BigDecimal val = new BigDecimal(123456); // int, long 타입으로 생성

BigDecimal val = BigDecimal.valueOf(123.456); // 생성자 대신 valueOf(double)사용

BigDecimal val = BigDecimal.valueOf(123456); // 생성자 대신 valueOf(int) 사용

BigDecimal val = BigDecimal.valueOf(123412345678901L, 2) // 소수점 아래자리수 지정
```

### BigDecimal 형 변환

```java
String toPlainString() // 어떤 경우에도 다른 기호없이 숫자로만 표현

String toString()      // 필요하면 지수형태로 표현할 수 있음
```

| 리턴값 | 메서드명 | 리턴값 | 메서드명 |
| :---: | :---: | :---: | :---: |
| int | intValue() | byte | byteValueExact() |
| long | longValue() | short | shortValueExact() |
| float | floatValue() | int | intValueExact() |
| double | doubleValue() | long | longValueExact() |
|  |  | BigInteger | toBigIntegerExact() |

> `Exact`가 붙은 것은 변환 결과가 변환 타입 범위에 속하지 않으면 ArithmeticException을 발생

```java
BigDecimal val = BigDecimal.valueOf(123.45);  // double -> BigDecimal

int int_val = val.intValue(); // BigDecimal -> int 

long long_val = val.longValue(); // BigDecimal -> long

float float_val = val.floatValue(); // BigDecimal -> float

double double_val = val.doubleValue(); // BigDecimal -> double

String String_val = val.toString(); // BigDecimal -> String
```

### BigDecimal 연산

| 메서드 | 설명 |
| :--- | :---: |
|BigDecimal add(BigDecimal val) | 덧셈(this + val) |
|BigDecimal subtract(BigDecimal val) | 뺄셈(this - val) |
|BigDecimal multiply(BigDecimal val) | 곱셈(this * val) |
|BigDecimal divide(BigDecimal val) | 나눗셈(this / val) |
|BigDecimal remainder(BigDecimal val) | 나머지(this % val) |

```java
BigDecimal num1 = new BigDecimal("123.45");
BigDecimal num2 = new BigDecimal("100");

System.out.println("덧셈(+) : " +num1.add(num2)); // num1 + num2

System.out.println("뺄셈(-) : " +num1.subtract(num2)); // num1 - num2

System.out.println("곱셈(*) : " +num1.multiply(num2)); // num1 * num2

System.out.println("나눗셈(/) : " +num1.divide(num2)); // num1 / num2

System.out.println("나머지(%) : " +num1.remainder(num2)); // num1 % num2
```

#### 정수, 지수, 정밀도

연산결과는 부호, 지수, 정밀도에 따라 달라진다.

```java
BigDecimal bd = new BigDecimal("-12345.6789")

bd.precision(); // 정밀도 : 9

bd.scale(); // 지수값 : 4

bd.signum(); // 부호 : -1(음수)
```


### 반올림 모드 divide(), setScale()

divide(첫번째 인자(실수값), 두번째 인자(소수점 몇자리 지정), 세번째 인자(반올림 정책));

```java
BigDecimal divide(BigDecimal divisor)
BigDecimal divide(BigDecimal divisor, int roundingMode)
BigDecimal divide(BigDecimal divisor, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)
BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, MathContext mc)
```

**열거형 RoundingMode에 정의된 상수**

|상수 | 설명 |
|---|--- |
| RoundingMode.CEILING| 올림 |
| RoundingMode.FLOOR | 내림 |
| RoundingMode.UP | 양수일 때는 올림, 음수일 때는 내림 |
| RoundingMode.DOWN | 양수일 때는 내림, 음수일 때는 올림(UP과 반대) |
| RoundingMode.HALF_UP | 반올림(5이상 올림, 5미만 버림) |
| RoundingMode.HALF_EVEN | 반올림(반올림 자리의 값이 짝수면 **HALF_ DOWN**, 홀수면 **HALF_ UP**)|
| RoundingMode.HALF_DOWN | 반올림(6이상 올림, 6미만 버림) |
| RoundingMode.UNNECESSARY | 나눗셈의 결과가 딱 떨어지는 수가 아니면 ArithmeticException발생| 

### java.math.MathContext

반올림 모드와 정밀도(precision)을 하나로 묶어 놓은 것이다.

```java
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("1.0");

System.out.println(bd1.divide(bd2, 2, HALF_UP)); // 123.46
System.out.println(bd1.divide(bd2, new MathContext(2, HALF_UP))); // 1.2E+2
```

| 상수 | 설명 |
| --- | --- | 
| MathContext.DECIMAL32 | 7자리 정밀도 및 HALF_EVEN의 반올림 모드 |
| MathContext.DECIMAL64 | 16자리 정밀도 및 HALF_EVEN의 반올림 모드 |
| MathContext.DECIMAL128 | 34자리 정밀도 및 HALF_EVEN의 반올림 모드 |
| MathContext.UNLIMITED | 무제한 정밀 산술 |

### scale 변경

BigDecimal의 scale을 변경하려면, setScale()을 이용한다. 
setScale()로 scale을 값을 줄이는 것은 10의 n제곱으로 나누는 것과 같으므로, divide()를 호출할 때처럼 오차가 발생할 수 있고 반올림 모드를 지정해줘야 한다. 

```java
BigDecimal setScale(int newScale)
BigDecimal setScale(int newScale, int roundingMode)
BigDecimal setScale(int newScale, RoundingMode mode)
```