---
title: "[Java] Iterator, ListIterator, Enumeration"
description: "Iterator, ListIterator, Enumeration"
date: 2023-08-26
update: 2023-08-26
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## Iterator, ListIterator, Enumeration

**Iterator**, **ListIterator**, **Enumeration** 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다. 컬렉션 프레임워크에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화했다.

### Iterator

컬렉션에 저장된 각 요소에 접근하는 기능을 가진 **Iterator인터페이스를 정의**하고, Collection인터페이스에는 Iterator(Iterator를 구현한 클래스의 인스턴스)를 반환하는 `iterator()`를 정의하고 있다. 

```java
public interface Iterator {
	boolean hasNext();
	Object next();
	void remove();
}
public interface Collection {
	...
	public Iterator iterator();
	...
}
```

- `iterator()`는 Collection인터페이스에 정의된 메서드이므로 Collection인터페이스의 자손인 `List`와 `Set`에도 포함
    - List나 Set인터페이스를 구현하는 컬렉션은 `iterator()`가 각 컬렉션의 특징에 맞게 작성
- `iterator()`를 호출하여 Iterator를 얻은 다음 반복문, 주로 **while문을 사용**해서 컬렉션 클래스의 요소들을 읽어올 수 있음

| 메서드 | 설명 |
| --- | --- |
| `boolean hasNext()` | 읽어올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환 |
| `Object next()` | 다음 요소를 읽어온다.<br> `next()`를 호출하기 전에 `hasNext()`를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다. |
|  `void remove()` | `next()`로 읽어 온 요소를 삭제한다.<br> `next()`를 호출한 다음에 `remove()`를 호출해야 한다.(선택적 가능) |

#### ArrayList에 저장된 요소를 출력

```java
Collection c = new ArrayList(); // 다른 컬렉션으로 변경 시 이부분만 고치면 된다.
Iterator it = c.iterator(); // Iterator를 이용하여 컬렉션 요소 읽어오는 방법을 표준화함
// 코드 재사용성 높임
while(it.hasNext()) {
	System.out.println(it.next());
}
```

- ArrayList대신 Collection인터페이스를 구현한 다른 컬렉션 클래스에 대해서도 **동일한 코드 사용** 가능
    - 첫 줄에서 ArrayList대신 Collection인터페이스를 구현한 다른 컬렉션 클래스의 객체를 생성하도록 변경하기만 하면 된다.

> **ArrayList 대신에 Collection을 사용하는 이유**<br>
ArrayList은 Collection인터페이스의 구현 클래스 중 하나이다. 그래서 ArrayList 대신에 **Collection 인터페이스를 사용**하는 이유는 **유연성, 다형성, 코드변경 용이성, 다른 자료 구조로의 대체 가능성을 향상**시키기 위해서이다.


#### Map인터페이스에서의 Iterator사용

- **Map인터페이스**를 구현한 컬렉션 클래스는 `키(key)`와 `값(value)`을 쌍(pair)으로 저장하기에 `iterator()`를 직접 호출할 수 없고, 대신에 `keySet()`이나 `entrySet()`메서드를 통해 키와 값을 각각 Set형태로 얻은 후, `iterator()`를 호출해야 Iterator를 얻을 수 있다.

```java
Map map = new HashMap();
...
Iterator it = map.entrySet().iterator(); // 아래의 코드들을 한 줄로 합친 것
// Set eSet = map.entrySet();
// Iterator it = eSet.iterator();
```

1. `map.entrySet()`의 실행결과가 Set이므로

```java
Iterator it = map.entrySet().iterator(); -> Iterator it = Set인스턴스.iterator();
```

2. `map.entrySet()`을 통해 얻은 Set인스턴스의 `iterator()`를 호출해서 Iterator인스턴스를 얻는다.

```java
Iterator it = Set인스턴스.iterator(); -> Iterator it = Iterator인스턴스;
```

3. 마지막으로 Iterator인스턴스의 참조가 `it`에 저장된다.

### ListIterator와 Enumeration

- **Enumeration** : 컬렉션 프레임워크가 만들어지기 이전에 사용하던 것으로 **Iterator의 구버전**이다.
    - 작성된 소스와의 호환을 위해 남겨두고 있을 뿐 가능하면 Iterator를 사용하자
- **ListIterator** : Iterator를 상속받아서 기능 추가, Iterator에 **양방향 조회 기능 추가**, List인터페이스(ArrayList, LinkedList)를 구현한 컬렉션에서만 사용 가능

#### 메서드

Enumeration과 Iterator는 메서드이름만 다를 뿐 기능은 같고, ListIterator는 Iterator에 이전방향으로의 접근기능추가

**Enumeration인터페이스의 메서드**

| 메서드 | 설명 |
| --- | --- |
| `boolean hasMoreElements()` | 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환. Iterator의 `hasNext()`와 같다. |
| `Object nextElement()` | 다음 요소를 읽어온다. `nextElement()`를 호출하기 전에 `hasMoreElements()`를 호출해서 읽어올 요소가 남아있는지 확인하는 것이 안전하다. Iterator의 `next()`와 같다. |

**ListIterator의 메서드**

| 메서드 | 설명 |
| --- | --- |
| `void add(Object o)` | 컬렉션에 새로운 객체(o)를 추가한다.(선택적 가능) |
| `boolean hasNext()` | 읽어 올 다음 요소가 남아있는지 확인한다. 있으면 true, 없으면 false반환 |
| `boolean hasPrevious()` | 읽어 올 이전 요소가 남아있는지 확인한다. 있으면 true, 없으면 false반환 |
| `Object next()` | 다음 요소를 읽어 온다. `next()`를 호출하기 전에 `hasNext()`를 호출해서 읽어올 요소가 있는지 확인하는 것이 안전하다. |
| `Object previous()` | 이전 요소를 읽어온다. `previous()`를 호풀하기 전에 `hasPrevious()`를 호출해서 읽어올 요소가 있는지 확인하는 것이 안전하다. |
| `int nextIndex()` | 다음 요소의 index를 반환한다. |
| `int previousIndex()` | 이전 요소의 index를 반환한다. |
| `void remove()` | `next()` 또는 `previous()`로 읽어 온 요소를 삭제한다. 반드시 `next()`나 `previous()`를 먼저 호출한 다음에 이 메서드를 호출해야 한다.(선택적 기능) |
| `void set(Object o)` | `next()` 또는 `previous()`로 읽어 온 요소를 지정된 객체(o)로 변경한다. 반드시 `next()`나 `previous()`를 먼저 호출한 다음에 이 메서드를 호출해야 한다.(선택적 기능) |

- 선택적 기능이라고 표시된 것들은 반드시 구현하지 않아도 된다.
    - 예를 들어, Iterator인터페이스를 구현하는 클래스에서 `remove()`는 선택적인 기능이므로 구현하지 않아도 된다.
    - 인터페이스로부터 상속받은 메서드는 `추상메서드`라 메서드의 `몸통(body)`을 반드시 만들어줘야 한다.

```java
public void remove() {
	throw new UnsupportedOperationException(); 
    // 예외를 던저 구현되지 않은 기능이라는 걸 알리는게 좋다.
}
```

- remove메서드를 지원하지 않은 Iterator는 `UnsupportedOperationException`을 발생
    - remove를 쓰지 않을 경우 예외가 발생하도록 구현하라는 것이다.(안쓴다고만 말하는것이다)
- 위 코드에서 remove메서드의 선언부에 예외처리를 하지 않은 이유 : `UnsupportedOperationException`이 `RuntimeException`의 자손이기 때문
- Iterator에서 `remove()`는 단독으로 쓰일 순 없고 `next()`와 같이 써야한다.
    - 특정위치의 요소를 삭제하는 것이 아니라 읽어 온 것을 삭제
    - `next()` 호출없이 `remove()` 호출 시, `IllegalStateException` 발생
- **서버를 읽어오기만 할 때** : `next()` 사용, 읽어 온 메일을 서버에 남기지 않고 지울 때 `next() + remove()` 사용

## 참고

- 자바의 정석