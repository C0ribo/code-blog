---
title: "[아이템1] 생성자 대신 정적 팩터리 메서드를 고려하라"
description: "이펙티브 자바, 정적 팩터리 메서드"
date: 2023-09-29
update: 2023-09-30
tags:
  - java
series: "이펙티브 자바"
---

##  생성자 대신 정적 팩터리 메서드를 고려하라

정적 패터리 메서드(Static Factory Method) 패턴은 개발자가 구성한 Static Method를 통해 간접적으로 생성자를 호출하는데 객체를 생성하는 디자인 패턴이다. 객체 생성의 역할을 하는 클래스 메서드를 통해 간접적으로 객체 생성을 유도하는 것이다. 

### 정적 팩터리 메서드 장점

#### 첫 번째, 이름을 가질 수 있다.

정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다. 정적 메서드를 통해 적절한 메서드 네이밍을 해준다면 반환될 객체의 특성을 한번에 유추할 수 있게 된다. 

```java
class Car {
    private String brand;
    private String color;

    // private 생성자
    private Car(String brand, String color) {
        this.brand = brand;
        this.color = color;
    }

    // 정적 팩토리 메서드 (매개변수 하나는 from 네이밍)
    public static Car brandBlackFrom(String brand) {
        return new Car(brand, "black");
    }

    // 정적 팩토리 메서드 (매개변수 여러개는 of 네이밍)
    public static Car brandColorOf(String brand, String color) {
        return new Car(brand, color);
    }
}
```

```java
public static void main(String[] args) {
    // 검정색 테슬라 자동차 
    Car teslaCar = Car.brandBlackFrom("Tesla");

    // 빨간색 BMW 자동차
    Car bmwRedCar = Car.brandColorOf("BMW", "Red");
}
```

생성자 대신 정적 팩토리 메서드를 호출함으로써 생성될 객체의 특성에 대해 쉽게 묘사할 수 있어서 코드의 가독성을 높여준다.

> 정적 팩토리 메서드를 구성하고자 하면, 반드시 생성자에 private 접근 제어자를 두어 외부에서 new 키워드를 이용하여 객체를 생성한다.

#### 두 번째, 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

불변 클래스(immutable class)는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱(Caching)하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다. 인스턴스의 생성에 관여하여, 생성되는 인스턴스의 수를 통제할 수 있는 클래스를 인스턴스 통제(instance-controlled) 클래스라고 한다. 

인스턴스를 통제하면 클래스를 싱글턴(singleton)으로 만들 수도, 인스턴스화 불가(noninstantiable)로 만들수도 있다. 대표적인 예로 Boolean.valueOf(boolean) 메서드는 객체를 아예 생성하지 않는다. 그외에도 Singleton 디자인 패턴에서, getInstance() 라는 정적 팩터리 메서드를 사용해 하나의 객체만 반환하도록 하여 객체를 재사용해 메모리를 아끼도록 유도할 수 있다.

<details>
<summary>Singleton 코드 예시</summary>
<div markdown="1">       

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    // 정적 팩토리 메서드
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

```java
public static void main(String[] args) {
    Singleton s1 = Singleton.getInstance();
    Singleton s2 = Singleton.getInstance();
    Singleton s3 = Singleton.getInstance();

    System.out.println(s1);
    System.out.println(s2);
    System.out.println(s3);

    System.out.println(s1 == s2);
    System.out.println(s1 == s3);
}
```
[출처](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)

</div>
</details>


#### 세 번째, 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

API를 만들 때 이 유연성을 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다. 인터페이스를 정적 팩터리 메서드의 반환 타입으로 사용하는 인터패이스 기반 프레임워크를 만드는 핵심 기술이기도 하다.

<details>
<summary>코드 예시</summary>
<div markdown="1">       

```java
interface SmarPhone {}

class Galaxy implements SmarPhone {}
class IPhone implements SmarPhone {}
class Huawei implements SmarPhone {}

class SmartPhones {
    public static SmarPhone getSamsungPhone() {
        return new Galaxy();
    }

    public static SmarPhone getApplePhone() {
        return new IPhone();
    }

    public static SmarPhone getChinesePhone() {
        return new Huawei();
    }
}
```

[코드 출처](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)

</div>
</details>

자바 8부터는 인터페이스가 정적 메서드를 가질 수 없다는 제한이 풀렸기 때문에 인스턴스화 불가 동반 클래스를 둘 이유가 별로 없다. public 정적 멤버들 상당수를 그냥 인터페이스 자체에 두면 된다.

<details>
<summary>코드 예시2</summary>
<div markdown="1">       

```java
interface SmarPhone {
    public static SmarPhone getSamsungPhone() {
        return new Galaxy();
    }

    public static SmarPhone getApplePhone() {
        return new IPhone();
    }

    public static SmarPhone getChinesePhone() {
        return new Huawei();
    }
}
```
[코드 출처](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)

</div>
</details>

#### 네 번째, 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. EnumSet 클래스는 public 생성자 없이 오직 정적 팩터리만 제공하는데, OpenJDK에서는 원소의 수에 따라 두 가지 하위 클래스 중 하나의 인스턴스를 반환한다. 
- 원소가 64개 이하면 원소들을 long 변수 하나로 관리하는 `RegularEnumSet`의 인스턴스를 반환
- 원소가 65개 이상이면 long 배열로 관리하는 `JumboEnumSet`의 인스턴스를 반환

메서드이니 매개변수를 받을 수 있을거고, 메서드 블록 내에서 분기문을 통해 여러 자식 타입의 인스턴스를 반환하도록 응용 구성이 가능하다.

#### 다섯 번째, 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

생성자를 사용하는 경우 외부에 내부 구현을 드러내야 하는데, 정적 팩토리 메서드는 구현부를 외부로부터 순길 수 있어 캡슐화(encapsulation) 및 정보 은닉(information hiding)을 할 수 있다는 특징이 있다. 

### 정적 팩터리 메서드 단점

#### 첫 번째, 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

정적 팩토리 메서드로 클래스를 설계하면 생성자를 private 접근 제어자로 설정하게 된다. 정적 팩터리 메서드를 적용할 경우에는 상속을 이용한 확장이 불가능해진다.

Collections 클래스는 생성자가 private로 되어 있는데, 일부로 상속을 하게 하지 않기 위한 것으로 Collections는 컬렉션에 대한 헬퍼 제공 용도일 뿐, 상속으로 확장하는 용도가 아니기 때문에 상속보다는 합성을 하도록 유도하는 용이다. 

#### 두 번째, 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

정적 팩터리 메서드는 개발자가 임의로 만든 메서드이기 때문에 많은 메서드 중에서 정적 팩터리 역할을 하는 메서드를 직접 찾아서 이해해야 하기 때문이다.

### 정적 팩터리 메서드에 사용하는 명명 방식

- **from** : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드

```java
Date d = Date.from(instance);
```

- **of** : 여러 매개변수를 받아서 적합한 타입의 인스턴스를 반환하는 집계 메서드

```java
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
```

- **valueOf** : from과 of의 더 자세한 버전

```java
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

- **instance** or **getInstance** : (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지 않는다.

```java
StackWalker luke = StackWalker.getInstance(options);

Calendar instance = Calendar.getInstance();
```

- **create** or **newInstance** : 매번 새로운 인스턴스를 생성해 반환함을 보장한다.

```java
Object newArray = Array.newInstance(classObject, arrayLen);
```

- **getType** : `getInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. **get[OrderType]**은 다른 타입의 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.

```java
FileStore fs = Files.getFileStore(path);
```

- **newType** : `newInstance`와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. **new[OrderType]**은 항상 다른 타입의 새로운 인스턴스를 생성.

```java
BufferedReader br = Files.newBufferedReader(path);
```