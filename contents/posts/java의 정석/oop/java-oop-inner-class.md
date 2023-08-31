---
title: "[Java] 내부 클래스(inner class)"
description: "내부 클래스, 인스턴스 클래스, 스태틱 클래스, 지역 클래스, 익명 클래스"
date: 2023-08-07
update: 2023-08-07
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다.
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 내부 클래스란?

내부 클래스는 클래스 내에 선언된 클래스이다.
클래스 내에 다른 클래스를 선언하는 이유는 **두 클래스가 서로 긴밀한 관계에 있고 하나의 클래스 또는 메서드에만 사용**할 수 있기 때문이다.
내부 클래스도 클래스이기 때문에 <span style="color:red">abstract나 final과 같은 제어자를 사용</span>할 수 있고 <span style="color:red">private, protected과 접근제어자도 사용</span> 가능하다.

```
- 클래스를 논리적으로 그룹화하여 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
- 코드의 복잡성을 줄일 수 있다(캡슐화).
    - 내부 클래스에 private 제어자를 적용함으로, 내부로 숨길 수 있다.
- 가독성이 좋고 유지관리가 쉬운 코드
```

A는 **외부 클래스(outer class)**이고 B는 **내부 클래스(inner class)**이다.

```java
class A {   // 외부 클래스
    ...
    class B {   // 내부 클래스
        ...
    }
    ...
}
```

내부 클래스인 B는 외부 클래스인 A를 제외하고 다른 클래스에서 사용되지 않는 것이다.

## 내부 클래스의 종류와 특징

내부 클래스는 변수의 선언 위치와 같이 내부 클래스도 <span style="color:red">선언위치</span>에 따라 구분된다.

| 내부 클래스 | 특 징 |
| --- | --- |
|인스턴스 클래스<br>(instance class)| 외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 인스턴스멤버처럼 다뤄진다. 주로 외부 클래스의 인스턴스멤버들과 관련된 작업에 사용될 목적으로 선언된다.|
|스태틱 클래스<br>(static class)| 외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 static멤버처럼 다루어진다. 주로 외부 클래스의 static멤버, 특히 static메서드에서 사용될 목적으로 선언된다.|
|지역 클래스<br>(local class)| 외부 클래스의 메서드나 초기화블럭 안에 선언하며, 선언된 영역 내부에서만 사용될 수 있다.|
|익명 클래스<br>(anonymous class) | 클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스(일회용) |

```java
class Outer { // 외부 클래스

    // 내부 클래스 
    class InstanceInner {}  // 인스턴스 클래스
    static class StaticInner {} // 스태틱 클래스

    void myMethod() {
        class LocalInner {} // 지역 클래스
    }
}
```

## 인스턴스 클래스

- 외부 클래스의 멤버변수(인스턴스변수와 클래스변수)와 같은 위치에 선언된다.
- 내부 클래스가 외부 클래스의 멤버와 같이 간주되기 때문에, <span style="color:red">외부 클래스의 객체 먼저 생성 후 내부 클래스의 객체를 생성</span>할 수 있다.
- 인스턴스 클래스 내부에 **인스턴스 멤버만 선언** 가능<span style="color:red">(static 멤버는 선언 불가)</span>
- 주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용될 목적 선언

```java
class Outer {
    // 인스턴스 변수
    int size = 100;
    int pay = 800;
    
    class InstanceInner {   // 인스턴스 내부 클래스 
        int iv = 100;
        String name = "아이스크림";

        static int cost = 10; // 에러, 인스턴스 내부 클래스에는 static 변수를 선언할 수 없다
        final static int cost = 10; // final static은 상수이므로 허용

        public void getIceShop(){
            // 외부 클래스 멤버 접근 가능
            System.out.println(size);
            System.out.println(pay);

            // 내부 클래스 멤버
            System.out.println(iv);
            System.out.println(name);
            System.out.println(cost);
        }
    }
}
```

```java
public class Main{
    public static void main(String[] args){

        Outer out = new Outer(); // 외부 클래스를 인스턴스화 함
        // 외부클래스.내부클래스 형식으로 내부클래스를 초기화 후 사용할 수 있음
        Outer.InstanceInner instancein = out.new InstanceInner();
        instancein.getIceShop();

        // 위의 단계를 한줄로 표현
        Outer.InstanceInner instancein2 = new Outer().new InstanceInner();
    }
}
```

내부 클래스는 다른 클래스에서 직접 사용하는 것보다 외부클래스에서만 사용하는 것이 일반적이므로 위의 예제같은 경우는 드물다.

### 이름이 동일할 경우 

내부 클래스와 외부 클래스에 선언된 변수의 이름이 같을 경우, 변수 앞에 `this` 또는 `클래스명.this`를 붙여서 구분한다.
클래스가 상속일 경우 `super`키워드를 통해 부모 메서드를 호출할 수 있지만, 외부-내부 클래스 관계일 경우 상속관계가 아니기 때문에 `this`를 사용함으로 <span style="color:blue">외부 인스턴스의 메서드를 호출하거나 외부 인스턴스의 참조를 가져올 수 있다.</span>

```java
// 외부 클래스
public class Main {

    public void print(String txt) {
        System.out.println(txt);
    }

	// 내부 클래스
    class Sub {
        public void print() {       
            Main.this.print("외부 클래스 메소드 호출");
            System.out.println("내부 클래스 메소드 호출");  
        }
    }
}

------------------------------------------------------

public static void main(String[] args) {
    Main.Sub s = new Main().new Sub();
    s.print();
    /*      
    외부 클래스 메소드 호출
    내부 클래스 메소드 호출
    */
}
```

### 인스턴스 클래스 컴파일

컴파일 시 생성되는 파일명은 `외부 클래스명$내부 클래스명.class` 형식이다. 

## 스태틱 클래스

- `static`키워드가 붙은 내부 클래스
- static 클래스 내부에서 **인스턴스 멤버**와 **static 멤버 모두 선언** 가능(일반적인 static메서드와 동일하게 **외부 클래스의 인스턴스 멤버에는 접근 불가**)
- static멤버가 인스턴스멤버를 직접 호출할 수 없는 것처럼, static 클래스도 <span style="color:red">외부 클래스의 인스턴스멤버를 객체생성 없이 사용할 수 없다.</span>

```java
class Outer{
    int size = 100;
    static int pay = 800;

    // static 내부 클래스
    static class InstanceInner {
        static int iv = 100;
        String name = "아이스크림";

        public static void getIceShop() {
            System.out.println(size);// 외부 클래스 인스턴스 멤버 접근 불가능

            System.out.println(pay); // 외부 클래스 static 멤버 접근 가능

            System.out.println(iv); // 내부 클래스 멤버도 static 멤버 접근 가능
            System.out.println(name); // 접근 불가능
        }
    }
}
```

```java
public class Main{
    public static void main(String[] args){
        // static 내부 클래스의 인스턴스는 외부 클래스를 먼저 생성하지 않아도 된다.
        Outer.InstanceInner instancein = new Outer().InstanceInner();
        System.out.println(instancein.name);
        System.out.printlnp(Outer.InstanceInner.iv);

        // 클래스.정적내부클래스.정적메서드()
        Outer.InstanceInner.getIceShop();
    }
}

```

static 클래스는 `static`이라 해서 한번만 로드되는 객체 개념이 아닌, 내부 클래스의 인스턴스를 바로 생성할 수 있다는 것이다. 

### static 클래스 컴파일

인스턴스 내부 클래스와 동일하다. 

## 지역 클래스

- **메서드 내부에 위치**하는 클래스 
- 지역 변수처럼 해당 메서드 내부에서만 한정적으로 쓰인다.
- <span style="color:red">접근제한자</span>와 <span style="color:red">static</span>을 붙일 수 없다.

```java
class Outer{
    int size = 100;
    int pay = 800;

    public void iceMethod() {
        int count = 5;

        // 메서드 내에 클래스 선언
        class InstanceInner {
            int iv = 100;
            String name = "아이스크림";

            public void getIceShop(){
                System.out.println(iv); // 인스턴스 변수 출력
                System.out.println(count); // 메서드 지역 상수 출력
            }
        }

        // 메서드 내 클래스 선언
        class InstanceInner2 {
            int iv = 200;
            String name ="팥빙수";
        }

        new InstanceInner().getIceShop();
        System.out.println("메서드 실행 완료");
    }
}

```

지역 클래스는 메서드 내 인스턴스를 생성 후 사용하고 메서드 종류와 함께 사라진다.

### 지역 클래스 지역 상수 접근

메서드 내 지역 클래스에서 지역 변수에 접근해서 사용하려고 할때 반드시 <span style="color:red">final 상수화 된 지역변수만 사용 가능</span>하다.

### 지역 클래스 컴파일

- 컴파일시 생성되는 클래스 파일명 : `외부 클래스명$내부 클래스명.class`

## 익명 클래스

- 클래스의 선언과 객체 생성을 동시에 한다.
- 생성자를 가질 수 없다.
- 단 하나의 객체를 생성하는 일회용 클래스이다.

```java
new 상위부모클래스명() {
    // 멤버 선언
}

또는 

new 구현인터페이스명() {
    // 멤버 선언
}
```

```java
class Inner {
    Object iv = new Object(){   // 익명 클래스
        void method() {}
    };

    static Object cv = new Object(){    // 익명 클래스
        void method() {}
    };

    void myMethod() {
        Object lv = new Object(){   // 익명 클래스
            void method() {}
        }
    };
}

```

### 익명 클래스 컴파일

- 컴파일시 생성되는 클래스명 : `외부 클래스명$숫자.class`
- 익명클래스는 이름이 없기 때문에 숫자로 구분한다.


## 참조

- 자바의 정석
- [참조한 내부 클래스에 대한 블로그](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%82%B4%EB%B6%80-%ED%81%B4%EB%9E%98%EC%8A%A4Inner-Class-%EC%9E%A5%EC%A0%90-%EC%A2%85%EB%A5%98)