---
title: "[Java] 1차원 배열(Array)"
description: "배열 선언, 초기화, 출력, 복사"
date: 2023-07-20
update: 2023-07-20
tags:
  - java
  - study
series: "Java 기초"
---

> 책에 부족한 내용이 많아서 혼자 보기 위해 기록해서 남긴 글

### 배열이란

배열은 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것이다. 

![자바 배열 자료(1)](https://github.com/C0ribo/code-blog/assets/133131980/28708339-6486-4eb0-9567-50514eaed7a9)

배열을 구성하는 각각의 값을 배열의 요소(element), 배열의 위치를 가리키는 숫자를 인덱스(index)라고 한다. 배열이름[인덱스]의 형식으로 이루어진다. 인덱스의 범위는 0부터 '배열 길이 - 1' 까지이다.

#### 배열의 선언과 생성


배열을 생성할 때에는 **타입[] 변수이름;** 이나 **타입 변수이름[];** 으로 나타낸다. 배열을 생성하기 위해서 연산자 **'new'**와 함께 배열의 타입과 길이를 지정해 주어야 한다.
```java
타입[] 변수이름;    // 배열 선언
변수이름 = new 타입[길이]; // 배열 생성
```
예로 들자면,
```java
int[] score; // 배열 선언
score = new int[5]; // 5개의 빈 배열 생성
```

위와 같이 나타낼 수 있다.

배열은 선언과 생성을 동시에 한줄로 나타낼수도 있다.
```java
int[] score = new int[5];
```

#### 배열의 인덱스와 길이

##### 배열의 인덱스

배열의 인덱스는 앞에서 말한 것과 같이 길이가 5인 배열은 모두 5개의 요소를 가지게 되며 인덱스의 범위는 0부터 시작되서 0, 1, 2, 3, 4 로 5는 포함하지 않는다. 


##### 배열의 길이

배열의 길이는 배열의 요소 개수로 값을 저장할 수 있는 공간의 개수이다. 배열의 길이는 int범위의 양의 정수(0 포함)이어야 한다. 또한 배열의 길이를 나타내고 싶을 때에는 **'배열이름.length'** 로 나타낸다.
```java
int[] arr = new int[5]; // 길이가 5인 int배열 
int tmp = arr.length; // arr.length의 값 5이고 tmp에 저장된다.
```

배열은 한번 생성하면 길이를 변경할 수 없기에 '배열이름.length'는 상수이다. 

배열의 길이를 나타낼 때에는 for문을 이용하여 나타낼 수 있는데 for문의 조건식에 배열의 길이를 직접 적어주는 것보다 배열이름.length를 사용하여 나타내는 것이 좋다. 

```java
for(int i=0; i < 6; i++) -> for(int i=0; i < score.length; i++)
```

#### 배열의 초기화

배열은 생성과 동시에 자동적으로 자시의 타입에 해당하는 기본값으로 초기화되지만 원하는 값을 저장하려면 지정해주어야 한다.
```java
int[] score = new int[3];
score[0] = 50; // 각 빈 공간에 값 초기화
score[1] = 20;
score[2] = 40; 
```

배열을 생성과 초기화를 한번에 진행할 수도 있다.
```java
int[] score = new int[] { 50, 20, 40 };
```
저 괄호 {} 안에 아무것도 넣지 않으면 길이가 0인 배열이 생성된다. 

#### 배열의 출력

배열을 출력할 때 변수를 출력하듯이 **System.out.println** 으로 출력하면 이상한 값이 나오게 된다.
```java
int[] iArr = { 100, 95, 80, 70, 60 };
System.out.println(iArr); // [I@14318bb 
```

타입@주소의 형식으로 출력이 되는데 '[I'는 1차원 int 배열이고 @뒤에는 배열의 주소를 16진수로 나타낸다.
이러한 방법을 해결하기 위해서는 `Arrays.toString(배열이름)`를 사용하여 배열을 문자열 형식으로 만들어 출력한다.
```java
int[] iArr = { 100, 95, 80, 70, 60 };
System.out.println(Arrays.toString(iArr));
```

다만 `Arrays.toString()`을 사용하려면 `import java.util.*`를 추가해야 한다.

#### 배열의 복사 

배열은 한번 생성하면 길이를 변경할 수 없기에 더 많은 저장공간이 필요하다면 
1. 더 큰 배열을 생성한다.
2. 기존 배열의 내용을 새로운 배열에 복사한다.   

라는 방법이 있다. 

그 중에서 배열을 복사하는 방법을 알아볼텐데 방법은 두가지가 있다.
- for문을 이용해서 배열을 복사
- `System.arraycopy()` 메서드나 `Arrays.copyOf()` 메서드 사용

##### for문을 이용해서 배열 복사 
```java
int[] arr = new int[5];
int[] tmp = new int[arr.length*2];

for(int i=0; i< arr.length; i++)
  tmp[i] = arr[i];
  arr = tmp; 
```
다만 이러한 작업들은 비용이 많이 들기 떄문에 처음부터 배열 길이를 넉넉하게 잡는게 낫다.

##### System.arraycopy() 메서드와 Arrays.copyOf() 메서드

- System클래스의 `arraycopy()`를 사용하면 간편하고 빠르게 배열을 복사할 수 있다. 또한 지정된 범위의 값을 한번에 복사한다.
- `Arrays.copyOf()`는 `System.copyOf()`를 래핑한 함수로 둘이 동일하다. `Arrays.copyOf()`가 좀 더 직관적이다.

```java
System.arraycopy(복사할 배열, 복사를 시작할 배열의 위치, 붙여넣을 배열, 복사된 배열값들이 붙여질 시작위치, 지정된 길이만큼 값들이 복사)
System.arraycopy(num , 0, newNum, 0, num.length);
```

- Arrays.copyOf()
```java
Arrays.copyOf(원본 배열, 복사할 길이)
arr2 = Arrays.copyOf(arr1, arr1.length);
```

- Arrays.copyOfRange()
```java
Arrays.copyOfRange(원본 배열, 복사 시작 인덱스, 복사하려는 마지막 요소의 인덱스 바로 다음 인덱스);
arr2 = Arrays.copyOfRange(arr1, 1, 3); 
```


### 참조

- 자바의 정석
- [자바의 배열](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94-%EB%B0%B0%EC%97%B4Array-%EB%AC%B8%EB%B2%95-%EC%9D%91%EC%9A%A9-%EC%B4%9D%EC%A0%95%EB%A6%AC)