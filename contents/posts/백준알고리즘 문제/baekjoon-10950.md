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
        //String str = br.readLine();
        //StringTokenizer st = new StringTokenizer(str, " ");

        //int num1 = Integer.parseInt(st.nextToken());
        //int num2 = Integer.parseInt(st.nextToken());
        //int result;

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