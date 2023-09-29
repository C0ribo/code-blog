---
title: "[아이템1] 생성자 대신 정적 팩터리 메서드를 고려하라"
description: "이펙티브 자바, 정적 팩터리 메서드"
date: 2023-09-29
update: 2023-09-29
tags:
  - java
series: "이펙티브 자바"
---

##  생성자 대신 정적 팩터리 메서드를 고려하라


### 정적 팩터리 메서드 장점

#### 첫 번째, 이름을 가질 수 있다.

정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.

#### 두 번째, 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

불변 클래스(immutable class)는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다. 인스턴스의 생성에 관여하여, 생성되는 인스턴스의 수를 통제할 수 있는 클래스를 인스턴스 통제(instance-controlled) 클래스라고 한다. 

인스턴스를 통제하면 클래스를 싱글턴(singleton)으로 만들 수도, 인스턴스화 불가(noninstantiable)로 만들수도 있다.

#### 세 번째, 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

API를 만들 때 이 유연성을 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다. 인터페이스를 정적 팩터리 메서드의 반환 타입으로 사용하는 인터패이스 기반 프레임워크를 만드는 핵심 기술이기도 하다.

자바 8부터는 인터페이스가 정적 메서드를 가질 수 없다는 제한이 풀렸기 때문에 인스턴스화 불가 동반 클래스를 둘 이유가 별로 없다. public 정적 멤버들 상당수를 그냥 인터페이스 자체에 두면 된다.

#### 네 번째, 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. EnumSet 클래스는 public 생성자 없이 오직 정적 팩터리만 제공하는데, OpenJDK에서는 원소의 수에 따라 두 가지 하위 클래스 중 하나의 인스턴스를 반환한다. 
- 원소가 64개 이하면 원소들을 long 변수 하나로 관리하는 RegularEnumSet의 인스턴스
- 원소가 65개 이상이면 long 배열로 관리하는 JumboEnumSet의 인스턴스를 반환한다.

#### 다섯 번째, 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.



### 정적 팩터리 메서드 단점

#### 첫 번째, 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

#### 두 번째, 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.


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