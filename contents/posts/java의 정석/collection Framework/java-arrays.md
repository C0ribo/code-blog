---
title: "[Java] Arrays(제네릭스x)"
description: "Arrays, copyOf(), copyOfRange(), fill(), setAll(), sort(), binarySearch(), equals(), toString(), asList(Object...a)"
date: 2023-08-27
update: 2023-08-27
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## Arrays

`java.util.Arrays`클래스는 **배열과 관련된 작업을 수행**하는 메서드를 제공한다. 

Arrays에 정의된 메서드는 모두 `static`메서드이다.

### copyOf(), copyOfRange()

`copyOf()`는 <u>배열 전체를 복사</u>하고, `copyOfRange()`는 <u>배열의 일부를 복사해서 새로운 배열을 만들어 반환</u>한다. (`copyOfRange()`에 지정된 범위의 끝은 포함되지 않는다.)

```java
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length); // arr2 = [0, 1, 2, 3, 4]
int[] arr3 = Arrays.copyOf(arr, 3); // arr3 = [0, 1, 2]
int[] arr4 = Arrays.copyOf(arr, 7); // arr4 = [0, 1, 2, 3, 4, 0, 0]
int[] arr5 = Arrays.copyOfRange(arr, 2, 4); // arr5 = [2, 3]
int[] arr6 = Arrays.copyOfRange(arr, 0, 7); // arr6 = [0, 1, 2, 3, 4, 0, 0]
```

### fill(), setAll()

`fill()`은 <u>모든 요소를 지정된 값으로 채우고</u>, `setAll()`은 <u>배열을 채우는데 사용할 함수형 인터페이스를 매개변수</u>로 받는다. (함수형 인터페이스를 매개변수로 지정하던가, 람다식으로 지정해야한다)

```java
int[] arr = new in[5];
Arrays.fill(arr, 9); // arr = [9, 9, 9, 9, 9]
Arrays.setAll(arr, () -> (int)(Math.random()*5)+1); // arr = [1,5,2,1,1]
```

`i → (int)(Math.random()*5)+1);` 은 람다식(lmabda expression)인데 1~5 범위에 속한 임의의 정수를 반환하는 일을 한다.

- `setAll()`메서드는 람다식이 반환한 임의의 정수로 배열을 채운다.

### sort(), binarySearch()

`sort()`는 배열을 정렬하는데 기본적으로 **오름차순으로 정렬**되며, `binarySearch()`는  배열에 저장된 요소를 검색할 때 사용한다. 

- `binarySearch()`는 배열에서 지정된 값이 저장된 위치(index)를 찾아서 반환하는데, 반드시 배열이 정렬된 상태야한다.
    - 다만, 검색한 값과 일치한 요소가 여러 개 있으면, 어떤 것이 반환될지 알 수 없다.
    
    ```java
    int[] arr = { 3, 2, 0, 1, 4};
    int idx = Arrays.binarySearch(arr, 2); // idx = -5 오류
    
    Arrays.sort(arr); // 배열 arr 정렬
    System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
    int idx = Arrays.binarySearch(arr, 2); // idx = 2 옳은 결과
    ```
    
- **순차 검색(linear search)** : 배열의 첫 번째 요소부터 순서대로 하나씩 검색하는 것을 말한다.
    - 이 검색 방법은 배열이 정렬되어 있을 필요는 없지만 배열의 요소를 하나씩 비교하기 때문에 시간이 많이 걸린다.
- **이진 검색(binary search)**은 배열의 검색할 범위를 반복적으로 절반씩 줄여가며 검색하기 때문에 검색 속도가 빠르다.
    - 배열의 길이가 10배 늘어나도 검색 회수를 3~4회 밖에 늘어나지 않으므로 큰 배열의 검색에 유리
    - 배열이 정렬이 되어 있는 경우에만 사용할 수 있다.

### equals(), toString()

`toString()`배열의 모든 요소를 문자열로 편하게 출력할 수 있다. 

- 1차원 배열에만 사용, 다차원 배열에 사용 시 **deepTo String()** 사용
    - **deepTo String()** : 모든 배열의 모든 요소를 재귀적으로 접근해서 문자열을 구성하므로 2차원, 3차원이상의 배열도 동작

```java
int[] arr = {0, 1, 2, 3, 4};
int[][] arr2D = {{11, 12}, {21, 22}};

System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
System.out.println(Arrays.deepToString(arr2D)); // [[11, 12], [21,22]]
```

`equals()`는 두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환한다.

- 1차원 배열에만 사용 가능하므로, 다차원 배열 비교 시 `deepEquals()` 사용

```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

SYstem.out.println(Arrays.equals(str2D, str2D2)); // false
System.out.println(Arrays.deepEquals(str2D, str2D2)); // true
```

String배열을 `equals()`로 비교하면 배열에 저장된 내용이 같은데도 false를 결과로 얻는다.

- 다차원 배열은 **배열과 배열**의 형태로 구성하기 때문에 `equals()`로 비교하면, 문자열을 비교하는 것이 아니라 **배열에 저장된 배열의 주소를 비교**하게 된다.
    - 서로 다른 배열은 항상 주소가 다르므로 **false**를 얻는다.

### asList(Object… a)

`asList()`는 배열을 List에 담아서 반환한다. 

- 매개변수의 타입이 가변인수라서 배열 생성 없이 저장할 요소들만 나열하는 것도 가능

```java
List list = Arrays.asList(new Integer[]{1,2,3,4,5}); // list =[1,2,3,4,5]
List list = Arrays.asList(1,2,3,4,5);
list.add(6); // UnsupportedOperationException 예외 발생
```

- `asList()`가 반환된 List의 크기를 변경할 수 없다.(삭제 또는 추가 불가)
- 저장된 내용은 변경이 가능하다.

```java
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```

크기를 변경할 수 있는 List가 필요하다면 위와 같이 하면 된다.

### parallelXXX(), spliterator(), stream()

- **parallel**로 시작하는 메서드는 빠른 결과를 얻기 위해 여러 쓰레드가 작업을 나누어 처리하도록 하는 역할이다.
- **spliterator()** : 여러 스레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 `Spliterator()`반환
- **stream()** : 컬렉션을 스트림으로 변환

## 참고

- 자바의 정석