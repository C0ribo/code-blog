---
title: "[Java] 다형성(polymorphism)"
description: "다형성, 업캐스팅, 다운캐스팅, 참조변수의 형변환, instanceof연산자, 매개변수의 다형성,"
date: 2023-08-02
update: 2023-08-02
tags:
  - java
  - study
series: "Java 기초"
---

## 다형성이란?

객체지향개념에서 다형성이란, 여러 가지 형태를 가질 수 있는 능력을 의미하며 즉, 같은 자료형에 여러 가지 타입의 데이터를 대입하여 다양한 결과를 얻어낼 수 있는 성질을 말한다. 
자바에서는 오버로딩, 오버라이딩, 업캐스팅, 다운캐스팅, 인터페이스, 추상메서드, 추상클래스 방법 모두 다형성에 속한다. 

구체적으로 말하자면 부모클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있도록 하였다는 것이다. 

```java
class Tv {
    boolean power;
    int channel;

    void power() { power = !power; }
    void channelUp() { ++channel; }
    void channelDown() { --channel; }
}
class CaptionTv extends Tv{
    String text;    // 캡션을 보여주기 위한 문자열
    void caption() { /* 생략 */}
}
```

위의 코드로 Tv클래스와 CaptionTv클래스는 서로 상속관계라는 것을 알 수 있다.
![](https://github.com/C0ribo/code-blog/assets/133131980/37176594-e7bc-4317-9219-39ff1eb2947e)

이 두 클래스의 인스턴스를 생성하고 사용하기 위해서는 다음과 같이 나타낼 수 있다.

```java
Tv t = new Tv();
Caption c = new Caption();
```

여태, 동일한 클래스타입의 참조 타입 변수를 생성해 초기화하여 사용해왔다. 
Tv와 CaptionTv클래스 서로 상속관계에 있을 경우, 부모 클래스 타입의 참조변수로 자식 클래스의 인스턴스를 참조하도록 하는 것이 가능하다. 

```java
Tv t = new CaptionTv();     // 부모 타입의 참조변수로 자식 인스턴스를 참조
```

Tv 클래스 타입의 참조변수 t를 선언한다. 
CaptionTv의 인스턴스를 생성하고, 이 인스턴스의 주소값을 참조변수 t에 대입한다.

반대 상황을 보자.

```java
CaptionTv c = new Tv();     // 자식 타입의 참조변수로 부모 인스턴스 참조
```

에러가 난다. 그 이유는 실제 Tv의 멤버 개수보다 참조변수 c가 사용할 수 있는 멤버 개수가 더 많기 때문이다. 
자식타입의 참조변수로 부모타입의 인스턴스를 참조하는 것은 존재하지 않은 멤버를 사용하고자 할 가능성이 있으므로 허용하지 않는 것이다. 

> 부모타입의 참조변수로 자식타입의 인스턴스를 참조할 수 있다.<br>반대로 자식타입의 참조변수로 부모타입의 인스턴스를 참조할 수 없다.

## 참조변수의 형변환

하나의 데이터 타입으로 바꾸는 것은 타입변환 혹은 형변환(캐스팅)이라 한다.
기본형 변수와 같이 참조변수도 형변환이 가능하다.

```
기본형(primitive type) - Boolean Type(boolean) - Numeric Type(short, int, 
long, float, double, char)

참조형(reference type) - Class Type - Interface Type - Array Type - Enum Type
```

자바의 상속관계에 있는 부모와 자식 클래스 간의 서로 간 형변환이 가능하다. 모든 참조변수는 모든 클래스의 조상인 Object클래스 타입으로 형변환이 가능하다. 

![](https://github.com/C0ribo/code-blog/assets/133131980/515277c3-1285-4c1a-98c2-abff6cb2be1e)

- 부모타입의 참조변수를 자식타입의 참조변수로 변환하는 것은 다운캐스팅(down-casting)이라 한다.
- 자식타입의 참조변수를 부모타입의 참조변수로 변환하는 것을 업캐스팅(up-casting)이라 한다.

참조변수간의 형변환 역시 캐스트연산자를 사용하며, 괄호()안에 변환하고자 하는 타입의 이름(클래스명)을 적어주면 된다.

![](https://github.com/C0ribo/code-blog/assets/133131980/a3ca90b3-fe83-4eab-96b2-9ecb78f25c8b)

같은 부모 클래스를 상속받고 있더라도 형제 클래스끼리는 아예 타입이 다르기 때문에 참조 형변환이 불가능하다. 

```java
FireEngine f; // 자식 클래스 1
Ambulance a; // 자식 클래스 2
a = (Ambulance)f;   // error
f = (FireEngine)a;  // error
```

Ambulance타입의 참조변수와 FireEngine타입의 참조변수 간에는 서로 형변환이 불가능하다. 

```java
Car car = null; 
// Car타입의 참조변수 car를 선언하고 null로 초기화
FireEngine fe = new FireEngine(); 
// FireEngine인스턴스를 생성하고 FireEngine타입 참조변수가 참조하도록 한다.
FireEngine fe2 = null;

car = fe;   // car = (Car)fe; 형변환 생략. 업 캐스팅
fe2 = (FireEngine)car; // 형변환 생략 불가. 다운 캐스팅
```

업 캐스팅은 형변환 생략이 가능한 반면 다운 캐스팅은 형변환 생략이 불가능하다. 
형변환을 수행하기 전에 instanceof연산자를 사용하여 참조변수가 참조하고 있는 실제 인스턴스 타입을 확인하는 것이 좋다. 

형변환은 참조변수의 타입을 변환하는 것이지 인스턴스를 변환하는 것이 아니기에 참조변수의 형변환은 인스턴스에 아무런 영향을 미치지 않는다. 참조변수의 형변환은 인스턴스에서 사용할 수 있는 멤버의 범위 개수를 조절하는 것이다. 

> 서로 상속관계에 있는 타입간의 형변환은 양방향으로 자유롭게 수행될 수 있으나, 참조변수가 가리키는 인스턴스의 자손타입으로 형변환은 허용되지 않는다.<br>그래서 참조변수가 가리키는 인스턴스의 타입이 무엇인지 확인하는 것이 중요하다.