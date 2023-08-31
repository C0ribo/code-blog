---
title: "[Java] 다형성(polymorphism)"
description: "다형성, 업캐스팅, 다운캐스팅, 참조변수의 형변환, instanceof연산자, 매개변수의 다형성"
date: 2023-08-02
update: 2023-08-02
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

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

## instanceof연산자

instanceof연산자는 객체에 대한 클래스(참조형) 타입에만 사용할 수 있으며 참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 쓰인다.

주로 조건문에 쓰이며, 참조변수 instanceof 타입(클래스명) 순으로 사용한다.
그리고 연산의 결과로 true/false 중 하나로 반환한다.

```java
void doWork(Car c){
  if(c instanceof FireEngine){ // 참조변수 + instanceof + 타입(클래스명)
    FireEngine fe = (FireEngine)c;
    fe.water();
    ...
  }
  else if(c instanceof Ambulance){
    Ambulance a = (Ambulance)c;
    a.siren();
    ...
  }
}
```

instanceof연산자를 이용해서 참조변수 c가 가리키고 있는 인스턴스 타입을 체크하고, 적절히 형변환한 다음에 작업을 해야 한다.

부모타입의 참조변수로는 실제 인스턴스 멤버들을 모두 사용할 수 없기 때문에, 실제 인스턴스와 같은 타입의 참조변수로 형변환을 해야만 인스턴스의 모든 멤버들을 사용할 수 있다.

> 어떤 타입에 대한 instanceof연산의 결과가 true라는 것은 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.

## 매개변수의 다형성

다형성의 특성은 변수의 타입뿐만 아니라 인터페이스나 파라미터에서 똑같이 적용한다.

```java
class Product { 
  int price;  // 제품 가격
  int bonusPoint; // 제품 구매시 제공되는 보너스 점수
}
class Tv extends Product {}
class Computer extends Product {}
class Audio extends Product {}
class Buyer {   // 고객
  int money = 1000; // 소유금액
  int bonusPoint = 0;  // 보너스 점수
}
```

Product클래스는 Tv, Computer, Audio클래스의 부모이고, Buyer클래스는 제품을 구입하는 사람을 클래스로 표현한 것이다.
Buyer클래스에 물건을 구입하는 기능을 추가하고 싶은데 새로운 기능을 구현할 때마다 메서드를 추가적으로 넣어야 하지만, 다형성을 적용하면 간단하게 만들 수 있다.

```java
void buy(Product p){ // Tv, Audio, Computer 포함되어 있다.
  money = money - p.price;
  bonusPoint = bonusPoint + p.bonusPoint;
}
```

매개변수가 Product타입의 참조변수라는 것은, 메서드의 매개변수로 Product클래스의 자식타입의 참조변수면 어느 것이나 매개변수로 받아들일 수 있다는 뜻이다.

## 참조 

- 자바의 정석
- [다형성에 대한 블로그 참조글](https://inpa.tistory.com/entry/OOP-JAVA%EC%9D%98-%EB%8B%A4%ED%98%95%EC%84%B1Polymorphism-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4)
- [업캐스트 다운캐스트 참조 블로그](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85-%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85-%ED%95%9C%EB%B0%A9-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0#instanceof_%EC%97%B0%EC%82%B0%EC%9E%90)