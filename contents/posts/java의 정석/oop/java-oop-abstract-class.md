---
title: "[Java] 추상클래스(abstract class)"
description: "추상클래스, 추상메서드"
date: 2023-08-03
update: 2023-08-04
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다.
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 추상클래스란?

추상클래스는 미완성 설계도에 비유할 수 있다.
클래스가 미완성이라는 것은 멤버의 개수에 관계된 것이 아닌, <span style="color:red">미완성 메서드(추상메서드)를 포함</span>하고 있다는 의미이다. 추상클래스로 인스턴스를 생성할 수 없고 <span style="color:red">상속을 통해 자식클래스에 의해서만 완성</span>할 수 있다.

추상클래스는 `abstract`를 붙이기만 하면 되고, **클래스 선언부의 abstract**를 보고 이 클래스에는 <span style="color:blue">추상메서드가 있으니 상속을 통해 구현</span>해줘야 한다는 것을 알 수 있다.

```java
abstract class 클래스이름 {
  ...
}
```

추상클래스에는 <span style="color:blue">생성자가 있고 독립적인 인스턴스 멤버변수와 메서드도 가질 수 있다.</span>
추상클래스를 포함하고 있지 않은 클래스에도 `abstract`를 붙여서 추상클래스로 지정할 수 있다.
추상메서드가 없는 완성된 클래스라도 **추상클래스로 지정되면 클래스의 인스턴스를 생성할 수 없다.**

## 추상메서드

메서드는 선언부와 구현부로 구성이 되어 있다.
추상메서드는 선언부만 작성하고 구현부는 미완성인 채로 남겨 둔 것이다.

이처럼 미완성인 채로 남겨두는 이유는 <span style="color:blue">메서드의 내용이 상속받는 클래스에 따라 달라질 수 있기 때문</span>이다.
부모 클래스에서는 선언부만 작성하고 실제 내용은 상속받는 클래스에서 구현하도록 비워두는 것이다.
추상클래스를 상속받는 자식 클래스는 부모의 추상메서드를 상황에 맞게 적절히 구현해줘야 한다.

```java
/* 주석을 통해 어떤 기능을 수행할 목적으로 작성하였는지 설명한다. */
abstract 리턴타입 메서드이름();
```

추상클래스로부터 상속받는 **자손클래스는 오버라이딩을 통해 부모인 추상클래스의 추상메서드를 모두 구현**해줘야 한다.
부모로부터 상속받은 추상메서드 중 하나라도 구현하지 않으면, 자식클래스 역시 추상클래스로 지정해줘야 한다.

```java
abstract class Player{    // 추상클래스
  abstract void play(int pos);  // 추상메서드
  abstract void stop();   // 추상메서드
}
class AudioPlayer extends Player{ // 상속받은 부모(추상)메서드를 직접 구현
  void play(int pos){ /* 내용 생략 */ } // 추상메서드 구현
  void stop() { /* 내용 생략 */ } // 추상메서드 구현
}
abstract class AbstractPlayer extends Player {
  void play(int pos) { /* 내용 생략 */ }  // 추상메서드 구현
}
```

메서드를 작성할 때 실제 작업내용인 구현부보다 중요한 것이 선언부이다. 
선언부만 작성해도 메서드의 절반 이상이 완성된 것이라고 할 수 있다.

## 추상클래스의 작성

### 공통멤버의 통합으로 중복제거

```java
class Marine {  // 보병
  int x, y; // 현재위치
  void move(int x, int y){ }  // 지정된 위치로 이동
  void stop() {}  // 현재 위치에 정지
  void stimPack() {} // 스팀팩을 사용
}
class Tank { // 탱크
  int x, y; // 현재 위치
  void move(int x, int y){} // 지정된 위치로 이동
  void stop() {}  // 현재 위치에 정지
  void changeMode() { } // 공격모드로 변환
}
class Dropship {  // 수송선
  int x, int y; // 현재 위치
  void move(int x, int y) {} // 지정된 위치로 이동
  void stop() {} // 현재 위치에 정지
  void load() {} // 선택된 대상 태움
  void unload() {} // 선택된 대상 내림 
}
```

`상속(extends)` 기능을 이용해 3개의 클래스를 대표할 수 있는 부모 추상클래스로 묶으면, 상위 클래스의 특징을 하위클래스에 그대로 물려 받아 사용할 수 있는 상속 특징을 이용하여 <span style="color:blue">코드의 중복 제거, 코드 재사용성 증대 효과</span>를 누릴 수 있게 된다.

```java
abstract class Unit {
  int x, y;
  abstract void move(int x, int y);
  void stop() { } // 현재 위치에 정지
}
class Marine extends Unit { // 보병
  void move(int x, int y) {} // 지정된 위치로 이동
  void stimPack() {}  // 스팀팩을 사용
}
class Tank extends Unit { // 탱크
  void move(int x, int y) {}  // 지정된 위치로 이동
  void changeMode() {}  //공격모드로 변환 
}
class Dropship extends Unit { // 수송선
  void move(int x, int y) { } // 지정된 위치로 이동
  void load() {}  // 선택된 대상을 태움
  void unload() {}  // 선택된 대상을 내림
}
```

각 클래스의 공통부분을 뽑아서 Unit클래스를 정의하고 이로부터 상속받도록 하였다.
`stop()메서드`는 선언부와 구현부 모두 공통적이지만, `move()메서드`는 각 클래스마다 이동하는 방법이 다르기 때문에 <span style="color:red">부모클래스의 메서드를 오버라이딩하여 재정의</span>해주었다.

### 구현의 강제성을 통한 기능 보장

```java
Unit[] group = new Unit[4];
group[0] = new Marine();
group[1] = new Tank();
group[2] = new Marine();
group[3] = new Dripship();

for(int i = 0; i < group.length; i++)
    group[i].move(100, 200);
```

위 코드는 공통부모인 `Unit클래스` 타입의 참조변수 배열을 통해서 서로 다른 종류의 인스턴스를 하나의 묶음으로 다룰 수 있다는 것을 보여준다.
부모 클래스타입의 참조변수로 자식 클래스의 인스턴스를 참조하기는 것이 가능하기 때문에 **부모 클래스타입의 배열에 자식 클래스의 인스턴스를 담을 수 있다.** 

만약, 이들의 공통부모가 없으면 배열로 다룰 수 없다.
메서드는 참조변수의 타입에 관계없이 실제 인스턴스에 구현된 것이 호출되기 때문에 `Unit클래스` 타입의 참조변수로 `move()메서드`로 호출된다. 

```java
Object[] group = new Object[4];
group[0] = new Marine();
group[1] = new Tank();
group[2] = new Marine();
group[3] = new Dripship();

for(int i = 0; i < group.length; i++)
    group[i].move(100, 200);  // error - Object클래스에 move메서드가 정의X
```

모든 클래스 조상인 `Object클래스` 타입의 배열로도 서로 다른 종류의 인스턴스를 하나의 묶음으로 다룰 수 있지만, `Object클래스`에는 `move()메서드`가 정의되어 있지 않기 때문에 `move()메서드`를 호출하는 부분에서 에러가 발생한다.

## 참조

- 자바의 정석
- [추상클래스 참조 블로그](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%B6%94%EC%83%81-%ED%81%B4%EB%9E%98%EC%8A%A4Abstract-%EC%9A%A9%EB%8F%84-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

