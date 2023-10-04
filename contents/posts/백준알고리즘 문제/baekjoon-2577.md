---
title: "[Java] 백준 2577 알고리즘 문제"
description: "java, 브론즈2"
date: 2023-10-04
update: 2023-10-04
tags:
  - java
  - 알고리즘
  - 백준
series: "백준 알고리즘"
---

## 글을 쓰게 된 계기

문제를 풀다가 기본적인 걸 너무 모르는 거 같아서. 사실상 복습과 오답노트 개념 차원으로 쓰게 되었다.

## 문제

세 개의 자연수 A, B, C가 주어질 때 A × B × C를 계산한 결과에 0부터 9까지 각각의 숫자가 몇 번씩 
쓰였는지를 구하는 프로그램을 작성하시오.
예를 들어 A = 150, B = 266, C = 427 이라면 A × B × C = 150 × 266 × 427 = 17037300 이 
되고, 계산한 결과 17037300 에는 0이 3번, 1이 1번, 3이 2번, 7이 2번 쓰였다.

## 내가 푼 방식

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        int a = Integer.parseInt(br.readLine());
        int b = Integer.parseInt(br.readLine());
        int c = Integer.parseInt(br.readLine());

        int multipler = a * b * c;
        int[] num = new int[10];
        char[] arr = String.valueOf(multipler).toCharArray();

        for(int i = 0; i<arr.length; i++) {
            char ch = arr[i];
            int index = Integer.parseInt(String.valueOf(ch));
            num[index]++;
        }
        for (int j : num) {
            sb.append(j).append("\n");
        }
        System.out.println(sb);
    }
}
```

## 고쳐야 하는 점

### 무분별한 StringTokenizer 사용
```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringBuilder sb = new StringBuilder();
StringTokenizer st = new StringTokenizer(br.readLine(), "\n");

int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());
int c = Integer.parseInt(st.nextToken());
```
처음에는 이런식으로 풀려고 했다. 그리고 혼났다. 나는 무조건 `StringTokenizer`를 이용해 입력문을 나누려고 했다.

### String과 char 변환 이해도

`toCharArray()` 사용이 미숙하며, `String.valueOf()`을 생각도 못했다.

### 배열 이해도 박살
```java
for(int i = 0; i<arr.length; i++) {
    char ch = arr[i];
    int index = Integer.parseInt(String.valueOf(ch));
    num[index]++;
}
```
arr은 입력받은 숫자 세개를 곱한 결과값이다. 그 결과값을 char형으로 쪼개서 배열로 만든 것이다.
하지만 multipler는 int형이고, int형에서 char형으로 바로 바꿀 순 없으므로 먼저 int형을 String형으로 바꾼 뒤
char형 배열로 넣었다.

거기서 사용한 게 `String.valueOf()`이다. 이후, 그 결과값만큼 배열의 길이를 생성해줘야 하기 때문에 for문을 썼다.
이후에 원래는 조건문을 사용해서 만들어준 0부터 9까지의 배열 10개인 num[i]와 결과값의 arr[i]의 수가 같으면 
count를 이용해 수를 증가시키는 걸 만들려고 했지만 num[i]는 int형이고, arr[i]는 char형이기 때문에 형변환을 
해주었다. 