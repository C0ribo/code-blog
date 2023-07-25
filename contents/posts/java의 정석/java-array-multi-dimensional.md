---
title: "[Java] 다차원 배열(Array)"
description: "2차원 배열 생성, 2차원 배열 출력, 가변 배열"
date: 2023-07-21
update: 2023-07-21
tags:
  - java
  - study
series: "Java 기초"
---

## 다차원 배열

다차원 배열은 2차원 이상의 배열을 의미한다. 1차원 배열은 배열 요소로 '단일값'을 가지는 배열이고, 2차원 배열은 배열 요소로 '1차원 배열'을 가지는 배열이다. 

### 배열의 생성

2차원 배열은 1차원 배열 형태에서 괄호[] 하나를 더 붙인 꼴이다.

|선언 방법|선언 예|
|----|----|
|타입[] 변수이름;   | int[][] score;|
|타입 변수이름[][]; | int score[][];|
|타입[] 변수이름[]; | int[] score[];|

2차원 배열은 테이블 형태의 데이터를 담는데 사용된다. 행(row)과 열(column)로 구성되어있다.
```java
int[][] score = new int[4][3]; // 4행 3열의 2차원 배열 생성
```

![다차원 이미지](https://github.com/C0ribo/code-blog/assets/133131980/c4533619-1a9c-4cb1-a249-f070ddaa41f5)

### 배열의 출력

1차원 배열의 `Arrays.toString()`을 이용해 출력했던 것처럼 2차원 배열도 `Arrays.deepToString()`을 이용해 출력할 수 있다. 

```java
int[] arr2 = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
System.out.println(Arrays.deepToString(arr2));
```

### 배열의 초기화

2차원 배열도 괄호{}를 사용해서 생성과 초기화를 동시에 할 수 있다. 
```java
int[] arr = new int[][]{ {1,2,3}, {4,5,6} };
```
for문을 이용해서 2차원 배열을 초기화 할 수 있다.
```java
for(int i=0; i< score.length; i++){
  for(int j=0; j<score[i].length; j++){
    score[i][j]=10;
  }
}
```
위 코드는 2차원 배열 score의 모든 요소를 10으로 초기화한다.

### 가변 배열

2차원 이상의 다차원 배열을 생성할 때 열과 행이 균등하지 않고 마지막 차수의 길이를 다르게 지정할 수 있기 때문에 이러한 것을 **가변 배열** 이라고 부른다.
```java
int[][] score = new int[5][3]; // 5행 3열의 2차원 배열 생성
```
위의 코드처럼 5 x 3 길이의 2차원 배열을 생성한다. 
```java
int[][] score = new int[5][];
score[0] = new int[4];
score[1] = new int[3];
score[2] = new int[2];
score[3] = new int[2];
score[4] = new int[3];
```
위의 코드를 그림으로 나타내면 다음과 같다.
![자바의 정석](https://github.com/C0ribo/code-blog/assets/133131980/1a8aaae6-a742-484b-b1f7-6974ea11d0b5)

가변 배열도 중괄호{}를 이용해서 생성과 초기화를 동시에 하는 것이 가능하다.
```java
int[][] score = {
   {100, 100, 100, 100}
  ,{20, 20, 20}
  ,{30, 30}
  ,{40, 40}
  ,{50, 50, 50}
};
```

### 참조

- 자바의 정석
- [참조 블로그](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94-%EB%B0%B0%EC%97%B4Array-%EB%AC%B8%EB%B2%95-%EC%9D%91%EC%9A%A9-%EC%B4%9D%EC%A0%95%EB%A6%AC)