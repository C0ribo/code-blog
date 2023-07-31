---
title: "[Java] 제어자(modifier)"
description: "접근 제어자, static, final, 제어자"
date: 2023-07-31
update: 2023-07-31
tags:
  - java
  - study
series: "Java 기초"
---

```
😯개인 공부용으로 끄적이는 블로그 입니다.
```

## 제어자(modifier)

### 제어자란?

제어자는 클래스, 변수 또는 메서드의 선언부에 함께 사용되어 부가적인 의미를 부여한다.

`접근 제어자` public, protected, default, private<br>
`그 외` static, final, abstract, native, transient, synchronized, volatile, strictfp

클래스나 멤버변수와 메서드에 주로 사용되며, 여러 제어자를 조합하여 사용하는 것이 가능하다. 다만, 접근 제어자는 네 가지 중 하나만 선택할 수 있다.

```java
public static void main()
```

메인 메서드에서 `public`과 `static`을 보면 알 수 있다. <br>
`public`은 **접근 제어자**이고 `static`은 **그외의 제어자**에 해당한다.

### static

static(정적) 멤버들은 static 영역에 생성된다. static멤버변수(클래스변수)는 하나의 변수를 모든 인스턴스가 공유하기 때문에 인스턴스에 관계없이 같은 값을 갖게 된다.

```
static이 사용될 수 있는 곳 : 멤버변수, 메서드, 초기화 블럭
```

| 제어자 |   대상   | 의미                                                                                                                                                             |
| :----: | :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| static | 멤버변수 | - 모든 인스턴스에 공통적으로 사용되는 클래스변수가 된다.<br>- 클래스변수는 인스턴스를 생성하지 않고도 사용 가능하다.<br> - 클래스가 메모리에 로드될 때 생성된다. |
|        |  메서드  | - 인스턴스를 생성하지 않고도 호출이 가능한 `static` 메서드가 된다.<br> - `static`메서드 내에서는 인스턴스멤버들을 직접 사용할 수 없다.                           |

`static` 영역에 할당된 메모리는 모든 객체가 공유하여 하나의 멤버를 어디서든지 참조할 수 있다.

> `static초기화 블럭`은 클래스가 메모리에 로드될 때 한번만 수행되며, `클래스변수(static변수)`를 초기화하는데 사용된다.

```java
class StaticTest {
    static int width = 200; // 클래스변수(static변수- 한 클래스에서 공통적으로 사용되는 변수)
    static int height = 120; // 클래스변스(static변수)
    static {
        // static변수의 복잡한 초기화 수행
    }
    static int max(int a, int b) {
        return a > b ? a : b;
    }
    static void func(){ // 클래스 메서드(static 메서드)
        // static 메서드에서는 this 키워드 사용 불가능(this는 인스턴스 멤버에 대한 접근 키워드)
    }
}
StaticTest.width; // 클래스명.멤버명 으로 접근
StaticTest.func();
```

### final

final은 `변경될 수 없다`는 의미로 사용된다.

- 변수에 사용되면 값을 변경할 수 없는 <span style="color:red">상수</span>가 된다.
- 메서드에 사용하면 <span style="color:red">오버라이딩을 통한 재정의를 할 수 없</span>다.
- 클래스에 사용되면 해당 클래스는 다른 클래스가 <span style="color:red">상속받을 수 없다</span>.

```
final이 사용될 수 있는 곳 : 클래스, 메서드, 멤버변수, 지역변수
```

| 제어자 | 대상  | 의미 |
| --- | --- | --- |
| final  | 클래스| 변경될 수 없는 클래스. 확장될 수 없는 클래스가 된다.<br>그래서 final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.|
||메서드| 변경될 수 없는 메서드. <br>final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.<br> 자신이 만든 메서드에 변경할 수 없게 끔 하고 싶을 때 사용.|
||멤버변수<br>지역변수|변수 앞에 final이 붙으면 값을 변경할 수 없는 상수가 된다.|
||메서드 인자값|인자값 변경이 불가능하다(잘 사용하지는 않는다)|

> final클래스로는 `String`과 `Math`가 있다.

```java
final class FinalTest {     // 부모가 될 수 없는 클래스
    final int MAX_SIZE = 10;    // 값을 변경할 수 없는 멤버변수(상수)
    final void getMaxSize(){    // 오버라이딩할 수 없는 메서드(변경불가)
        final int LV = MAX_SIZE;    // 값을 변경할 수 없는 지역변수(상수)
        return MAX_SIZE;
    }
}
```

```java
public class Parent {
    protected final int finalMethod(){  /// final 메서드 선언
        // ...
    }
}
class Child extends Parent {    // Child가 Parent 상속
    
    protected int finalMethod() {   // 컴파일 오류, 오버라이딩 불가
        // ...
    }
}
```

```java
final class FinalClass {    // final 클래스 선언
    ....
}
class Child extends FinalClass {    // 컴파일 오류, FinalClass 상속불가
    ...
}
```

#### 생성자를 이용한 final멤버 변수의 초기화

1. final이 붙은 변수는 상수이므로 선언과 초기화를 동시에 하지만, 인스턴스변수의 경우 생성자에게 초기화 되도록 할 수 있다.
2. 클래스 내에 매개변수를 갖는 생성자를 선언하여, 인스턴스를 생성할 때 final이 붙은 멤버변수를 초기화하는데 필요한 값을 생성자의 매개변수로 제공받는 것이다.


### abstract

### 접근 제어자(access modifier)

### 제어자의 조합
