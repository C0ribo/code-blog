---
title: "[Java] LinkedList"
description: "LinkedList"
date: 2023-08-24
update: 2023-08-24
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## LinkedList

배열은 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리는 시간이 가장 빠르다.  그에 반해 단점도 존재한다.

1. **크기를 변경할 수 없다.**
    - 크기를 변경할 수 없으므로 <u>새로운 배열을 생성해서 데이터를 복사</u>해야 한다.
    - 실행속도를 향상시키기 위해서는 충분히 큰 크기의 배열을 생성해야 하므로 메모리가 낭비된다.
2. **비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.**
    - 차례대로 데이터를 추가하고 마지막에서부터 데이터를 삭제하는 것은 빠르다.
    - 배열의 중간에 데이터를 추가하려면, 빈자리를 만들기 위해 다른 데이터들을 복사해서 이동해야 한다.

단점을 보완하기 위해 **링크드 리스트(linked list)**라는 자료구조를 사용한다.

![배열과 링크드 리스트.png](https://github.com/C0ribo/code-blog/assets/133131980/3f4f1882-f49b-42f5-97ae-15361d91f14b)

배열은 데이터가 연속적으로 존재하지만 링크드 리스트는 **불연속적으로 존재하는 데이터를 서로 연결(link)**한 형태로 구성되어 있다. 위 링크드 리스트의 각 요소(node)들은 자신과 연결된 다음 **요소에 대한 참조(주소값)와 데이터로 구성**되어 있다.

```java
class Node {
	Node next; // 다음 요소의 주소 저장
	Object obj; // 데이터를 저장
}
```

### 링크드 리스트 데이터 삭제

삭제하고자 하는 요소의 이전요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하기만 하면 된다. 단 하나의 참조만 변경하면 삭제가 된다.

![링크드 리스트에서 데이터삭제.png](https://github.com/C0ribo/code-blog/assets/133131980/737298f3-523a-4a5a-b6e0-4fe13e4abf01)

배열 복사과정이 없기 때문에 처리 속도가 빠르다.

### 링크드 리스트 데이터 추가

새로운 요소를 생성한 다음 추가하고자 하는 위치의 이전 요소의 참조를 새로운 요소에 대한 참조로 변경해주고, 새로운 요소가 그 다음 요소를 참조하도록 변경하기만 하면 되므로 처리속도가 매우 빠르다.

![링크드 리스트에서의 데이터 추가.png](https://github.com/C0ribo/code-blog/assets/133131980/deedab0e-ceda-4c7f-b489-f3886a90eeb2)

링크드 리스트는 이동방향이 단방향이기 때문에 다음요소에 대한 접근은 쉬운 편이지만 이전 요소에 대한 접근은 어렵다. 이러한 링크드 리스트의 보완한 방법이 **더블 링크드 리스트(이중 연결리스트, doubly linked list)**이다. 

### 더블 링크드 리스트

단순히 링크드 리스트에 참조변수를 하나 더 추가하여 다음 요소에 대한 참조뿐만 아니라 이전 요소에 대한 참조도 양 방향으로 가능할 뿐, 나머지는 링크드 리스트와 동일하다. 더블 링크드 리스트는 링크드 리스트보다 각 요소에 대한 접근과 이동이 쉽기 때문에 링크드 리스트보다 많이 사용된다.

```java
class Node {
	Node next; // 다음 요소의 주소를 저장
	Node previous; // 이전 요소의 주소를 저장
	Object obj; // 데이터를 저장
}
```

![링크드 리스트와 더블 링크드 리스트.png](https://github.com/C0ribo/code-blog/assets/133131980/714aaaca-b467-4bee-8c99-7c8033dd1bcf)

### 더블 써큘러 링크드 리스트(이중 원형 연결리스트, doubly circular linked list)

**더블 링크드 리스트의 첫 번째 요소와 마지막 요소를 서로 연결**시킨 것이다. 마지막요소의 다음 요소가 첫번째 요소가 되고, 첫 번째 요소의 이전 요소가 마지막 요소가 된다.(Tv 마지막 채널에서 한 채널 올리면 처음 채널로 돌아가는 것처럼)

![더블 써큘러 링크드 리스트.png](https://github.com/C0ribo/code-blog/assets/133131980/b9aad74d-96cb-4f22-b42a-ca192977d441)

실제로 LinkedList클래스는 이름과 달리, **더블 링크드 리스트로 구현**되어 있는데, <u>낮은 접근성을 높이기 위한 것</u>이다. 

| 생성자 또는 메서드 | 설명 |
| :--- | :--- |
| `LinkedList()` | LinkedList객체를 생성 |
| `LinkedList(Collection c)` | 주어진 컬렉션을 포함하는 LinkedList객체를 생성 |
| `boolean add(Object o)` | 지정된 객체(o)를 LinkedList의 끝에 추가.<br>저장에 성공하면 true, 실패하면 false |
| `void add(int index, Object element)` | 지정된 위치(index)에 객체(element)를 추가 |
| `boolean addAll(Collection c)` | 주어진 컬렉션에 포함된 모든 요소를 LinkedList의 끝에 추가한다.<br>성공하면 true, 실패하면 false |
| `boolean addAll(int index, Collection c)` | 지정된 위치(index)에 주어진 컬렉션에 포함된 모든 요소를 추가<br>성공하면 true, 실패하는 false |
| `void clear()` | LinkedList의 모든 요소를 삭제 |
| `boolean contains(Object o)` | 지정된 객체가 LinkedList에 포함되어있는지 알려줌 |
| `boolean containsAll(Collection c)` | 지정된 컬렉션의 모든 요소가 포함되었는지 알려줌 |
| `Object get(int index)` | 지정된 위치(index)의 객체를 반환 |
| `int indexOf(Object o)` | 지정된 객체가 저장된 위치(앞에서 몇번째)를 반환 |
| `boolean isEmpty()` | LinkedList가 비어있는지 알려준다<br>비어있으면 true |
| `Iterator iterator()` | Iterator를 반환 |
| `int lastIndexOf(Object o)` | 지정된 객체의 위치(index)를 반환(끝부터 역순검색) |
| `ListIterator listIterator()` | ListIterator를 반환 |
| `ListIterator listIterator(int index)` | 지정된 위치에서부터 시작하는 ListIterator를 반환 |
| `Object remove(int index)` | 지정된 위치(index)의 객체를 LinkedList에서 제거 |
| `boolean remove(Object o)` | 지정된 객체를 LinkedList에서 제거<br>성공하면 true, 실패하면 false |
| `boolean removeAll(Collection c)` | 지정된 컬렉션의 요소와 일치하는 요소를 모두 삭제 |
| `boolean retainAll(Collection c)` | 지정된 컬렉션의 모든 요소가 포함되어 있는지 확인 |
| `Object set(int index, Object element)` | 지정된 위치(index)의 객체를 주어진 객체로 바꿈 |
| `int size()` | LinkedList에 저장된 객체의 수를 반환 |
| `List subList(int fromIndex, int toIndex)` | LinkedList의 일부를 List로 반환 |
| `Object[] toArray()` | LinkedList에 저장된 객체를 배열로 반환 |
| `Object[] toArray(Object[] a)` | LinkedList에 저장된 객체를 주어진 배열에 저장하여 반환 |
| `Object element()` | LinkedList의 첫번째 요소를 반환 |
| `boolean offer(Object o)` | 지정된 객체(o)를 LinkedList의 끝에 추가<br>성공하면 true, 실패하면 false |
| `Object peek()` | LinkedList의 첫 번째 요소를 반환 |
| `Object poll()` | LinkedList의 첫번째 요소를 반환.<br>LinkedList에서는 제거된다 |
| `Object remove()` | LinkedList의 첫번째 요소를 제거 |
| `void addFirst(Object o)` | LinkedList의 맨 처음에 객체(o)를 추가 |
| `void addLast(Object o)` | LinkedList의 맨 끝에 객체(o)를 추가 |
| `Iterator descendingIterator()` | 역순으로 조회하기 위한 DescendingIterator를 반환 |
| `Object getFirst()` | LinkedList의 첫번째 요소를 반환 |
| `Object getLast()` | LinkedList의 마지막 요소를 반환 |
| `boolean offerFirst(Object o)` | LinkedList의 맨앞에 객체(o)를 추가. 성공하면 true |
| `boolean offerLast(Object o)` | LinkedList의 맨끝에 객체(o)를 추가. 성공하면 true |
| `Object peekFirst()` | LinkedList의 첫번째 요소를 반환 |
| `Object peekLast()` | LinkedList의 마지막 요소를 반환 |
| `Object pollFirst()` | LinkedList의 첫번째 요소를 반환하면서 제거 |
| `Object pollLast()` | LinkedList의 마지막 요소를 반환하면서 제거 |
| `Object pop()` | removeFirst()와 동일 |
| `void push(Object o)` | addFirst()와 동일 |
| `Object removeFirst()` | LinkedList의 첫번째 요소를 제거 |
| `Object removeLast()` | LinkedList의 마지막 요소를 제거 |
| `boolean removeFirstOccurrence(Object o)` | LinkedList에서 첫번째로 일치하는 객체를 제거  |
| `boolean removeLastOccurrence(Object o)` | LinkedList에서 마지막으로 일치하는 객체를 제거 |

LinkedList역시 List인터페이스를 구현했기 때문에 ArrayList와 내부 구현방법만 다를 뿐 **제공하는 메서드 종류와 기능은 거의 같다**.

### ArrayList와 LinkedList의 장단점

- **순차적으로 추가/삭제하는 경우**에는 `ArrayList`가 LinkedList보다 **빠르다**.
    1. 만약 ArrayList의 공간이 충분하지 않으면, 새로운 크기의 ArrayList를 생성하고 데이터를 복사하는 일이 생기므로 LinkedList가 더 빠를 수 있다.
    2. 순차적으로 삭제한다는 것은 마지막 데이터부터 역순으로 삭제해나간다는 것을 의미한다.
    3. 고로, ArrayList는 마지막 데이터부터 삭제할 경우 각 요소들의 재배치가 필요하지 않기 때문에 빠르다.(마지막 요소값은 null)
- **중간 데이터를 추가/삭제하는 경우**에는 `LinkedList`가 ArrayList보다 **빠르다**.
    1. LinkedList는 각 요소간의 연결만 변경해주면 되기 때문에 처리속도가 상당히 빠르다.
    2. ArrayList는 각 요소들을 재배치하여 추가할 공간을 확보하거나 빈 공간을 채워야하기 때문에 처리속도가 늦다.

배열의 경우 인덱스가 n인 요소의 값을 얻어 오고자 한다면 다음과 같이 계산해준다.

```java
인덱스가 n인 데이터의 주소 = 배열의 주소 + n * 데이터타입의 크기
```

arr[2]에 저장된 값을 읽을때  `n = 2`, 데이터타입의 크기 `4byte`, 생성된 배열의 주소 `0x100` 이므로 3번째 데이터가 저장된 주소는 `0x100 + 2 * 4 = 0x108`이 된다.

![배열의 메모리구조.png](https://github.com/C0ribo/code-blog/assets/133131980/4e572a81-65bd-4e88-ab5e-4c35e58a9bbe)

| 컬렉션 | 읽기(접근시간) | 추가/삭제 | 비고 |
| --- | --- | --- | --- |
| ArrayList | 빠르다 | 느리다 | 순차적인 추가삭제는 더 빠름(연속적인 요소로 나열되어 있기 때문)<br>비효율적인 메모리사용 |
| LinkedList | 느리다 | 빠르다 | 데이터가 많을수록 접근성이 떨어짐(불연속적 요소로 이루어져 있기 때문) |

## 참고

- 자바의 정석