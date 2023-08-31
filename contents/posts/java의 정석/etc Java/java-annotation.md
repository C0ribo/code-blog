---
title: "[Java] 어노테이션(annotation)"
description: "annotation, @Override, @Deprecatedd, @FunctionalInterface, @SuppressWarnings, @SafeVarargs"
date: 2023-08-31
update: 2023-08-31
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## Annotation

annotation은 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것이다.

| 어노테이션 | 설명 |
| --- | --- |
| `@Override` | 컴파일러에게 오버라이딩하는 메서드라는 것을 알린다 |
| `@Deprecated` | 앞으로 사용하지 않을 것을 권장하는 대상에 붙인다 |
| `@SuppressWarnings` | 컴파일러의 특정 경고메시지가 나타나지 않게 한다 |
| `@SafeVarargs` | 제네릭스 타입의 가변인자에 사용한다. | 
| `@FunctionalInterface` | 함수형 인터페이스라는 것을 알린다. |
| `@Native` | native메서드에서 참조되는 상수 앞에 붙인다 |
| `@Target` | 어노테이션이 적용가능한 대상을 지정하는데 사용 |
| `@Documented` | 어노테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다 |
| `@Inherited` | 어노테이션이 자손 클래스에 상속되도록 한다 | 
| `@Retension` | 어노테이션이 유지되는 범위를 지정하는데 사용한다. |
| `@Repeatable` | 어노테이션을 반복해서 적용할 수 있게 한다. |

### @Override

메서드 앞에서만 붙일 수 있는 어노테이션으로, 부모의 메서드를 오버라이딩하는 것이라는 걸 컴파일러에게 알려주는 역할이다. 오버라이딩할 때 부모 메서드의 이름을 잘못 써도 컴파일러는 잘못된 것인지 알지 못한다.

```java
class Parent {
    void parentMethod() { ... }
}

class Child extends Parent {
    @Override
    void parentMethod() { ... }
}
```

위 코드처럼 메서드 앞에 `@Override`라고 붙이면, 컴파일러가 같은 이름의 메서드가 부모에 있는지 확인하고 없으면 에러메시지를 출력한다.

### @Deprecated

더 이상 사용되지 않은 필드나 메서드 그리고 타입에 붙여서 쓰는 것이다. 주로 삭제 예정인 기능이거나 대체 가능한 새로운 기능이나 메서드가 도입되었을 때 함부로 삭제할 수는 없고 새 대안 사용 유도 목적으로 사용한다. 

`@Deprecated`가 붙은 필드, 메서드, 타입은 사용하지 않는다. 만약 사용하게 된다면 컴파일시 아래와 같은 메시지가 나타나기 때문이다.

```java
Note: AnnotationEx2.java uses or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
```

다만, `@Deprecated`가 붙은 대상을 사용했을 경우, 강제성은 없기에 컴파일은 실행된다.


### @FunctionalInterface

**함수형 인터페이스**를 선언할 때, 이 어노테이션을 붙이면 컴파일러가 **함수형 인터페이스**를 올바르게 선언했는지 확인하고, 잘못된 경우 에러를 발생시킨다. 함수형 인터페이스를 선언할 때 필수는 아니지만, 꼭 어노테이션(`@FunctionalInterface`)을 붙이도록 하자.

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run(); 
}
```

> 함수형 인터페이스는 하나의 ***추상 메서드***만 갖는 인터페이스이다.


### @SuppressWarnings

컴파일러가 보여주는 경고메시지가 나타나지 않게 억제해주는 역할이다. 위에서 배운 `@Deprecated`를 사용할 때 나오는 경고메시지를 매 컴파일마다 뜰 수 없으므로 `@SuppressWarnings`을 사용함으로써 나타나지 않게 해준다.

경고 메시지의 종류는 JDK 버전이 올라가면서 계속 추가되는데 주로 사용되는 것은 `deprecation`, `unchecked`, `rawtypes`, `varargs` 등이 있다.
자세한 것은 [여기서](https://www.ibm.com/docs/ko/radfws/9.6.1?topic=code-excluding-warnings) 볼 수 있다.

- **deprecation** : `@Deprecated`가 붙은 대상을 사용해서 발생하는 경고를 억제할 때 사용
- **unchecked** : 제네릭스로 타입을 지정하지 않을 때 발생하는 경고를 억제할 때 사용
- **rawtypes** : 제네릭스를 사용하지 않아서 발생하는 경고를 억제할 때 사용
- **varargs** : 가변인자의 타입이 제네릭 타입일 때 발생하는 경고를 억제할 때 사용

억제하려는 경고 메시지를 어노테이션 뒤에 괄호`()`안에 문자열로 지정한다.

```java
@SuppressWarnings("unchecked")      // 제네릭스와 관련된 경고를 억제
ArrayList list = new ArrayList();   // 제네릭 타입을 지정하지 않았다.
list.add(obj);                      // 경고 발생
```

둘 이상의 경고를 동시에 억제하려면 배열처럼 괄호`{}`를 추가로 사용한다.

```java
@SuppressWarnings({"deprecation", "unchecked", "varargs"})
```

> `-Xlint`옵션으로 컴파일해서 나타나는 경고의 내용 중 대괄호`[]` 안에 있는 것이 메시지 종류이다.


### @SafeVarargs

메서드에 선언된 가변인자의 타입이 **non-reifiable**타입일 경우, 해당 메서드를 선언하는 부분과 호출하는 부분에서 `unchecked` 경고가 발생한다. 이 경고를 억제하기 위해 사용한다. 이 어노테이션은 `static`이나 `final`이 붙은 `메서드`와 `생성자`에만 사용할 수 있다.

- **reifiable타입** : 컴파일 후 제거되지 않은 타입
- **non-reifiable타입** : 컴파일 후 제거되는 타입

제너릭 타입들은 컴파일 시 제거되므로 **non-reifiale타입**이다.

```java
@SafeVarargs                // unchecked 경고를 억제한다
@SuppressWarnings("varargs")// varargs 경고를 억제한다
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

`@SafeVarargs`를 붙이면, 이 메서드를 호출하는 곳에서 발생하는 경고도 억제된다. `@SafeVarargs` 대신 `@SuppressWarnings("unchecked")`로 경고를 억제하려면, 메서드 선언뿐만 아니라 메서드가 호출되는 곳에도 어노테이션을 붙여야한다.

`@SafeVarargs`로 `unchecked`경고는 억제할 수 있지만, `varargs`경고는 억제할 수 없기 때문에 `@SafeVarargs`와 `@SuppressWarnings("varargs")`를 같이 붙인다.

## 참고

- 자바의 정석