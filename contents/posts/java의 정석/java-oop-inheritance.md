---
title: "[Java] 상속(inheritance)"
description: "상속, 자식 클래스, 부모 클래스, "
date: 2023-07-28
update: 2023-07-28
tags:
  - java
  - study
series: "Java 기초"
---

## 상속(inheritance)

### 상속의 정의와 장점

상속이란, 기존 클래스를 재사용하여 새로운 클래스를 작성한다.

```
1. 적은 양의 코드로 새로운 클래스 작성
2. 공통적으로 관리
3. 코드의 추가 및 변경이 매우 용이
```

작성하고자 하는 클래스 이름 뒤에 상속받고자 하는 클래스 이름을 `extend`와 같이 써주면 된다.

```java
class Child extends Parent {
  // ... 
}
```

상속해주는 클래스를 `조상 클래스`라 하고 상속 받는 클래스를 `자손 클래스`이다.

- **조상 클래스** : 부모(parent)클래스, 상위(super)클래스, 기반(base)클래스
- **자손 클래스** : 자식(child)클래스, 하위(sub)클래스, 파생된(derived)클래스

```java
class Parent { } // 부모 클래스 생성
class Child extends Parent { } // 자식 클래스 
```

![](https://github.com/C0ribo/code-blog/assets/133131980/6aec21e2-5464-4f69-b4d0-7e92ac7cddf4)

자손 클래스는 조상 클래스의 모든 멤버를 상속받기 때문에, 위의 다이어그램처럼 **Child클래스는 Parent클래스의 멤버들을 포함**한다.

- Child클래스에 새 코드가 추가되면 Parent클래스는 아무런 영향을 받지 않는다.
  * 부모클래스가 변경되면 자식 클래스는 자동적 영향을 준다.
- 자식 클래스는 부모 클래스의 모든 멤버를 상속 받으므로 항상 부모 클래스보다 같거나 많은 멤버를 갖는다.
  * 상속에 상속을 추가할수록 상속받는 클래스 멤버 개수는 점점 늘어난다.

```
- 생성자와 초기화 블럭은 상속되지 않는다.(멤버만 상속)
- 자식 클래스의 멤버 개수는 부모 클래스보다 항상 같거나 많다.
```

> 접근제어자가 private 또는 default인 멤버들은 상속되지 않는다기보다 상속은 받지만 자손 클래스로부터 접근이 제한된다.

#### 접근제어자 

![](https://github.com/C0ribo/code-blog/assets/133131980/e795b612-f470-43c6-bd4b-cef08746ee24)

- **public** : 모두 허용
- **protected** : 다른 패키지에서 접근 불가(상속 시 가능)
- **default** : 다른 패키지에서 접근 불가
- **private** : 같은 클래스에서만 허용

#### Parent클래스로부터 상속받는 Child와 Child2

```java
class Parent { }
class Child extends Parent { }
class Child2 extends Parent { }
```

![](https://github.com/C0ribo/code-blog/assets/133131980/aceff6f2-1b7d-40e7-91da-7e5246dc693e)

Parent와 Child 간의 상속관계, Parent와 Child2 간의 상속관계는 존재하지만 Child와 Child2 간에는 아무런 관계도 성립되지 않는다. 부모와 자식의 관계 뿐이다.

#### Parent 클래스와 Child 클래스, Child2 클래스 그리고 GrandChild

```java
class Parent { }
class Child extends Parent { }
class Child2 extends Parent { }
class GrandChild extends Child { }
```

![](https://github.com/C0ribo/code-blog/assets/133131980/c08d803a-7dd7-49fc-b40c-3703398ab5c7)

GrandChild는 Child의 직접 자손이고 Parent의 간접 자손이기 때문에 모든 멤버를 다 가지고 있다. 

### 클래스 간의 관계 - 포함관계

상속 외에 재사용하는 방법은 `포함(Composite)관계`를 맺어주는 것이다. 클래스 간의 포함관계를 맺어 주는 것은 **한 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언**하는 것을 말한다. 

```java
// point class
class Point {
  int x; // x좌표
  int y; // y좌표
}
---------------
// Circle class
class Circle {
  int x; // 원점의 x좌표
  int y; // 원점의 y좌표
  int r; // 반지름
}
----------------
// Point 클래스를 재사용해서 Circle클래스 작성
class Circle {
  Point c = new Point(); // 원점
  int r;
}
```

단위별로 여러 개의 클래스를 작성하고, 이 단위 클래스들을 포함관계로 재사용하면 보다 간결하게 사용할 수 있다.

### 클래스 간의 관계 결정하기

```java
// 포함 관계 : 원은 점을 가지고 있다.
class Circle {
  Point c = new Point();
  int r;
}

// 상속 관계 : 원은 점이다.
class Circle extends Point {
  int r;
}
```

- **상속관계** : `~은 ~이다`
- **포함관계** : `~은 ~을 가지고 있다`


### 단일 상속(single inheritance)

다른 객체지향언어인 C++는 여러 부모 클래스로부터 상속받는 것이 가능하지만 자바는 오직 단일 상속만 허용한다. 

```java
class TVCR extends TV, VCR { // error
  // ... 
}
```

- 다중상속 허용 시
  * 복합적인 기능을 가진 클래스를 쉽게 작성가능
  * 클래스 간에 관계가 복잡해지고 이름이 같을 경우 구별할 수 없다.
- 단일상속 시
  * 하나의 부모 클래스만 가질 수 있어서 불편하다.
  * 클래스 간 관계가 명확해지고 코드를 더 신뢰하게 만들어준다.

### Object클래스 - 모든 클래스의 조상

Object클래스는 모든 클래스 상속계층도의 최상위에 있는 조상클래스이다.
다른 클래스로부터 상속 받지 않은 모든 클래스들은 자동적으로 Object클래스로부터 상속받게 한다.

```java
class Tv extends Objects { 
// extends Object를 추가해서 Tv클래스가 Object클래스로부터 상속
  ...
}
```
Object클래스가 모든 클래스의 조상이 되도록 한다. 

> 이미 어떤 클래스로부터 상속받도록 작성된 클래스에 대해 컴파일러가 `extends Object`를 추가하지 않는다.


#### Tv클래스가 있고 Tv클래스를 상속받는 CaptionTv가 있을 때 상속 계층도

![](https://github.com/C0ribo/code-blog/assets/133131980/0bf7b3f6-7685-422d-b917-0ce1eae523bd)

```java
class Tv{
  ...
}
class CaptionTv extends Tv {
  ...
}
```
모든 상속계층도의 최상위에는 `Object클래스`가 위치한다. 자바의 모든 클래스는 `Object클래스`의 멤버들을 상속 받는다.
> `toString()` `equals(Object o)`와 같은 메서드를 정의하지 않고 사용할 수 있는 건, `Object클래스`에 정의되었기 때문

## 참조

- 자바의 정석