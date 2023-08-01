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

static(정적) 멤버들은 static 영역에 생성된다. static멤버변수(클래스변수)는 <span style="color:red">하나의 변수를 모든 인스턴스가 공유</span>하기 때문에 인스턴스에 관계없이 같은 값을 갖게 된다.

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

| 제어자 | 대상                 | 의미                                                                                                                                                  |
| ------ | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| final  | 클래스               | 변경될 수 없는 클래스. 확장될 수 없는 클래스가 된다.<br>그래서 final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.                               |
|        | 메서드               | 변경될 수 없는 메서드. <br>final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.<br> 자신이 만든 메서드에 변경할 수 없게 끔 하고 싶을 때 사용. |
|        | 멤버변수<br>지역변수 | 변수 앞에 final이 붙으면 값을 변경할 수 없는 상수가 된다.                                                                                             |
|        | 메서드 인자값        | 인자값 변경이 불가능하다(잘 사용하지는 않는다)                                                                                                        |

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

**1.** final이 붙은 변수는 상수이므로 **선언과 초기화를 동시**에 하지만, **인스턴스변수의 경우 생성자에게 초기화** 되도록 할 수 있다.

**2.** 클래스 내에 매개변수를 갖는 생성자를 선언하여, 인스턴스를 생성할 때 final이 붙은 멤버변수를 초기화하는데 필요한 값을 **생성자의 매개변수로 제공**받는 것이다.


인스턴스마다 final이 붙은 멤버변수가 다른 값을 갖도록 하는 것이 가능하다.
생성자는 final 필드의 최종 초기화를 마쳐야 하는데 만약 초기화가 되지 않은 final 필드가 있다면 컴파일 에러가 발생한다.

```
사실 잘 모르겠다.😱
```

### abstract

`abstract`는 **메서드의 선언부만 작성**하고 **실제 수행내용은 구현하지 않은** 추상 메서드를 선언하는데 사용한다. 자세한 내용은 뒤에서 배운다.

```
abstract가 사용될 수 있는 곳 : 클래스, 메서드
```

추상 클래스는 인스턴스를 생성할 수 없다.

```java
abstract class AbstractTest {   // 추상클래스(추상 메서드를 포함한 클래스)
    abstract void move();   // 추상 메서드(구현부가 없는 메서드)
}
```


### 접근 제어자(access modifier)

접근 제어자는 멤버 또는 클래스에 사용되어, 해당하는 멤버 또는 클래스를 외부에서 접근하지 못하도록 제한하는 역할을 한다. 

```
접근 제어자가 사용될 수 있는 곳 : 클래스, 멤버변수, 메서드, 생성자

private : 같은 클래스 내에서만 접근 가능하다.
default : 같은 패키지 내에서만 접근이 가능하다.
protected : 같은 패키지 내에서, 그리고 다른 패키지의 자식클래스에서 접근이 가능하다.
public : 접근 제한이 없다.
```

|구분|public|protected|default|private|
|---|:---:|:---:|:---:|:---:|
|동일한 클래스| o | o | o | o |
|동일한 패키지 + 상속| o | o | o | x |
|동일한 패키지| o | o | o | x |
|다른 패키지 + 상속| o | o | x | x |
|다른 패키지 | o | x | x | x |

```
public > protected > (default) > private
```

#### public

- 접근 제한이 전혀 없기 때문에 프로그램 어디에서나 접근 가능하다.

```java
public class Everywhere {
    public Stirng var = "누구든지 허용"; // public 필드
    punlic String getVar() {    // public 메서드
        return this.var;
    }
}
```

#### privated

- 같은 클래스 내에서만 사용하도록 제한하는 가장 높은 제한이다.
- 자바에서 권장되는 제어 기법은 **클래스 멤버 변수들은 `private`를 지정**해서 접근을 제한하고, **메서드를 `public`으로 지정**한다.
- `Getter/Setter`을 구현하는데 자주 이용된다.


#### default

- 같은 패키지내의 클래스에서만 접근이 가능하다.
- 따로 접근 제어자를 명시하지 않을 경우 기본값으로 적용된다.

```java
package house;  // 패키지가 동일하지 않다.

public class HouseKim {
    String lastname = "kim";  
    // lastname은 다른 패키지에서 상속하든 뭘하든 접근이 절대 불가능 하다
}
```
```java
package truck;  // 패키지가 동일하지 않다.

import house.HouseKim; // 다른 패키제 있는 클래스를 사용하기 위해 불러옴

public class TruckPark {
    public static void main(String[] args) {
        HouseKim kim = new HouseKim(); // import 했으니 선언은 문제 없음
        System.out.println(kim.lastname);  
        // ERROR - 그러나 HouseKim 클래스의 lastname 변수를 사용할 수 없다
    }
}
```

#### protected

- 다른 패키지에는 접근 불가하지만, 상속할 경우 접근이 가능하다.
- 클래스의 protected멤버에 접근할 수 있는 영역
    * 이 멤버를 선언한 **클래스 멤버**
    * 이 멤버를 선언한 클래스가 **속한 같은 패키지 멤버**
    * 이 멤버를 선언한 클래스를 **상속받은 자식 클래스의 멤버**

```java
package house;  // 패키지가 동일하지 않다.

public class HouseKim {
    protected String lastname = "kim";  
    // lastname은 누군가 HouseKim을 상속하면 접근이 가능하다
}
```

```java
package truck;  // 패키지가 동일하지 않다.

import house.HouseKim; // 다른 패키제 있는 클래스를 사용하기 위해 불러옴

public class TruckPark extends HouseKim {
    public static void main(String[] args) {
        TruckPark park = new TruckPark();
        System.out.println(park.lastname);  // kim
    }
}
```

|대상| 사용가능한 접근 제어자 |
|--- | :---: |
| 클래스 | public, (default)|
| 메서드 | public, protected, (default), private |
| 멤버변수 | |
| 지역변수 | 없음 |

#### 접근 제어자를 이용한 캡슐화

접근 제어자를 사용하는 이유는 클래스의 내부에 선언된 데이터를 보호하기 위해서이다.

```
- 외부로부터 데이터를 보호하기 위해
- 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해서
```

**데이터 감추기/정보 은닉(data hiding)** 사용자가 알 필요없는 정보는 사용자로부터 숨겨야 한다는 개념

외부에서 접근할 필요가 없는 멤버들은 `private`으로 지정하여 외부에 노출시키지 않음으로써 복잡성을 줄일 수 있다. 이것을 **은닉화/캡슐화**라고 한다.

```java
public class Time {
    private int hour; // 외부에서 직접 접근하지 않는다. 
    private int minute;
    private int second;

    // get/set
    public int getHour() { return hour; } // getter 메서드
    public void setHour(int hour){   // setter 메서드
        if(hour < 0 || hour > 23) return;
        this.hour = hour;
    }
    public int getMinute() { return minute; }
    public void setMinute(int minute){
        if(minute < 0 || minute > 59) return;
        this.minute = minute;
    }
    public int getSecond() { return second; }
    public void setSecond(int second){
        if(second < 0 || second > 59) return;
        this.second = second;
    }
}
```

`get`으로 시작하는 메서드는 단순히 <span style="color:red">멤버변수의 값을 반환하는 일</span>을 하고, `set`으로 시작하는 메서드는 <span style="color:red">매게변수에 지정된 값을 검사하여 조건에 맞는 값일 때만 멤버변수의 값을 변경</span>하도록 작성한다. 

멤버변수의 값을 읽는 메서드의 이름을 `get멤버변수이름`으로 하고, 멤버변수의 값을 변경하는 메서드의 이름을 `set멤버변수이름`으로 한다.
- get으로 시작하는 메서드를 `getter`
- set으로 시작하는 메서드를 `setter`

#### 생성자의 접근 제어자

생성자에 접근 제어자를 사용함으로써 **인스턴스 생성을 제한**할 수 있다.
생성자의 접근 제어자를 `private`으로 지정하면, 외부에서 생성자에 접근할 수 없으므로 **인스턴스를 생성할 수 없게** 된다. 그래도 클래스 내부에서 **인스턴스를 생성**할 수 있다.

```java
class Singleton {
    private Singleton(){
        ...
    }
    ...
}
```

인스턴스를 생성해서 반환해주는 `public메서드`를 제공함으로써 외부에서 이 클래스의 **인스턴스를 사용**할 수 있다. 
`public`인 동시에 `static`이어야 한다.

```java
class Singleton{
    ...
    private static Singleton s = new Singleton();
    // getInstance()에서 사용될 수 있도록 인스턴스가 미리 생성되어야 하므로 static이어야 한다.
    private Singleton() {
        ...
    }
    // 인스턴스를 생성하지 않고도 호출할 수 있어야 하므로 static이어야 한다.
    public static Singleton getInstance(){
        return s;
    }
    ...
}
```

생성자를 통해 직접 인스턴스를 생성하지 못하고  `public메서드`를 통해 인스턴스에 접근하게 함으로써 사용할 수 있는 **인스턴스의 개수를 제한**할 수 있다.

생성자가 `private`인 클래스는 다른 클래스의 부모가 될 수 없다. 자식클래스의 인스턴스를 생성할 때 부모클래스의 생성자를 호출해야 하는데, 생성자의 접근 제어자가 `private`이므로 자식클래스에서 호출하는 것이 불가능하다.
클래스 앞에 `final`을 추가하여 상속할 수 없는 클래스라는 걸 알리는게 좋다.

`Math클래스`의 몇 개의 상수와 `static메서드`만으로 구성되어 있기에 인스턴스를 생성할 필요가 없다. 

```java
public final class Math{
    private Math(){
        ...
    }
}
```

### 제어자의 조합

| 대상 | 사용가능한 제어자 |
| --- | --- |
| 클래스 | public, (default), final, abstract |
| 메서드 | 모든 접근 제어자, final, abstract, static |
| 멤버변수 | 모든 접근 제어자, final, static |
| 지역변수 | final |

#### 제어자를 조합해서 사용할 떄 주의해야 할 사항

**1. 메서드에 static과 abstract를 함께 사용할 수 없다.**

static메서드는 몸통이 있는 메서드에만 사용할 수 있기 때문

**2. 클래스에 abstract와 final을 동시에 사용할 수 없다.**

클래스에 사용되는 final은 클래스를 확장할 수 없다는 의미이고 abstract는 상속을 통해서 완성되어야 한다는 의미이므로 서로 모순된다.

**3. abstract메서드의 접근 제어자가 private일 수 없다.**

abstract메서드는 자식클래스에서 구현해줘야 하는데 접근 제어자가 private이면, **자식클래스에서 접근할 수 없기** 때문이다.

**4. 매서드에 private과 final을 같이 사용할 필요는 없다.**

접근 제어자가 `private`인 메서드는 오버라이딩될 수 없기 때문이다. 둘 중 하나만 써도 충분하다. 


## 참조

- 자바의 정석
- [코드 예시 출처](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5OOP-%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%AC%B8%EB%B2%95-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#%EA%B8%B0%ED%83%80_%EC%A0%9C%EC%96%B4%EC%9E%90)