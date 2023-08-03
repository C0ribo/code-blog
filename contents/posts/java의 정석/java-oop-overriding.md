---
title: "[Java] 오버라이딩(overriding)"
description: "오버라이딩 , 오버라이딩 조건, super, super()"
date: 2023-07-29
update: 2023-07-29
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 오버라이딩(overriding)

### 오버라이딩이란?

부모 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라 한다. 주로 자식 클래스에서 자신에 맞게 변경하려고 할 때 부모의 메서드를 오버라이딩한다. 

```java
class Point{ // 부모 클래스
    int x; // 부모 인스턴스 
    int y;

    String getLocation(){  // x, y 좌표를 문자열로 반환
        return "x :" + x + ", y : " + y;
    }
}
class Point3D extends Point { // 자식 클래스
    int z; // 자식 인스턴스 

    String getLocation(){ // 오버라이딩
        return "x :" + x + ", y : " + y + ", z :" + z; 
    }
}
```

### 오버라이딩의 조건

오버라이딩은 메서드의 내용만 새로 작성하므로 메서드의 선언부는 부모의 것과 완전히 일치해야 한다.

```
자식클래스에서 오버라이딩하는 메서드는 부모클래스의 메서드와
- 이름이 같아야 한다.
- 매개변수가 같아야 한다.
- 반환타입이 같아야 한다.
```

다만 접근제어자와 `예외(exception)`는 제한된 조건에만 변경이 가능하다.

**1. 접근제어자는 부모 클래스의 메서드보다 좁은 범위로 변경할 수 없다.**

접근제어자는 앞의 [블로그](https://cori-blog.netlify.app/java-oop-inheritance/)에 작성할 것처럼 `public > protected > default > private` 순으로 접근범위가 줄어드는데 만약 **부모 클래스에 정의된 접근제어자가 protected** 라고 한다면 **자식 클래스에 정의된 접근제어자는 protected와 public만 사용** 가능한 것이다.

**2. 부모 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.**

```java
class Parent { // 부모 클래스 
    void parentMethod() throws IOException, SQLException {
        ...
    }
}
class Child extends Parent { // 자식 클래스
    void parentMethod() throws IOException {
        ...
    }
}
```

위의 코드를 보면 자식 클래스에 선언된 예외의 개수가 부모 클래스에 선언된 예외의 개수보다 적으므로 오버라이딩이 되었다.

```java
class Child extends Parent {
    void parentMethod() throws Exception {
        ...
    }
}
```

`Exception`은 모든 예외의 최고 부모이다. 가장 많은 개수의 예외를 던질 수 있다.

```
부모 클래스의 메서드를 자식 클래스에서 오버라이딩할 때
1. 접근 제어자를 부모 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
2. 예외는 부모 클래스의 메서드보다 많이 선언할 수 없다.
3. 인스턴스메서드를 static 메서드로 또는 그 반대로 변경할 수 없다. 
```

> 부모 클래스에 정의된 `static 메서드`를 자식 클래스에서 똑같은 이름의 static 메서드로 정의할 수 있다.
> 각 클래스 별개의 `static 메서드`를 정의한 것. 오버라이딩은 아니다. 
> 호출 시, `참조변수.메서드이름()` -> `클래스이름.메서드이름()`


### 오버로딩 vs 오버라이딩

**오버로딩(overloading)** 기존에 없는 새로운 메서드를 정의하는 것(new)  
**오버라이딩(overriding)** 상속받은 메서드의 내용을 변경하여 덮어씌우는 것(change, modify)

```java
class Parent {
    void parentMethod() { }
}
class Child extends Parent {
    void parentMethod() { } // 오버라이딩
    void parentMethod(int i) { } // 오버로딩

    void childMethod() { }
    void childMethod(int i){ } // 오버로딩
}
```

| 구분 | 오버로딩 | 오버라이딩 |
| :---: | :---: | :---: |
| 메서드이름 | 동일 | 동일 |
|매개변수, 타입 | 다름 | 동일 |
| 리턴타입 | 상관없음 | 동일 |

### super

super는 자식클래스에서 부모클래스로부터 상속받은 멤버(필드, 메서드)를 참조하는데 사용되는 참조변수이다. 
- 상속받은 멤버와 자신의 멤버와 이름이 같을 때 `super`를 붙여 구별한다. 
- 부모클래스의 멤버와 자식클래스의 멤버가 중복 정의되어 서로 구별할 경우 `super`사용한다.
- `static메서드`에서 사용할 수 없고 **인스턴스메서드에서만 사용**할 수 있다.

```java
class SuperTest {
    public static void main(String args[]){
        Child c = new Child();
        c.method();
    }
}
class Parent{ // 부모 클래스
    int x = 10; // 부모 인스턴스 변수
}
class Child extends Parent { // 자식 클래스
    int x = 20; // 자식 인스턴스 변수

    void method(){
        System.out.println("x=" + x); // 자식 인스턴스 변수 출력
        System.out.println("this.x=" + this.x); // 자식 인스턴스 변수 출력
        System.out.println("super.x="+ super.x); // 부모 인스턴스 변수 출력
    }
}
```

`super.x`는 부모 클래스로부터 상속받은 멤버변수 `x`를 뜻하고, `this.x`는 자식 클래스에 선언된 멤버변수를 뜻한다.

부모 클래스의 메서드를 자식 클래스에서 오버라이딩한 경우 `super`를 사용한다. 

```java
class Point {
    int x;
    int y;
    String getLocation() {
        return "x :" + x + ", y:" + y;
    }
}
class Point3D extends Point {
    int x;
    String getLocation() { // 오버라이딩
        // return "x :" + x + ", y:" + y + ",z: "+ z;
        return super.getLocation() + ", z:" + z; // 부모의 메서드 호출
    }
}
```

부모클래스의 메서드의 내용에 추가적으로 작업을 덧붙이는 경우, `super`를 사용해서 부모클래스의 메서드를 포함시키는 게 좋다.

### super()

`this()`는 같은 클래스의 다른 생성자를 호출하는데 사용한다면, `super()`는 부모 클래스의 생성자를 호출하는 데 사용된다. 

자식 클래스의 인스턴스를 생성하면, 자식의 멤버와 부모의 멤버가 모두 합쳐진 하나의 인스턴스가 생성된다. 그래서 자식 클래스의 인스턴스가 부모 클래스의 멤버들을 사용할 수 있다.

부모 클래스의 멤버를 초기화하기 위해서 **자식 클래스의 생성자에서 부모 클래스의 생성자를 호출**해야 한다. 생성자의 첫 줄에 부모클래스의 생성자를 호출해야 하는 이유는 **자식 클래스 멤버가 부모 클래스의 멤버를 사용할 수 있으므로 부모의 멤버들이 먼저 초기화**되야 한다.

Object클래스를 제외한 **모든 클래스 생성자는 첫 줄에 반드시 자신의 다른 생성자 또는 부모 생성자를 호출**해야 한다. 

>`Object클래스`를 제외한 모든 클래스의 생성자 첫 줄에 생성자, `this()` 또는 `super()` 를 호출해야 한다. 컴파일러가 자동적으로 `super();`를 생성자의 첫 줄에 삽입하기 때문이다.   


```java
class PointTest {
    public static void main(String args[]){
        Point3D p3 = new Point3D(1, 2, 3);
    }
}
class Point{ // 부모 클래스 
    int x, y; // 부모 인스턴스 변수 x, y
    Point(int x, int y){ // 매개변수 있는 생성자
        this.x = x;
        this.y = y;
    }

    String getLocation(){
        return "x :" + x + ", y:" + y;
    }
}
class Point3D extends Point { // 자식 클래스
    int z;  // 자식 인스턴스 변수 

    Point3D(int x, int y, int z){ 
        // super();
        this.x = x;
        this.y = y;
        this.z = z;
    }
    String getLocation(){ // 오버라이딩
        return "x :" + x + ", y:" + y + ", z:" + z;
    }
}
```

위 코드는 에러가 나는 코드이다. 왜냐하면 부모클래스에서 `Point()` 기본 생성자를 찾지 못했기 때문이다. 

위와 같은 현상을 해결하기 위해서는 
1. 부모클래스에게 **기본 생성자를 선언**하거나
    * Point클래스에 생성자 `Point()` 추가
2. 오버로딩된 생성자에 맞춰 `super()` 를 채워줘야 한다. 
    - `Point3D(int x, int y, int z)`의 첫줄에 `Point(int x, int y)` 호출

    ```java
    Point3D(int x, int y, int z){
        super(x, y); // 부모클래스 생성자 Point(int x, int y) 호출
        this.z = z;
    }
    ```
    - **부모 클래스의 멤버변수는 이처럼 부모 생성자에 의해 초기화** 해야 한다.



## 참조 

- 자바의 정석