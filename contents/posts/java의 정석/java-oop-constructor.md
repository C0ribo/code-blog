---
title: "[Java] 생성자(constructor)"
description: "생성자, 기본 생성자, this()"
date: 2023-07-25
update: 2023-07-26
tags:
  - java
  - study
series: "Java 기초"
---

## 생성자

### 생성자란?

생성자는 인스턴스(객체)가 생성될 때 호출되는 인스턴스 초기화 메서드이다. 
```
1. 생성자의 이름은 클래스의 이름과 같아야 한다.
2. 생성자는 리턴값이 없다(void 사용 x)
```
> 생성자도 메서드이기에 리턴값이 없으면 `void`를 붙어야 하지만, 모든 생성자가 리턴값이 없어서 `void`가 생략가능 한 것이다.

생성자는 오버로딩이 가능해서 여러개의 생성자가 존재할 수 있다.
```java
클래스이름(타입 변수명, 타입 변수명, ...){
    // 인스턴스 생성 시 수행될 코드,
    // 주로 인스턴스 변수의 초길하 코드를 적음
}

class Card(){
    Card() { // 매개변수가 없는 생성자
        ...
    }
    
    Card(String k , int num){ // 매개변수가 있는 생성자
        ...
    }
}
```
첫 문장에서 말했다시피 객체를 생성할 때 `new` 연산자를 쓰면서 생성자를 호출하게 되는 것이다. 
```java
Card c = new Card(); // 클래스명 변수명 = new 클래스명();
Card c = new Card(String k , int num); // 클래스명 변수명 = new 클래스명(매개변수, 매개변수);
1. 연산자 new에 의해서 메모리(heap)에 Card클래스의 인스턴스가 생성된다.
2. 생성자 Card()가 호출되어 수행된다.
3. 연산자 new의 결과로, 생성된 Card인스턴스의 주소가 반환되어 참조변수 c에 저장된다.
```

### 기본 생성자(default constructor)

- 모든 클래스에는 반드시 **하나 이상의 생성자가 정의**되어 있어야 한다.
- 클래스 내에 생성자를 정의하지 않고 인스턴스를 생성할 수 있었던 이유는 **기본 생성자**가 있기 때문이다.

```
클래스이름() { }
Card() { }
```

기본 생성자는 매개변수도 없고 아무런 명령어도 포함되어 있지 않는다.
> 접근 제어자가 `public`인 경우에 기본 생성자로 `public 클래스이름() { }`이 추가된다. 

```java
class Date1 {
    int value;
}
class Data2 {
    int value;
    Data2(int x) {
        value = x;
    }
}
class ConstructorTest {
    public static void main(String[] args){
        Data1 d1 = new Data1(); // ok
        Data2 d2 = new Data2(); // error
        Data2 d2 = new Data2(10); // ok
    }
}
```

- `Data1`에 정의되어 있는 생성자가 하나도 없어서 기본 생성자를 추가해줬지만, `Data2`는 이미 생성자 `Data2(int x)`가 정의되어 있어서 기본 생성자를 따로 추가할 필요가 없다.
- 에러가 나지 않지 위해 인스턴스 생성 시 생성자 `Data2(int x)`를 사용하던가, 클래스 `Data2`에 생성자 `Data2()`를 추가로 정의해준다.

> 기본 생성자가 컴파일러에 의해 추가되는 경우 클래스에 **정의된 생성자가 하나도 없을 때 뿐**이다.

### 매개변수가 있는 생성자

생성자도 **매개변수를 선언하여 호출 시 값을 넘겨받아서 인스턴스의 초기화 작업**에 사용할 수 있다.
```java
class Car {
    String color;
    String gearType;
    int door;

    Car() {} // 매개변수가 없는 생성자
    Car(String c, String g, int d){ // 매개변수가 있는 생성자
        color = c;
        gearType = g;
        door = d;
    }
}
```
- Car 인스턴스를 생성할 때, 생성자 Car()를 사용한다면 **인스턴스를 생성한 후 인스턴스변수들을 따로 초기화**해줘야 하지만, 매개변수가 있는 생성자 Car(String color, String gearType, int door)를 사용하면 **인스턴스 생성과 동시에 초기화**고 할 수 있다.

```java
Car.c = new Car();
c.color = "white";
c.gearType = "auto";
c.door = 4;

Car c = new Car("white", "auto", 4);
```
인스턴스 생성한 다음 인스턴스변수의 값을 변경하는 것보다 **매개변수를 갖는 생성자를 사용**하는 것이 더 좋다.

### this(), this

생성자 간에도 서로 호출이 가능하다.

```
- 생성자의 이름으로 클래스이름 대신 this를 사용한다.
- 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫 줄에서만 호출이 가능하다.
- this와 this()는 비슷하게 생겼을 뿐, 완전히 다르다.
```

#### this 참조변수

- 참조변수를 통해 인스턴스의 멤버에 접근할 수 있는 것처럼, `this`로 **인스턴스변수에 접근**할 수 있다.
- `this`를 사용할 수 있는 것은 **인스턴스멤버** 뿐이다.
- 생성자를 포함한 모든 인스턴스 메서드에서 자신이 관련된 **인스턴스를 가리키는 참조변수 `this`가 지역변수로 숨겨진 채로 존재**한다.

```java
Car(String color, String gearType, int door){ // 생성자(인스턴스 변수 값 초기화)
    this.color = color; // 메서드 입력값으로 인스턴스 변수 값 지정
    this.gearType = gearType;
    this.door = door;
}
```

#### this() 메서드
- 같은 클래스의 다른 생성자를 호출할 때 사용한다. 
- 생성자 내부에서만 사용할 수 있다.

```java
Car() {
    this("white", "auto", 4); // 해당 인자가 일치하는 다른 생성자를 호출한다.
}
public class Main{
    public static void main(String[] args){
        // this() 메서드가 호출되어 결과적으로 
        // Car(String color, String gearType, int door)생성자가 호출되는 결과를 얻는다.
        Car myCar = new Car();
        (생략)
    }
}
```

### 생성자를 이용한 인스턴스 복사

- 같은 상태를 갖는 인스턴스를 하나 더 만들고자 할 때 **생성자**를 이용할 수 있다.
- 두 인스턴스가 같은 상태를 갖는다는 것은 **인스턴스의 모든 인스턴스 변수기 동일한 값**을 갖고 있다는 의미이다.
- 하나의 클래스로부터 생상된 모든 인스턴스의 메서드와 클래스변수는 서로 동일하기 때문에 인스턴스간의 차이는 **인스턴스마다 각기 다른 값을 가질 수 있는 인스턴스변수** 뿐이다.

```java

Car(Car c){ // Car클래스의 참조변수를 매개변수로 선언한 생성자(인스턴스 복사를 위한 생성자)
    // 매개변수로 넘겨진 참조변수가 가리키는 Car인스턴스의 인스턴스 변수인 
    // color, gearType, door의 값을 인스턴스 자신으로 복사
    color = c.color; // 방법1
    gearType = c.gearType;
    door = c.door;

    this(c.color, c.gearType, c.door); // 방법2
}
```

위의 코드와 같이 방법1보단 방법2의 방식으로 호출하는 게 바람직하다.

```인스턴스를 생성할 때 다음의 2가지 사항을 결정해야 한다.
1. 클래스 - 어떤 클래스의 인스턴스를 생성할 것인가?
2. 생성자 - 선택한 클래스의 어떤 생성자로 인스턴스를 생성할 것인가?
```

## 참조

- 자바의 정석
- [자바 객체 지향 클래스 (생성자) 블로그 ](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5OOP-%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%AC%B8%EB%B2%95-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#%EC%83%9D%EC%84%B1%EC%9E%90constructor)