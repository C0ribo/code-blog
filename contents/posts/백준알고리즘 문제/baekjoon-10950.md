---
title: "[Java] 백준 10950 알고리즘 문제"
description: "java, 브론즈5"
date: 2023-09-05
update: 2023-09-05
tags:
  - java
  - 알고리즘
  - 백준
series: "백준 알고리즘"
---

## 글을 쓰게 된 계기

알고리즘을 시작한지 얼마안됐다. 문제를 푸는데 아직 내게 어려운 부분이 많았다. 그러다가 StringBuilder를 사용해서 풀어야한다는 것을 알게 되었고, StringBuilder를 사용한 것과 사용하지 않은 것을 기록해보려고 한다. 

문제는 이 문제 : https://www.acmicpc.net/problem/10950 이고 내가 쓴 코드가 무조건 맞다는 것은 아닌 다른 방법으로도 충분히 풀 수 있다. 


### StringBuilder 사용하지 않았을 경우

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon10950 {
    public static void main(String[] args) throws IOException {
        /*
        * (1) 몇개 할건지 먼저 입력
        * (2) A, B 입력
        * (3) A + B 각각 출력
        * */
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int count = Integer.parseInt(br.readLine());

        for(int i = 1; i <= count; i++) {
            String str = br.readLine();
            StringTokenizer st = new StringTokenizer(str, " ");
            int num1 = Integer.parseInt(st.nextToken());
            int num2 = Integer.parseInt(st.nextToken());
            //int result = num1 + num2;
            System.out.println(num1 + num2);
        }
    }
}

```

결과값이 나오긴 했다만 사실상 매우 불안정한 코드이다. 백준에 나와있는 예제 입력을 복사했을 때 붙이면 결과값은 나오지만 깔끔하지 못하고, 다른 숫자를 입력했을 때 이상하게 결과값이 나오긴했다. 하지만 일단은 백준에서는 이 코드를 제출했을 때 정답이었다고 뜨긴했다. 

![](https://github.com/C0ribo/code-blog/assets/133131980/06dac51c-d08c-4d56-ba8f-0bca10123e3e)


### StringBuilder를 사용할 경우

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon10950_2 {
    public static void main(String[] args) throws IOException {
        /*
        * StringBuilder 사용
        * */
        BufferedReader br = new BufferedReader((new InputStreamReader(System.in)));
        int input = Integer.parseInt(br.readLine());

        StringBuilder sb = new StringBuilder();
        StringTokenizer st;

        for(int i = 1; i <= input; i++){
            st = new StringTokenizer(br.readLine(), " ");

            int num1 = Integer.parseInt(st.nextToken());
            int num2 = Integer.parseInt(st.nextToken());
            int result = num1 + num2;

            sb.append(result);
            sb.append("\n");
        }
        System.out.println(sb);
    }
}

```

StringBuilder를 사용했을 경우, 아직 사용하는 법이 매우 서툴다. 이게 맞나? 싶을 정도이다. 
확실히 StringBuilder를 사용하면 아래의 이미지처럼 깔끔한 결과가 나오고 또한 다른 숫자를 입력했을때도 제대로 나온다는 것이다. 

!()[https://github.com/C0ribo/code-blog/assets/133131980/f876c278-7013-4cc4-a2d2-b0aa1441715d]