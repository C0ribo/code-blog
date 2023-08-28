---
title: "[Java] Comparator과 Comparable(제네릭스x)"
description: "Comparator, Comparable"
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

## Comparator와 Comparable

Comparator와 Comparable은 모두 인터페이스로 컬렉션을 정렬하는데 필요한 메서드를 정의하고 있다.

- Comparable : 기본 정렬기준을 구현하는데 사용
- Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용

```java
public interface Comparator {
	int compare(Object o1, Object o2);
	boolean equals(Object obj);
}
public interface Comparable {
	public int compareTo(Object o);
}
```

> Comparable은 `java.lang패키지`에 있고, Comparator는 `java.util패키지`에 있다.
> 

### Comparable&Comparator

기본적으로 오름차순, 작은 값부터 큰 값의 순으로 정렬되도록 구현되어 있다. Comparable은 단일 메서드인 `CompareTo()`를 정의한다. 정수나 문자열같은 내장 자료형들은 이미 Comparable인터페이스를 구현하고 있어서 정렬이 가능하다. 

Comparator은 단일 메서드인 `compare()`를 정의하고 있다. 

- 음수 :  비교하는 값보다 작을 경우
- 0 : 비교하는 두 개체가 같을 경우
- 양수 : 비교하는 값보다 클 경우

```java
public final class Integer extends Number implements Comparable {
	...
	public int compareTo(Object o) {
		return compareTo((Integer)o);
	}
	public int compareTo(Integer anotherInteger) {
		int thisVal = this.value;
		int anotherVal = anotherInteger.value;
		// 비교하는 값이 크면 -1, 같으면 0, 작으면 1을 반환
		return (thisVal<anotherVal ? -1 : (thisVal == anotherVal ? 0 : 1));
	}
	...
}
```

Comparable의 compareTo(Object o)를 구현했다. int값(value)을 비교해서 같으면 0, 크면 -1, 작으면 1을 반환하는 것을 알 수 있다. 

```java
static void sort(Object[] a) // 객체배열에 저장된 객체가 구현된 Comparable에 의한 정렬
static void sort(Object[] a, Comparator c) // 지정한 Comparator에 의한 정렬
```

Arrays.sort()는 배열을 정렬할 때, Comparator를 지정해주지 않으면 저장하는 객체(주로 Comparable을 구현한 클래스의 객체)에 구현된 내용에 따라 정렬된다. 

String의 Comparable구현은 문자열이 사전 순으로 정렬되도록 작성되어 있다. 

- 문자열의 오름차순 정렬 : 공백, 숫자, 대문자, 소문자 순으로 정렬
- 문자의 유니코드의 순서가 작은 값에서부터 큰 값으로 정렬

```java
public static final Comparator CASE_INSENSITIVE_ORDER
```

대소문자를 구분하지 않고 비교하는 Comparator를 상수의 형태로 제공한다. 

```java
Arrays.sort(strArr, String.CASE_INSENSITIVE_ORDER); // 대소문자 구분없이 정렬
```

Comparator를 사용하면, 문자열을 대소문자 구분없이 정렬할 수 있다. 

#### 문자열 내림차순(descending order)을 구현하는 방법

- String에 구현된 `comparaTo()`의 결과에 `-1`을 곱하기만 하면 된다.
- 또는 비교하는 객체의 위치를 바꿔서 `c2.compareTo(c1)`으로 해도 된다.

compare()의 매개변수가 Object타입이기 때문에 `compareTo()`를 바로 호출할 수 없기 때문에 Comparable로 형변환을 먼저 해야한다.

```java
class Descending implements Comparator {
	public int compare(Object o1, Object o2) {
		if(o1 instanceof Comparable && o2 instanceof Comparable) {
			Comparable c1 = (Comparable)o1; 
			Comparable c2 = (Comparable)o2;
			return c1.compareTo(c2) * -1; // 기본 정렬방식의 역으로 변경
		}
		return -1;
	}
}
```

## 참고

- 자바의 정석