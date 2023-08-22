---
title: "[Java] 컬렉션 프레임워크의 핵심 인터페이스"
description: "collection, List, Set, Map"
date: 2023-08-22
update: 2023-08-22
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 컬렉션 프레임워크(Collections Framework)

배열의 비효율적인 문제로 인해, 해결하기 위해 자료구조를 바탕으로 객체나 데이터들을 효율적으로 관리할 수 있도록 만든 라이브러리를 말한다. 대표적으로 List, Map, Set 등이 있다.

## 컬렉션 프레임워크의 핵심 인터페이스

![Collection.png](https://github.com/C0ribo/Study_JavaScript/assets/133131980/94ac38d0-3d67-4bb7-b0fb-1779c364ba05)

인터페이스 List와 Set을 구현한 컬렉션 클래스들은 서로 공통부분이 많아, 공통 부분끼리 모아 Collection인터페이스를 정의했지만 Map인터페이스는 이 둘과 다른 형태로 다루기 때문에 상속계층도에 포함되지 못한다.

| 인터페이스 | 특징 |
| --- | --- |
| **List** | 순서가 있는 데이터의 집합. 데이터의 중복을 허용한다. <br>예) 대기자 명단 |
|  | 구현 클래스 : ArrayList, LinkedList, Stack, Vector 등 |
| **Set** | 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않는다.<br>예) 양의 정수집합, 소수의 집합 |
|  | 구현 클래스 : HashSet, TreeSet 등 |
| **Map** | 키(key)와 값(value)의 쌍(pair)으로 이루어진 데이터의 집합<br>순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.<br>예) 우편번호, 지역번호(전화번호) |
|  | 구현 클래스 : HashMap, TreeMap, Hashtable, Properties 등 |

> 키(key)란, 데이터 집합 중에서 어떤 값(value)을 찾는데 열쇠(key)가 된다는 의미에서 붙여진 이름으로 중복을 허용하지 않는다.
> 
- 컬렉션 프레임워크는 모든 컬렉션 클래스들은 List, Set, Map 중의 하나를 구현하고 있으며, 구현한 인터페이스의 이름이 클래스의 이름에 포함되어있어서 이름만으로 클래스의 특징을 쉽게 알 수 있다.
- Vector, Stack, Hashtable, Properties와 같은 클래스들은 컬렉션 프레임워크가 만들어지기 이전에 존재하기 때문에 명명법을 따르지 않는다.
    - Vector나 Hashtable과 같은 기본의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 사용하지 않는것이 좋다. 차라리 `ArrayList`와 `HashMap`을 사용하자.

### Collection인터페이스

| 메서드 | 설명 |
| --- | --- |
| `boolean add(Object o)`<br>`boolean addAll(Collection c)` | 지정된 객체(o) 또는 Collection(c)의 객체들을 Collection에 추가한다. |
| `void clear()` | Collection의 모든 객체를 삭제 |
| `boolean contains(Object o)`<br>`boolean containsAll(Collection c)` | 지정된 객체(o) 또는 Collection(c)의 객체들이 Collection에 포함되어 있는지 확인 |
| `boolean equals(Object o)` | 동일한 Collection인지 비교 |
| `int hashCode()` | Collection의 hash Code를 반환 |
| `boolean isEmpty()` | Collection이 비어있는지 확인 |
| `Iterator iterator()` | Collection의 Iterator를 얻어서 반환 |
| `boolean remove(Object o)` | 지정된 객체를 삭제 |
| `boolean removeAll(Collection c)` | 지정된 Collection에 포함된 객체들을 삭제 |
| `boolean retainAll(Collection c)` | 지정된 Collection에 포함된 객체만을 남기고 다른 객체들은 Collection에서 삭제한다. 이 작업으로 인해 Collection에 변화가 있으면 true, 그렇지 않으면 false를 반환 |
| `int size()` | Collection에 저장된 객체의 개수를 반환 |
| `Object[] toArray()` | Collection에 저장된 객체를 객체배열(Object[])로 반환 |
| `Object[] toArray(Object[] a)` | 지정된 배열에 Collection의 객체를 저장해서 반환 |
- Collection인터페이스는 컬렉션 클래스에 저장된 데이터를 읽고, 추가하고 삭제하는 등 컬렉션을 다루는데 가장 기본적인 메서드를 정의한다.
- 반환 타입이 `boolean`인 메서드의 경우 작업을 성공하면 true, 그렇지 않으면 false를 반환한다.

### List인터페이스

List인터페이스는 **중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현**하는데 사용된다.

| 메서드 | 설명 |
| --- | --- |
| `void add(int index, Object element)`<br>`boolean add(int index, Collection c)` | 지정된 위치(index)에 객체(element) 또는 컬렉션에 포함된 객체들을 추가한다. |
| `Object get(int index)` | 지정된 위치(index)에 있는 객체를 반환한다. |
| `int indexOf(Object o)` | 지정된 객체의 위치(index)를 반환한다.<br>(List의 첫 번째 요소부터 순방향으로 찾는다.) |
| `int lastIndexOf(Object o)` | 지정된 객체의 위치(index)를 반환한다.<br>(List의 마지막 요소로부터 역방향을 찾는다.) |
| `ListIterator listIterator()`<br>`ListIterator listIterator(int index)` | List의 객체에 접근할 수 있는 ListIterator를 반환한다. |
| `Object remove(int index)` | 지정된 위치(index)에 있는 객체를 삭제하고 삭제된 객체를 반환한다. |
| `Object set(int index, Object element)` | 지정된 위치(index)에 객체(element)를 저장한다. |
| `void sort(Comparator c)` | 지정된 비교자(comparator)로 List를 정렬한다 |
| `List subList(int fromIndex, int toIndex)` | 지정된 범위(fromIndex부터 toIndex)에 있는 객체를 반환한다. |

### Set인터페이스

Set인터페이스는 **중복을 허용하지 않고 저장순서가 유지되지 않은 컬렉션 클래스를 구현**하는데 사용된다. 

Set인터페이스를 구현한 클래스로는 `HashSet`, `TreeSet` 등이 있다.

| 메서드  | 설명 |
| --- | --- |
| `boolean add(Object o)` | 지정된 객체(o)의 객체들을 Collection에 추가한다. |
| `boolean contains(Object o)` | 지정된 객체(o)의 객체들이 Collection에 포함되어 있는지 확인한다. |
| `Iterator iterator()` | Collection의 Iterator를 얻어서 반환한다. |
| `boolean isEmpty()` | Collection이 비어있는지 확인한다. |
| `int size()` | Collection에 저장된 객체의 개수를 반환한다. |
| `void clear()` | Collection의 모든 객체를 삭제한다. |
| `boolean remove(Object o)` | 지정된 객체를 삭제 |

### Map인터페이스

- Map인터페이스는 **키(key)와 값(value)을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현**하는데 사용된다.
- **키는 중복될 수 없**지만 **값은 중복을 허용**    한다.
- 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게 된다.
-   `Hashtable`, `HashMap`, `LinkedHashMap`, `SortedMap`, `TreeMap` 등

> Map이란, 어떤 두 값을 연결한다는 의미로 붙여졌다.
> 

| 메서드 | 설명 |
| --- | --- |
| `void clear()` | Map의 모든 객체를 삭제한다. |
| `boolean containsKey(Object key)` | 지정된 key객체와 일치하는 Map의 key객체가 있는지 확인한다. |
| `boolean containsValue(Object value)` | 지정된 value객체와 일치하는 Map의 value객체가 있는지 확인한다. |
| `Set entrySet()` | Map에 저장되어 있는 key-value쌍을 Map.Entry타입의 객체로 저장한 Set으로 반환한다. |
| `boolean equals(Object o)` | 동일한 Map인지 비교한다. |
| `Object get(Object key)` | 지정된 key객체에 대응하는 value객체를 찾아서 반환한다.  |
| `int hashCode()` | 해시코드를 반환한다. |
| `boolean isEmpty()` | Map이 비어있는지 확인한다. |
| `Set keySet()` | Map에 저장된 모든 key객체를 반환한다. |
| `Object put(Object key, Object value)` | Map에 value객체를 key객체에 연걸(mapping)하여 저장한다. |
| `void putAll(Map t)` | 지정된 Map의 모든 key-value쌍을 추가한다. |
| `Object remove(Object key)` | 지정된 key객체와 일치하는 key-value객체를 삭제한다. |
| `int size()` | Map에 저장된 key-value쌍의 개수를 반환한다. |
| `Collection values()` | Map에 저장된 모든 value객체를 반환한다. |
-  `values()`에서는 **반환타입이 Collection**이고, `keySet()`에서는 **반환타입이 Set**이다.
- Map인터페이스에서 **값(value)은 중복을 허용**하기 때문에 `Collection타입`으로 반환하고, **키(key)는 중복을 허용하지 않기** 때문에 `Set타입`으로 반환한다.

### Map.Entry인터페이스

Map.Entry인터페이스는 Map인터페이스의 내부 인터페이스이다.

- 내부 클래스와 같이 인터페이스도 인터페이스 안에 인터페이스를 정의하는 내부 인터페이스(inner interface)를 정의하는 것이 가능하다.
- Map에 저장되는 key-value쌍을 다루기 위해 내부적으로 Entry인터페이스를 정의해놓는다.
    - Map인터페이스를 구현하는 클래스에서는 Map.Entry인터페이스도 함께 구현해야 한다.

```java
public interface Map{
	...
	public static interface Entry {
			Object getKey();
			Object getValue();
			Object setValue(Object value);
			boolean equals(Object o);
			int hashCode();
			...
	}
}
```

| 메서드 | 설명 |
| --- | --- |
| boolean equals(Object o) | 동일한 Entry인지 비교한다. |
| Object getKey() | Entry의 key객체를 반환한다. |
| Object getValue() | Entry의 value객체를 반환한다. |
| int hashCode() | Entry의 해시코드를 반환한다. |
| Object setValue(Object value) | Entry의 value객체를 지정된 객체로 바꾼다. |


## 참조

- 자바의 정석