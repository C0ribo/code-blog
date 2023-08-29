---
title: "[Java] HashSet"
description: "HashSet"
date: 2023-08-28
update: 2023-08-28
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## HashSet

HashSet은 Set인터페이스를 구현한 대표적인 컬렉션이며, 중복된 요소를 저장하지 않는다. 

```java
HashSet set1 = new HashSet();
```

### HashSet 추가

새로운 요소를 추가할 때는 `add메서드`나 `addAll메서드`를 사용하는데, HashSet에 이미 저장되어 있는 요소와 중복된 요소를 추가하고자 하면 메서드들은 false를 반환하면서 중복된 요소로써 추가하는데 실패했다고 한다.

```java
HashSet set1 = new HashSet<Integer>();
set1.add(1); // 값 추가
```

### HashSet 삭제

HashSet에 값을 제거하려면 `remove(value)`메서드를 사용하면 된다. 매개변수 value값이 HashSet 내부에 존재한다면 그 값을 삭제한 후 true를 반환하고 없다면 false를 반환한다. 

```java
HashSet<Integer> set = new HashSet<Integer>(Arrays.asList(1,2,3)); // 초기값 지정
set.remove(1); // 값 1 제거
```

ArraysList와 같이 List인터페이스를 구현한 컬렉션과 달리 HashSet은 저장순서를 유지하지 않으므로 저장순서를 유지하고자 한다면 LinkedHashSet을 사용한다. 

#### HashSet의 메서드

| 생성자 또는 메서드 | 설명 |
| --- | --- |
| HashSet() | HashSet객체를 생성한다. |
| HashSet(Collection c) | 주어진 컬렉션을 포함하는 HashSet객체를 생성한다. |
| HashSet(int initialCapacity) | 주어진 값을 초기용량으로하는 HashSet객체를 생성한다. |
| HashSet(int initialCapacity, float loadFactor) | 초기용량과 load factor를 지정하는 생성자 |
| boolean add(Object o) | 새로운 객체를 저장한다.  |
| boolean add(Collection c) | 주어진 컬렉션에 저장된 모든 객체들을 추가한다.(합집합) |
| void clear() | 저장된 모든 객체를 삭제한다. |
| Object clone() | HashSet을 복제해서 반환한다(얕은복사) |
| boolean contains(Object o) | 지정된 객체를 포함하고 있는지 알려준다. |
| boolean containsAll(Collection c) | 주어진 컬렉션에 저장된 모든 객체들을 포함하고 있는지 알려준다. |
| boolean isEmpty() | HashSet이 비어있는지 알려준다. |
| Iterator iterator() | Iterator를 반환한다 |
| boolean remove(Object o) | 지정된 객체를 HashSet에서 삭제한다.(성공하면 true, 실패하면 false) |
| boolean removeAll(Collection c) | 주어진 컬렉션에 저장된 모든 객체와 동일한 것들을 HashSet에서 모두 삭제한다(차집합) |
| boolean retainAll(Collection c) | 주어진 컬렉션에 저장된 객체와 동일한 것만 남기고 삭제한다.(교집합) |
| int size() | 저장된 객체의 개수를 반환한다. |
| Object[] toArray() | 저장된 객체들을 객체배열의 형태로 반환한다. |
| Object[] toArray(Object[] a) | 저장된 객체들을 주어진 객체배열(a)에 담는다. |

## 참고

- 자바의 정석