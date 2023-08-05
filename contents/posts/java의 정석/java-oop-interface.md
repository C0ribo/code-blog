---
title: "[Java] 인터페이스(Interface)"
description: "인터페이스"
date: 2023-08-04
update: 2023-08-05
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다.
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 인터페이스란?

인터페이스는 일종의 추상클래스이다. 
추상메서드를 갖지만 추상화 정도가 높아서 몸통을 갖충 일반 메서드 또는 멤버변수를 구성원으로 가질 수 없다.
추상메서드와 상수(static)만을 멤버로 가질 수 있으며, 다른 어떤 요소도 허용하지 않는다.

## 인터페이스의 작성

- 인터페이스는 class 대신 interface를 사용한다.
- interface에는 클래스와 같이 접근제어자로 public 또는 default를 사용할 수 있다.

```java
interface 인터페이스이름 {
    public static final 타입 상수이름 = 값;
    public abstract 메서드이름(매개변수목록);
}
```

> public static final과 public abstract 제어자는 생략이 가능하다.

- 인터페이스에 정의된 모든 멤버에 예외없이 적용되는 사항이기에 제어자를 생략할 수 있으며, 편의상 생략하는 경우가 많다.
- 샹략된 제어자는 컴파일 시에는 컴파일러가 자동적으로 추가한다.

```java
interface PlayingCard {
  public static final int SPACE = 4;
  final int DIAMOND = 3;  // public static final int DIAMOND = 3;
  static int HEART = 2;   // public static final int HEART = 2;
  int CLOVER = 1;         // public static final int CLOVER = 1;

  public abstract String getCardNumber();
  String getCardKind();   // public abstract String getCardKind();
}
```

## 인터페이스의 상속

- 인터페이스는 상속만 받을 수 있고, 다중상속 즉 여러개의 인터페이스로부터 상속받는 것이 가능하다.
- 클래스와 달리 Object클래스와 같은 최고 조상이 없다.

## 인터페이스의 구현

- 인터페이스도 추상클래스처럼 그 자체로 인스턴스를 생성할 수 없으며, 추상클래스가 상속을 통해 완성된 것처럼 인터페이스도 구현부를 만들어주는 클래스에 구현되어야 한다.
- 클래스를 확장하는 의미를 extends 대신에 implements를 사용한다.

```java
class 클래스이름 implements 인터페이스이름 {
  // 인터페이스에 정의된 추상메서드를 구현
}

class Fighter implements Fightable {
  public void move(int x, int y){ /* 생략 */}
  public void attack(Unit u) { /* 생략 */}
}
```

### 인터페이스의 일부구현

구현하는 인터페이스의 메서드 중 일부만 구현한다면, abstract를 붙여서 추상클래스로 선언해야 한다.

```java
abstract 
```