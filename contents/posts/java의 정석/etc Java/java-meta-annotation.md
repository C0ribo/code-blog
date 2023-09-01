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


## 참고

- 자바의 정석