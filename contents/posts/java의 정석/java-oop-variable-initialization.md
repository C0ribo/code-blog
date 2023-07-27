---
title: "[Java] 변수의 초기화"
description: "변수의 초기화, 명시적 초기화, 초기화 블럭"
date: 2023-07-26
update: 2023-07-27
tags:
  - java
  - study
series: "Java 기초"
---

## 변수의 초기화

### 변수의 초기화

변수를 선언하고 처음으로 값을 저장하는 것이다. 경우에 따라 필수적이거나 

- **멤버변수** : 초기화를 하지 않아도 자동적으로 변수의 자료형에 맞는 기본값으로 초기화가 이루어진다.
- **지역변수** : 사용하기 전에 반드시 초기화해야 한다.

```java
class InitTest {
    int x;              // 인스턴스변수
    int y = x;          // 인스턴스변수
    void method1(){
        int i;          // 지역변수
        int j = i;      // error : 지역변수는 사용하기 전 초기화해야 하기 때문
    }
}
```

> 멤버변수(클래스변수와 인스턴스변수)와 배열의 초기화는 선택적이지만 지역변수의 초기화는 필수적이다.

##### - 각 타입의 기본값

| 자료형 | 기본값 |
| :---: | :---: |
| boolean | false |
| char | '\u0000' |
| byte, short, int | 0 |
| long | 0L |
| float | 0.0F |
| double | 0.0d 또는 0.0 |
| 참조형 변수 | null |

##### - 다양한 포기화 방법

| 선언 예 | 설명 |
| :---: | :---: |
| int i = 10; <br> int j = 10; | int형 변수 i를 선언하고 10으로 초기화 한다. <br> int형 변수 j를 선언하고 10으로 초기화 한다. |
| int i = 10 , j = 10; | 같은 타입의 변수는 콤마(,)를 사용해서 함께 선언하거나 초기화 할 수 있다. |
| int i = 10, long j = 10 | **에러.** 타입이 다른 변수는 함께 선언하거나 초기화할 수 없다. |
| int i = 10;<br>int j = i; | 변수 i에 저장된 값으로 변수 j를 초기화한다.<br>변수 j는 i의 값인 10으로 초기화한다. |
| int j = i;<br> int i = 10; | **에러.** 변수 i가 선언되기 전에 i를 사용할 수 없다. |

##### - 멤버변수의 초기화 방법

```
1. 명시적 초기화(explict initialization)
2. 생성자(constructor)
3. 초기화 블럭(initialization block)
  - 인스턴스 초기화 블럭 : 인스턴스변수를 초기화 하는데 사용
  - 클래스 초기화 블럭 : 클래스변수를 초기화 하는데 사용
```

### 명시적 초기화(explicit initialization)

변수를 선언과 동시에 초기화하는 것을 **명시적 초기화**라고 한다.

```java
class Car {
  int door = 4; // 기본형(primitive) 변수의 초기화
  Engine e = new Engine(); // 참조형(reference type) 변수의 초기화
  // (생략)
}
```

- 명시적 초기화가 간단하지만, **복잡한 초기화 작업이 필요할 때** `초기화 블럭` 또는 `생성자`를 사용해야 한다.

### 초기화 블럭(initialization block)

초기화 블럭에는 **클래스 초기화 블럭**과 **인스턴스 초기화 블럭** 두 가지 종류가 있다.
- **클래스 초기화 블럭** : 클래스변수의 복잡한 초기화에 사용
- **인스턴스 초기화 블럭** : 인스턴스변수의 복잡한 초기화에 사용

- **클래스 초기화 블럭 작성** : 인스턴스 초기화 블럭 앞에 단순히 `static`을 붙이면 된다.
- **인스턴스 초기화 블럭 작성** : 단순히 클래스 내에 블럭`{}`만들고 그 앞에 코드를 작성한다.

```java
class InitBlock {
  static { /* 클래스 초기화블럭 입니다. */ }

  { /* 인스턴스 초기화 블럭 */}
}
```

`클래스 초기화 블럭`은 클래스가 **메모리에 처음 로딩될 때 한번만 수행**되고, `인스턴스 초기화 블럭`은 **생성자와 같이 인스턴스를 생성할 때 마다 수행**된다.(생성자보다 인스턴스 초기화 블럭이 먼저 수행된다)

> 클래스가 처음 로딩될 때 **클래스변수들이 자동적으로 메모리에 만들**어지고, 클래스 초기화블럭이 **클래스변수들을 초기화되게 되는 것**이다.

```java
Car() {
  count++;  // 중복 코드
  serialNo = count;
  color = "white";
  gearType = "Auto";
}
Car(String color, String gearType){
  count++;    // 중복 코드
  serialNo = count;
  this.color = color;
  this.gearType = gearType;
}

-------------

{
  // 인스턴스 초기화 블럭
  count++; 
  serialNo = count;
}
Car() {
  color = "white";
  gearType = "Auto";
}
Car(String color, String gearType){
  this.color = color;
  this.gearType = gearType;
}
```
중복을 제거하므로써 코드의 신뢰성을 높여 주고, 오류의 발생 가능성을 줄여준다.

```java
class BlockTest {
  static { // 클래스 초기화 블럭 : 인스턴스 초기화 블럭 앞에 단순히 static 붙임
    System.out.println("Static {}");
  }
  { // 인스턴스 초기화 블럭 : 클래스 내에 블럭 {} 만든 후 코드 작성
    System.out.println(" { } ");
  }
  public BlockTest() {
    System.out.println("생성자");
  }
  public static void main(String[] args){
    System.out.println("BlockTest bt = new BlockTest(); ");
    BlockTest bt = new BlockTest();

    System.out.println("BlockTest bt2 = new BlockTest();" );
    BlockTest bt2 = new BlockTest();
  }
}
```

### 멤버변수의 초기화 시기와 순서

```
클래스변수의 초기화시점 : 클래스가 처음 로딩될 때 단 한번 초기화된다.
인스턴스변수의 초기화시점 : 인스턴스가 생성될 때마다 각 인스턴스로 초기화가 이루어진다.

클래스변수의 초기화순서 : 기본값 -> 명시적초기화 -> 클래스 초기화 블럭
인스턴스변수의 초기화순서 : 기본값 -> 명시적초기화 -> 인스턴스 초기화 블럭 -> 생성자
```

```java
class InitTest {
      // 명시적 초기화
  static int cv = 1;
  int iv = 1;
      // 클래스 초기화 블럭
  static { cv = 2; }
      // 인스턴스 초기화 블럭
  { iv = 2; }
      // 생성자
  InitTest() {
    iv = 3;
  }
}
```

`new InitTest();`와 같이 인스턴스를 생성했을 때, `cv`와 `iv`가 초기화되는 과정을 단계별로 보자.

![](https://github.com/C0ribo/code-blog/assets/133131980/e12638df-cff5-4c99-b391-7f45cb66e178)

- **클래스변수 초기화(1~3)** : 클래스가 처음 메모리에 로딩될 때 차례대로 수행
- **인스턴스변수 초기화(4~7)** : 인스턴스를 생성할 때 차례대로 수행

> 클래스변수는 항상 인스턴스변수보다 항상 먼저 생성되고 초기화된다.

```
1. cv가 메모리(method area)에 생성, cv는 int형의 기본값인 0이 cv에 저장
2. 명시적 초기화(int cv = 1)에 의해 cv에 1이 저장
3. 클래스 블럭 초기화 블럭(cv=2)이 수행되어 cv에는 2가 저장
4. InitTest클래스의 인스턴스가 생성되면서 iv가 메모리(heap)에 존재함
   iv 역시 int형 변수이므로 기본값 0이 저장
5. 명시적 초기화에 의해 iv에 1이 저장
6. 인스턴스 초기화 블럭이 수행되어 iv에 2가 저장
7. 생성자가 수행되어 iv에는 3이 저장
```

## 참고

- 자바의 정석