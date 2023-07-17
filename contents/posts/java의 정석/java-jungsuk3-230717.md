---
title: "🤓 [Java] 1. 자바의 정석 연습문제 4단원 풀이"
description: "자바의 조건문과 반복문 연습문제 if, switch, for, while statement"
date: 2023-07-17
update: 2023-07-17
tags:
  - Java
  - 연습문제
series: "자바의 정석 3판 연습문제"
---

```
공부한지 얼마 안되서 틀릴 수도 있습니다! 참고 바랍니다.
누군가 보라고 쓴 글도 아니고 공부한걸 끄적이는 용입니다.
```

### [4-1] 다음의 문장들을 조건식으로 표현하라.

```
1. int형 변수 x가 10보다 크고 20보다 작을 때 true인 조건식
2. char형 변수 ch가 공백이나 탭이 아닐때 true인 조건식
3. char형 변수 ch가 'x' 또는 'X'일 때 ture인 조건식
4. char형 변수 ch가 숫자(‘0’~‘9’)일 때 true인 조건식
5. char형 변수 ch가 영문자(대문자 또는 소문자)일 때 true인 조건식
6. int형 변수 year가 400으로 나눠떨어지거나 또는 4로 나눠떨어지고 100으로 나눠떨어지지 않을 때 true인 조건식
7. boolean형 변수 powerOn가 false일 때 true인 조건식
8. 문자열 참조변수 str이 “yes”일 때 true인 조건식
```

### 풀이:

```java
1. 10 < x && x < 20
2. ch == ' ' || ch == '\t'
3. ch == 'x' || ch == 'X'
4. '0' <= ch && ch <= '9'
5. ch == 'a' && ch == 'z' || ch == 'A' && ch == 'Z'
6. (year%400 == 0 && year%4 == 0) && year%100 != 0
7. powerOn != false
8. str.equals("yes") // equals 대소문자 구분
   str.equalsIgnoreCase("yes") // equalsIgnoreCase 대소문자 구분안함
```

> #### 논리 연산자
>
> - **||(OR결합)** : 둘 중 하나만 참(true)이면 참이 된다.
> - **&&(AND결합)** : 두개 다 참(true)이면 참이 된다.

### [4-2] 1부터 20까지의 정수 중에서 2 또는 3의 배수가 아닌 수의 총합을 구하시오.

```java
public static void main(String[] args) {
        int sum = 0;
        for(int x=1; x<= 20; x++){ // 1부터 20까지의 정수
            if(x%2!=0 || x%3!=0) { // 2 또는(or) 3의 배수가 아닌 수
                sum += x;
            }
        }
        System.out.println("sum:"+ sum);
    }
```

### [4-3] 1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+...+10)의 결과를 계산하시오.
```kotlin
public static void main(String[] args) {
        //int num = 0;
        int sum = 0;

        for(int i =1; i<= 10; i++){
            for(int j = 1; j<=i; j++){
                sum += j;
            }
        }
        System.out.print(sum);
    }
```