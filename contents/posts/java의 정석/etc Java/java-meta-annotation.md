---
title: "[Java] 메타 어노테이션(meta annotation)"
description: "meta-annotation, @Target, @Retention, @Documented, @Inherited, @Repeatable, @Native"
date: 2023-09-01
update: 2023-09-01
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 메타 어노테이션

어노테이션을 정의할 때 어노테이션의 적용대상(target)이나 유지기간(retention)등을 지정하는데 사용한다. 메타 어노테이션은 `java.lang.annotation` 패키지에 포함되어 있다.

### @Target

어노테이션이 적용가능한 대상을 지정하는데 사용된다.

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.source)
public @interface SuppressWarnings {
    String[] value();
}
```

여러 개의 값을 지정할 때는 배열처럼 괄호{}를 사용한다.

#### 어노테이션 적용대상 종류

| 대상 타입 | 의미 |
| --- | --- |
| ANNOTATION_TYPE | 어노테이션 |
| CONSTRUCTOR|생성자 |
| FIELD | 필드(멤버변수, enum상수) |
| LOCAL_VARIABLE | 지역변수 |
| METHOD | 메서드 |
| PACKAGE | 패키지 |
| PARAMETER | 매개변수 | 
| TYPE | 타입(클래스, 인터페이스, enum) <br>타입을 선언할 때 어노테이션을 붙일 수 있다는 뜻 |
| TYPE_PARAMETER | 타입 매개변수 |
| TYPE_USE | 타입이 사용되는 모든 곳<br>해당 타입의 변수를 선언할 때 붙일 수 있다는 뜻 |

위 표의 값들은 `java.lang.annotation.ElementType`이라는 열거형에 정의되어 있고, `static import문`을 쓰면 `ElementType.TYPE`을 `TYPE`과 같이 간단하게 할 수 있다.

```java
import static java.lang.annotation.ElementType.*;

@Target({FIELD, TYPE, TYPE_USE})    // 적용 대상 : FIELD, TYPE, TYPE_USE
public @interface MyAnnotation { }  // MyAnnotation 정의

@MyAnnotation       // 적용 대상 : TYPE인 경우
class MyClass {
    @MyAnnotation   // 적용 대상 : FIELD인 경우
    int i;

    @MyAnnotation   // 적용 대상 : TYPE_USE인 경우
    MyClass mc;
}
```

### @Retention

어노테이션이 유지되는 기간을 지정하는데 사용된다. 

#### 어노테이션 유지정책의 종류

| 유지 정책 | 의미 |
| ---  | --- |
| SOURCE | 소스 파일에만 존재. 클래스 파일에는 존재하지 않는다. |
| CLASS | 클래스 파일에 존재. 실행 시에는 사용 불가. 기본값 |
| RUNTIME | 클래스 파일에 존재. 실행 시에는 사용 가능 |

**SOURCE**는 `@Override`나 `@SuppressWarnings`처럼 컴파일러가 사용되는 어노테이션이다. 컴파일 시에만 어노테이션 정보 유지하고, 컴파일된 클래스 파일에는 포함되지 않는다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override { }
```

**CLASS**는 컴파일러가 어노테이션의 정보를 클래스 파일에 저장할 수 있게 하지만, 런타임 시에는 사용하지 않는다.

**RUNTIME**는 런타임 실행 시, **리플렉션(reflection)**을 통해 클래스 파일에 저장된 어노테이션의 정보를 읽어서 처리할 수 있다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface { }
```

`@FunctionalInterface`는 `@Override`처럼 컴파일러가 체크해주는 어노테이션이지만, 실행 시에도 사용되므로 유지 정책이 `RUNTIME`으로 되어 있다.


### @Documented

사용자 정의에 어노테이션의 정보가 **자동으로 API문서에 표시**되도록 해주는 역할이다. 문서화할 때 유용하게 활용된다. 

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface { }
```


### @Inherited

상위 클래스에서 사용한 어노테이션을 하위 클래스에 자동으로 상속되도록 지정하는 역할이다. `@Inherited`을 가진 어노테이션은 인터페이스에는 적용되지 않는다. 인터페이스에서 어노테이션을 선언하더라도 하위 클래스에 상속되지 않는다.

```java
@Inherited                      // @SupperAnno가 자식까지 영향 미치게
@interface SupperAnno { }

@SuperAnno
class Parent { }

class Child extends Parent { }  // Child에 어노테이션이 붙은 것으로 인식
```

Child클래스는 어노테이션이 붙지 않았지만, 부모인 Parent클래스에 붙은 `@SuperAnno`가 상속되어 Child클래스에도 `@SuperAnno`가 붙은 것처럼 인식한다.

### @Repeatable

@Repeatable이 붙은 어노테이션은 여러 번 붙이고 쓸 수 있다.

```java
@Repeatable(ToDos.class) // ToDo 어노테이션을 여러 번 반복해서 쓸 수 있다
@interface ToDo {
  String value();
}
```

위의 예제처럼 ToDo.class를 앞에 @Repeatable을 붙이므로써 똑같은 걸 반복적으로 사용할 수 있다.

```java
@ToDo("delete test codes")
@ToDo("override inherited methods")
class MyClass {
  ...
}
```

같은 이름의 어노테이션을 여러 개가 하나의 대상에 적용할 수 있기 떄문에, 하나로 묶어서 다룰 수 있는 어노테이션도 추가로 정의해야 한다.

```java
@interface ToDos {        // 여러 개의 ToDo어노테이션을 담을 컨테이너 어노테이션 ToDos
  ToDo[] value();         // ToDo어노테이션 배달타입의 요소 선언. 이름이 value여야 한다
}
@Repeatable(ToDos.class) // 괄호 안에 컨테이너 어노테이션을 지정해야한다
@interface ToDo {
  String value();
}
```

### @Native

네이티브 메서드(native method)에 의해 참조되는 상수 필드(constant field)에 붙이는 어노테이션이다. 네이티브 메서드는 JVM에 설치된 OS의 메서드를 말한다. 

```java
public class Object {
  private static native void registerNatives(); // 네이티브 메서드

  static {
    registerNatives(); // 네이티브 메서드
  }

  protected native Object clone() throws CloneNotSupportedException;
  public final native Class<?> getClass();
  public final native void notify();
  public final native void notifyAll();
  public final native void wait(long timeout) throws InterruptedException;
  public native int hashCode();
    ...
}
```

모든 클래스의 조상인 Object클래스의 메서드들은 대부분 네이티브 메서드이다.

## 참고

- 자바의 정석