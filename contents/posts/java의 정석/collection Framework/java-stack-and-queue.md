---
title: "[Java] Stack과 Queue"
description: "Stack, Queue, Deque, PriorityQueue"
date: 2023-08-25
update: 2023-08-25
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## Stack과 Queue

- **스택** : 데이터를 저장하고 관리하는 추상적인 자료구조로 , 마지막에 저장한 데이터를 가장 먼저 꺼내게 되는 `LIFO(Last In First Out)`구조로 되어 있다.
    - 0, 1, 2 순서로 데이터를 넣었다면 2, 1, 0 순으로 꺼낸다.
    - 데이터를 추가하고 삭제하는 스택에서는 **ArrayList와 같은 배열기반의 컬렉션클래스가 적합**하다.
- **큐** : 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 `FIFO(First In First Out)`구조로 되어 있다.
    - 0, 1, 2 순서로 데이터를 넣었다면 0, 1, 2 순 그대로 꺼낸다.
    - 큐는 데이터를 꺼낼 때 항상 첫 번째 저장된 데이터를 삭제하므로, 데이터의 추가/삭제가 쉬운 **LinkedList로 구현**하는 것이 적합하다.

![스택과 큐.png](https://github.com/C0ribo/code-blog/assets/133131980/345bf808-5524-4461-9764-7b8c5f28f6f3)

### Stack메서드

| 메서드 | 설명 |
| --- | --- |
| `boolean empty()`  | Stack이 비어있는지 알려준다. |
| `Object peek()` | Stack의 맨 위에 저장된 객체를 반환. pop()과 달리 Stack에서 객체를 꺼내지는 않음. (비어있을 때 EmptyStackException발생) |
| `Object pop()` | Stack의 맨 위에 저장된 객체를 꺼낸다.(비어있을 때 EmptyStackException발생) |
| `Object push(Object item)` | Stack에 객체(item)를 저장 |
| `int search(Object o)` | Stack에서 주어진 객체(o)를 찾아서 그 위치를 반환. 못찾으면 -1을 반환. (배열과 달리 위치는 0이 아닌 1부터 시작) |

### Queue의 메서드

| 메서드 | 설명 |
| --- | --- |
| `boolean add(Object o)` | 지정된 객체를 Queue에 추가한다. 성공하면 true를 반환. 저장공간이 부족하면 IllegalStateException발생 |
| `Object remove()` | Queue에서 객체를 꺼내 반환. 비어있으면 NoSuchElementException발생 |
| `Object element()` | 삭제없이 요소를 읽어온다. peek와 달리 Queue가 비었을 때 NoSuchElementException발생 |
| `boolean offer(Object o)` | Queue에 객체를 저장. 성공하면 true, 실패하면 false 반환 |
| `Object poll()` | Queue에서 객체를 꺼내 반환. 비어있으면 null을 반환 |
| `Object peek()`| 삭제없이 요소를 읽어 온다. Queue가 비어있으며 null을 반환 |

#### 예외 메모

`NoSuchElementException`

- 컬렉션(자료구조)를 다룰 때 발생.
    - 컬렉션에서 요소를 조회하려 할 때 해당 요소가 존재하지 않은 경우
    - 컬렉션에서 요소를 가져오거나 제거하려 할 때 컬렉션 자체가 비어있는 경우

`EmptyStackException`

- 스택 자료구조를 다룰 때 발생할 수 있는 예외 중 하나이다.
- 발생 원인
    - 스택에서 요소를 `팝(pop)`하려 할 때 스택이 비어있는 경우
    - 스택에서 요소를 `피크(peek)`하려 할 때 스택이 비어있는 경우

`IllegalStateException`

- 주로 객체의 상태가 허용되지 않은 상태일 때 발생
    - 객체의 상태나 조건이 메서드 호출을 수행하기 적절하지 않을 경우 사용된다.
- 발생 원인
    - **객체의 상태가 올바르지 않을 때** : 객체의 메서드를 호출하기 전에 필요한 초기화나 설정이 이루어지지 않은 경우나, 메서드 호출 순서가 잘못된 경우
    - **작업을 허용되지 않은 경우** : 객체의 현재 상태에 따라 특정 작업이 허용되지 않은 경우
    - **리소스 누수 방지** : 리소스를 할당한 후 해제하지 않은 상태에서 다시 해당 리소스를 사용하려 할 때
    

자바에서는 **스택을 Stack클래스로 구현**하여 제공하지만 **큐는 Queue인터페이스로**만 정의해놓았을 뿐 <u>스택과 다르게 클래스를 제공하고 있지 않다.</u> 대신에 Queue인터페이스를 구현한 클래스들이 있어서 그 클래스들 중에 택하여 사용하면 된다.

#### Queue 인터페이스 구현 클래스들 종류

```java
Queue q = new LinkedList(); // 객체 생성 사용
```

| 클래스 | 설명 |
| --- | --- |
| `LinkedList` | `java.util.LinkedList` 클래스는 연결 리스트를 기반으로한 Queue의 구현이다. 큐의 연산을 상수 시간(O(1))에 수행할 수 있어서 많이 사용된다. |
| `PriorityQueue` | `java.util.PriorityQueue` 클래스는 우선순위 큐를 구현한 것으로, 요소들이 우선순위에 따라 정렬되어 처리된다. 최소값 또는 최대값을 빠르게 찾을 수 있다는 장점이 있다. |
| `ArrayDeque` | `java.util.ArrayDeque` 클래스는 배열 기반의 양방향 큐의 구현이다. 큐 및 스택의 기능을 모두 제공하며, 큐의 연산도 상수 시간(O(1))에 수행된다. |
| `LinkedBlockingQueue` | `java.util.concurrent.LinkedBlockingQueue` 클래스는 스레드 안전(thread-safe)한 큐의 구현으로, 멀티스레드 환경에서 사용하기에 적합하다. |
| `ArrayBlockingQueue` | `java.util.concurrent.ArrayBlockingQueue` 클래스는 배열을 기반으로한 스레드 안전한 큐의 구현으로 큐의 크기를 제한할 수 있다. |
| `ConcurrentLinkedQueue` | `java.util.concurrent.ConcurrentLinkedQueue` 클래스는 비차단적(non-blocking)으로 스레드 안전한 큐를 구현한 것으로, 다수의 스에드에서 효율적으로 사용할 수 있다. |
| `DelayQueue` | `java.util.concurrent.DelayQueue` 클래스는 요소들이 지정된 지연 시간 후에 큐에서 사용 가능한 우선순위 큐를 구현한 것이다. |
| `AbstractQueue` | 자바에서 제공하는 추상 클래스로서, 실제 큐를 구현하는데 도움을 주며 사용자가 새로운 큐를 구현할 때 일부 메서드를 미리 구현한 상태로 제공한다.  |
| `ConcurrentLinkedDeque` | **스레드 안전한 양방향 연결 덱(Double-ended Queue)을 구현**한 클래스이다. 덱은 큐(FIFO)와 스택(LIFO)의 특성을 모두 가지고 있으며, 양쪽 끝에서 요소를 추가하거나 제거할 수 있는 자료구조이다. 멀티쓰레드 환경에서 사용할 수 있으며, 비차단 알고리즘이 사용되어 동시에 수정하더라도 안전한 수행을 하여 데이터 무결성을 보장한다. |
| `LinkedBlockingDeque` | `BlockingDeque` 인터페이스 구현 중 하나로, **양방향 블로킹 연결 덱을 구현**한 클래스이다. 이 클래스는 큐와 스택의 특성을 모두 가지면서 동시에 스레드간에 안전하게 블로킹(Blocking) 연산을 지원한다. 내부적으로 연결 리스트 기반으로 구현되었으며 멀티스레드 환경에서 안전한 동작을 보장한다. 또한 큐의 크기를 제한할 수 있어 제한된 용량을 가지는 블로킹 큐를 생성할 수 있다. |
| `LinkedTransferQueue` | `TransferQueue` 인터페이스의 구현 클래스로, **스레드 간에 데이터를 전달하면서 블로킹 연산을 지원**하는 큐 자료구조이다. 멀티스레드 환경에서 안전하게 사용할 수 있으며, `tryTransfer()` 메서드를 사용할 때 데이터를 우선적으로 전달할 스레드를 선택할 수 있다. 큐의 크기에 제한이 없으며, 요소를 추가할 때 크기가 동적으로 조정된다.  |
| `PriorityBlockingQueue` | `BlockingQueue`인터페이스의 구현 클래스로, **우선순위에 따라 요소를 처리하는 블로킹 큐 구조**이다. 이 클래스는 우선순위 큐(Priority Queue)를 구현한 것으로, 요소들이 우선순위에 따라 정렬되어 처리된다.  |
| `SynchronousQueue` | `BlockingQueue`인터페이스의 구현 클래스로, **블로킹 큐의 특수한 형태**이다. 이  큐는 요소를 보내는(producing) 스레드와 요소를 받는(consuming) 스레드 간에 일시적으로 데이터를 전달하는데 사용된다. SynchronousQueue는 큐의 크기가 0으로 고정되어 있어 실제로 데이터를 보관하지 않는다. 대신에 데이터 전달을 위해 스레드 간의 블로킹을 조절하는데 사용된다.  |

> 잘못된 정보가 입력되어 있다면 알려주시길 바랍니다.


### PriorityQueue

Queue인터페이스의 구현체 중의 하나로, **저장한 순서에 관계없이 우선순위(priority)가 높은 것부터 꺼내게 된다는 특징**이 있다.

- null은 저장할 수 없다.
    - null을 저장하면 `NullPointerException`이 발생
- 저장공간으로 배열을 사용
    - 각 요소를 `힙(heap)`이라는 자료구조의 형태로 저장
        - **힙**은 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다.

> 자료구조 힙(heap)은 JVM 힙과 다르다.
> 

### Deque(Double-Ended Queue)

Queue의 변형으로, 한쪽 끝으로만 추가/삭제할 수 있는 Queue와 달리, `Deque`(덱, 또는 디큐라 읽음)은 **양쪽 끝에 추가/삭제가 가능**하다. Deque의 조상은 Queue이며, 구현체로는 `ArrayDeque`과 `LinkedList` 등이 있다.

![큐와 덱.png](https://github.com/C0ribo/code-blog/assets/133131980/f9b7d07c-dcb0-4132-a8ce-abce56ad4f3d)

**덱**은 <u>스택과 큐를 하나로 합쳐놓은 것과 같</u>으며 스택으로 사용할 수 있고, 큐로 사용할 수 있다.

| Deque | Queue | Stack |
| --- | :---: | --- |
| `offerLast()` | **offer()** | **push()** |
| `pollLast()` | **-** | **pop()** |
| `pollFirst()` | **poll()** | **-** |
| `peekFirst()` | **peek()** |  |
| `peekLast()` | **-** | **peek()** |

## 참고

- 자바의 정석 