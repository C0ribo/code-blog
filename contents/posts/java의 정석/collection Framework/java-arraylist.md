---
title: "[Java] ArrayList (제네릭스x)"
description: "ArrayList"
date: 2023-08-23
update: 2023-08-23
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## ArrayList

- 컬렉션 프레임워크에서 가장 많은 사용되는 컬렉션 클래스일 것이다.
- ArrayList는 List인터페이스를 구현하기 때문에 **데이터의 저장순서가 유지되고 중복을 허용**하는 특징을 가진다.
- ArrayList는 기존의 Vector를 개선한 것으로 구현원리와 기능적인 측면에서 동일하다고 할 수 있다.
    - 그래도 Vector보다는 ArrayList를 사용하자
- ArrayList는 Object배열을 이용해서 데이터를 순차적으로 저장한다.
    - 첫번째 저장 객체 → Object배열의 0번째 위치 저장
    - 두번째 저장 객체 → Object배열의 1번째 위치 저장
    - 저장할 공간 없을 시 **더 큰 새로운 배열 생성 후 기존 배열 내용을 새로운 배열에 복사한 뒤 저장**한다.

```java
public class ArrayList extends AbstractList
	implements List, RandomAccess, Cloneable, java.io.Serializable {
			...
		transient Object[] elementData; // Object배열
			...
}
```

> `transient`는 직렬화(serialization)와 관련된 제어자이다.

- 위 코드는 ArrayList의 소스코드 일부로 ArrayList는 elementData라는 이름의 Object배열을 멤버변수로 선언하고 있다.
- 선언된 배열의 타입이 모든 객체의 최고조상인 Object이기 때문에 모든 종류의 객체를 담을 수 있다.

| 메서드 | 설명 |
| --- | --- |
| `ArrayList()` | 크기가 10인 ArrayList를 생성 |
| `ArrayList(Collection c)` | 주어진 컬렉션이 저장된 ArrayList를 생성 |
|  `ArrayList(int initialCapacity)` | 지정된 초기용량을 갖는 ArrayList를 생성 |
| `boolean add(Object o)` | ArrayList의 마지막에 객체를 추가. 성공하면 true, 그렇지 않으면 false |
| `void add(int index, Object element)` | 지정된 위치(index)에 객체를 저장 |
| `boolean addAll(Collection c)` | 주어진 컬렉션의 모든 객체를 저장 |
| `boolean addAll(int index, Collection c)` | 지정된 위치부터 주어진 컬렉션의 모든 객체를 저장 |
| `void clear()` | ArrayList를 완전히 비운다 |
| `Object clone()` | ArrayList를 복제한다 |
| `boolean contains(Object o)` | 지정된 객체(o)가 ArrayList에 포함되어 있는지 확인 |
| `void ensureCapacity(int minCapacity)` | ArrayList의 용량이 최소한 minCapacity가 되도록 한다 |
| `Object get(int index)` | 지정된 위치(index)에 저장된 객체를 반환 |
| `int indexOf(Object o)` | 지정된 객체가 저장된 위치를 찾아 반환한다. |
| `boolean isEmpty()` | ArrayList가 비어있는지  확인한다. |
| `Iterator iterator()` | ArrayList의 Iterator객체를 반환 |
| `int lastIndexOf(Object o)` | 객체(o)가 저장된 위치를 끝부터 역방향으로 검색해서 반환 |
| `ListIterator listIterator()` | ArrayList의 ListIterator를 반환 |
| `ListIterator listIterator(int index)` | ArrayList의 지정된 위치부터 시작하는 ListIterator()를 반환 |
| `Object remove(int index)` | 지정된 위치(index)에 있는 객체를 제거한다. |
| `boolean remove(Object o)` | 지정한 객체를 제거한다.(성공 시 true, 실패하면 false) |
| `boolean removeAll(Collection c)` | 지정된 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다. |
| `boolean retainAll(Collection c)` | ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만 남기고 나머지는 삭제한다. |
| `Object set(int index, Object element)` | 주어진 객체(element)를 지정된 위치(index)에 저장한다 |
| `int size()` | ArrayList에 저장된 객체의 개수를 반환 |
| `void sort(Comparator c)` | 지정된 정렬기준(c)으로 ArrayList를 정렬 |
| `List subList(int fromIndex, int toIndex)` | fromIndex부터 toIndex사이에 저장된 객체를 반환한다 |
| `Object[] toArray()` | ArrayList에 저장된 모든 객체들을 객체배열로 반환한다 |
| `Object[] toArray(Object[] a)` | ArrayList에 저장된 모든 객체들을 객체배열 a에 담아 반환한다. |
| `void trimToSize()` | 용량을 크기에 맞게 줄인다(빈 공간을 없앤다) |

```java
import java.util.*;

class ArrayListEx1{
	public static void main(String[] args) {
		ArrayList list1 = new ArrayList(10); // list1이라는 객체 10개 생성
		list1.add(new Integer(5));
		list1.add(new Integer(4));
		list1.add(new Integer(2));
		list1.add(new Integer(0));
		list1.add(new Integer(1));
		list1.add(new Integer(3));
		// [5, 4, 2, 0, 1, 3]
		ArrayList list2 = new ArrayList(list1.subList(1,4));
// List subList(int fromIndex, int toIndex) : fromIndex~toIndex사이 저장된 객체 반환
		// [ 4, 2, 0]
		print(list1, list2);
		
		Collections.sort(list1); // list1과 list2를 정렬
		Collections.sort(list2); // Collections.sort(List 1);

		System.out.println("list1.containsAll(list2):" 
							        + list1.containsAll(list2));
		list2.add("B");
		list2.add("c");

		list2.add(3, "A");
		print(list1, list2);
		
		list2.set(3, "AA");
		print(list1, list2);

		// list1에서 list2와 겹치는 부분만 남기고 나머지는 삭제한다.
		System.out.println("list1.retainAll(list2) :" 
										+ list1.retainAll(list2));
		print(list1, list2);
		
		// list2에서 list1에 포함된 객체들을 삭제한다.(공통요소 삭제)
		for(int i=list2.size()-1; i>= 0; i--) {
		// for문의 변수 i를 0부터 증가시킨 것이 아닌, list2.size()-1부터 감소시키며 거꾸로 반복
			 if(list1.contains(list2.get(i))) 
				// list2의 각 요소 접근을 위해 get(int index)메서드와 for문을 사용
					list2.remove(i);
		}
		print(list1, list2)
	} 

	static void print(ArrayList list1, ArrayList list2) {
		System.out.println("list1:"+list1);
		System.out.println("list2:"+list2);
		System.out.println();
	}
}
```

```java
list1: [5, 4, 2, 0, 1, 3]
list2: [4, 2, 0]

list1: [0, 1, 2, 3, 4, 5] // Collections.sort(List 1)를 이용해서 정렬
list2: [0, 2, 4]

list1.containsAll(list2) : true // list1이 list2의 모든 요소를 포함하고 있을 때 true
list1: [0, 1, 2, 3, 4, 5]
list2: [0, 2, 4, A, B, C] // add(Object obj)를 이용해서 새로운 객체를 저장

list1: [0, 1, 2, 3, 4, 5]
list2: [0, 2, 4, AA, B, C] // set(int index, Object obj)를 이용해 다른객체로 변경

list1.retainAll(list2) : true // retainAll에 의해 list1에 변화가 있었으므로 true를 반환
list1 : [0, 2, 4] // list2와의 공통요소 이외에는 모드 삭제되었다.
list2 : [0, 2, 4, AA, B, C]

list1 : [0, 2, 4]
list2 : [AA, B, C]
```

- ArrayList는 List인터페이스를 구현했기 때문에 저장된 순서를 유지한다는 것을 알 수 있다.
- `Collection`은 인터페이스이고, `Collections`는 클래스이다.
- 제어변수를 감소시켜가면서 삭제를 해야 자리이동이 발생해도 영향을 받지 않고 작업이 가능하다.

```java
final int LIMIT = 10; // 자르고자 하는 글자의 개수를 지정
String source = "0123456789abcdefghijABCDEFGHIJ!@#$%^&*()ZZZ";
int length = source.length();

List list = new ArrayList(length/LIMIT + 10); // 크기를 여유있게 잡는다
```

- ArrayList를 생성할 때, 저장할 요소의 개수를 고려해서 실제 저장할 개수보다 약간 여유있는 크기로 하는게 좋다.
    - 생성할 때 지정한 크기보다 더 많은 객체를 저장하면 자동적으로 크기가 늘어나긴하지만, 시간이 오래걸리기 때문이다.

- ArrayList나 Vector같이 배열을 이용한 자료구조는 데이터를 읽어오고 저장하는데 효율이 좋지만, 용량을 변경할 때는 **새로운 배열을 생성 후 기존 배열로부터 복사**해야하기 때문에 효율이 떨어진다.

## 참조
- 자바의 정석
- [제네릭스 ArrayList에 대한 설명이 좋은 블로그](https://coding-factory.tistory.com/551)