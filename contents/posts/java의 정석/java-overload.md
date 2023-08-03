---
title: "[Java] 오버로딩(overloading)"
description: "오버로딩, 가변 인수"
date: 2023-07-25
update: 2023-07-25
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 오버로딩(overloading)

한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것을 메서드 오버로딩(method overloading), 또는 오버로딩(overloading)이라 한다.

### 오버로딩이란?

자바에서는 한 클래스 내에 이미 사용하려는 이름과 같은 이름을 가진 메서드가 있더라도 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메서드를  정의할 수 있다.

### 오버로딩 조건
```
1. 메서드 이름이 같아야 한다.
2. 매개변수의 개수 또는 타입이 달라야 한다.
```
오버로딩된 메서드들은 매개변수에 의해서만 구별될 수 있으므로 **반환타입은 오버로딩을 구현하는데 아무런 영향을 주지 못**한다. 리턴값만 다른 것은 오버로딩할 수 없다.
> **접근제어자도 자유롭게 지정이 가능**하다. 주로 `public`, `default`, `protected`, `private`으로 다르게 지정해도 상관은 없지만 **접근제어자만 다르다고 오버로딩이 가능한 것은 아니**다.

### 오버로딩 예시

오버로딩의 예로 대표적인 것은 **`println`메서드**이다.
```java
void println()
void println(boolean x)
void println(char x)
void println(char[] x)
void println(double x)
void println(float x)
void println(int x)
void println(long x)
void println(Object x)
void println(String x)
```
#### 예시1

```java
int add(int a, int b) { return a + b; }
int add(int x, int y) { return x + y; }
```
두 메서드는 매개변수의 **이름**만 다를 뿐 **매개변수의 타입이 같기** 때문에 오버로딩이 성립하지 않는다.

#### 예시2
```java
int add(int a, int b) { return a + b; }
long add(int a, int b) { return (long)(a + b); }
```
리턴타입만 다를 경우, 매개변수의 **타입과 개수**가 일치하기 때문에 오버로딩으로 간주하지 않는다. 

#### 예시3
```java
long add(int a, long b) { return a + b; }
long add(long a, int b) { return a + b; }
```
`int형`과 `long형` 매개변수가 하나씩 선언되어 있지만, 서로 순서가 다른 경우이다. 오버로딩으로 간주한다.

- 오버로딩은 매개변수의 차이로만 구현하는데 매개변수가 다르다면 리턴값을 다르게 지정할 수 있다.

### 오버로딩 장점

- 같은 기능을 하는 메서드를 하나의 이름으로 사용할 수 있다.
- 메서드의 이름을 절약할 수 있다.(=이름 짓는 고민을 덜 수 있다.)

## 가변인자(varargs)

### 가변인자란?

매개변수로 들어오는 값의 개수와 상관없이 동적으로 인수를 받아 가능하도록 해주는 문법이다.
```java
print("ABC")
print("ABC", "DEF")
print("ABC", "DEF", "GHI")
```
여러 개의 매개변수가 들어올 수 있는 메서드를 구성하는 방법은 메서드 오버로딩으로 처리가 가능한다. 하지만 전달할 매개변수가 일정치 않을 때 매번 메서드 오버로딩을 사용하는 게 비효율적이기에 가변인자를 사용한다. 

### 가변인자 형식

가변인자는 `타입...변수명` 형식으로 선언하며, `printf()`가 대표적인 예이다. 
```java
public PrintStream printf(String format, Object...args) { ... }
```
만약 가변인자 외에 다른 매개변수가 있다면 **가변인자를 매개변수 중에서 제일 마지막에 선언**해야 한다.
```java
public PrintStream printf(Object... args, String format) {
  ...// Error
}
```
가변인자인지 아닌지 구별할 방법이 없기 때문이다.

```java
System.out.println(concatenate()); // 인자 없음
System.out.println(concatenate("a")); // 인자 하나
System.out.println(concatenate("a", "b")); // 인자 둘

System.out.println(concatenate(new String[]{"A", "B"}));
```

위와 같이 인자의 개수를 0~n개까지 가변적으로 할 수 있다. 
가변인자는 내부적으로 배열을 이용하는 데 선언된 메서드를 호출할 때마다 배열이 새로 생성된다. 
그래서 메서드 오버로딩을 n번 하지 않고 한번에 처리가 가능하다.

```java
String concatenate(String[] str) { ... }

Stirng result = concatenate(new String[0]); // 인자로 배열 지정
String result = concatenate(null); // 인자로 null 지정
String result = concatenate(); // error
```
매개변수의 타입을 배열로 하면, 반드시 인자를 지정해 줘야 하기 때문에 생략할 수 없다. 
> 배열 자료형은 매개변수 타입으로 명시된 것을 따라간다.

## 참조
- [가변인수 주의점](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B3%80-%EC%9D%B8%EC%88%98Varargs-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EB%A5%BC-%EB%8F%99%EC%A0%81%EC%9C%BC%EB%A1%9C)