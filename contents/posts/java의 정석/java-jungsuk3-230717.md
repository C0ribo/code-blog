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
6. year%400 == 0 || (year%4 == 0 && year%100 != 0)
7. !powerOn
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
        System.out.println("sum:"+ sum); // 73
    }
```

### [4-3] 1+(1+2)+(1+2+3)+(1+2+3+4)+...+(1+2+3+...+10)의 결과를 계산하시오.
```java
public static void main(String[] args) {
        //int num = 0;
        int sum = 0;

        for(int i =1; i<= 10; i++){
            for(int j = 1; j<=i; j++){
                sum += j;
            }
        }
        System.out.print(sum); // 220
    }
```

### [4-4] 1+(-2)+3+(-4)+... 과 같은 식으로 계속 더해나갔을 때, 몇까지 더해야 총합이 100이상이 되는지 구하시오.
```java
public static void main(String[] args) {
        int sum = 0;
        int i = 0;
        
        while(true) { // 무한이 진행될때
            i++;
           if(i%2 == 0){ // i가 짝수일때 
               sum -= i; //sum = sum - i , 
           } else { // i가 홀수 일때 
               sum += i; // sum = sum + i
           }
           if(sum >= 100){
               break;
           }
        }
        System.out.println("num : " +i); // 199
    }
```

### [4-5] 다음의 for문을 while문으로 변경하시오.
```java
public class Exercise4_5 {
    public static void main(String[] args){
        for(int i=0; i<= 10; i++){
            for(int j = 0; j<=i; j++)
                System.out.print("*");
            System.out.println();
        }
    }
}
```

```java
public static void main(String[] args){
        int i = 0;
        while(i<=10){ // i가 10 이하일때 참일 경우
            int j = 0;
            while(j <= i){
                System.out.print("*");
                j++;
            }
            i++;
            System.out.println();
        }
    }
```

> 중첩 for문은 중첩 while문으로 변경할 수 있다.

### [4-6] 두 개의 주사위를 던졌을 때, 눈의 합이 6이 되는 모든 경우의 수를 출력하는 프로그램을 작성하시오.
```java
public static void main(String[] args){
        for(int i=1; i<= 6; i++){ //주사위 1부터 6
            for(int j =1; j <= 6; j++){
                if(i+ j == 6)
                    System.out.printf("[%d, %d]", i, j);
            }
            System.out.println();
        }
    }
```

### [4-7] Math.random()을 이용해서 1부터 6사이의 임의의 정수를 변수 value에 저장하는 코드를 완성해라. (1)에 알맞은 코드를 넣으시오.
```java
class Exercise4_7 {
    public static void main(String[] args){
        int value = ( /* (1) */);
        System.out.prinln("Value:"+value);
    }
}
```

```java
int value = (int)(Math.random() * 6) + 1;
```

### [4-8] 방정식 2x+4y=10의 모든 해를 구하시오. 단, x와 y는 정수이고 각각의 범위는 0<=x<=10, 0<=y<=10 이다.
```java
[실행 결과]
x=1, y=2
x=3, y=1
x=5, y=0
```
```java
public static void main(String[] args){
        for(int x = 0; x<= 10; x++){
            for(int y = 0; y<= 10; y++){
                if((2 * x)+(4*y) == 10){
                    System.out.printf("x=%d, y=%d ", x, y);
                }
            }
            System.out.println();
        }
    }
```

### [4-9] 숫자로 이루어진 문자열 str이 있을 때, 각 자리의 합을 더한 결과를 출력하는 코드를 완성하라. 만일 문자열이 "12345"라면, "1+2+3+4+5"의 결과인 15를 출력이 되야 한다. (1)에 알맞은 코드를 넣으시오.
```java
class Exercise4_9 {
    public static void main(String[] args){
        String str = "12345";
        int sum = 0;
        for(int i=0; i < str.length(); i++){
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
        System.out.println("sum="+sum);
    }
}
```
```java
[실행결과]
15
```

```java
sum += (str.charAt(i) - '0');
```
> **String -> char 형 변환**
: str.charAt(i)   
 **char -> int 형 변환**
: str.charAt(i)-'0'

### [4-10] int타입의 변수 num이 있을 때, 각 자리의 합을 더한 결과를 출력하는 코드를 완성하라. 만일 변수 num의 값이 12345라면, '1+2+3+4+5'의 결과인 15를 출력하라. (1)에 알맞은 코드를 넣으시오.
```java
class Exercise4_10 {
    public static void main(String[] args){
        int num = 12345;
        int sum = 0;
        /*
            (1) 알맞은 코드를 넣어 완성하시오.
        */
       System.out.println("sum="+sum);
    }
}
```
```java
while(num != 0){
            sum += num%10; // sum = sum + num%10
            num /= 10; // num을 10으로 나눈 값을 다시 num에 저장
        }
```

### [4-11] 피보나치 수열은 앞을 두 수를 더해서 다음 수를 만들어 나가는 수열이다. 예를 들어 앞의 두 수가 1과 1이라면 그 다음 수는 2가 되고 그 다음 수는 1과 2를 더해서 3이 되어서 1, 2, 3, 5, 8, 13, 21,.. 과 같은 식으로 진행된다. 1과 1부터 시작하는 피보나치수열의 10번째 수는 무엇인지 계산하는 프로그램을 완성하시오.
```java
public class Exercise4_11{
    public static void main(String[] args){
        // 피보나치 수열의 시작의 첫 두 숫자를 1, 1로 한다.
        int num1 = 1;
        int num2 = 1;
        int num3 = 0;
        System.out.print(num1+","+num2);

        for(int i=0; i< 8; i++){
            /*
                (1) 알맞은 코드를 넣어 완성하시오.
            */
        }
    }
}
```
```java
[실행 결과]
1, 1, 2, 3, 5, 8, 13, 21, 34, 55
```

```java
num3 = num1 + num2;
System.out.print(","+num3);
num1 = num2;
num2 = num3;
```

### [4-12] 구구단의 일부분을 다음과 같이 출력하시오.
```java
[실행 결과]
2 * 1 = 2   3 * 1 = 3   4 * 1 = 4
2 * 2 = 4   3 * 2 = 6   4 * 2 = 8
2 * 3 = 6   3 * 3 = 9   4 * 3 = 12

5 * 1 = 5   6 * 1 = 6   7 * 1 = 7
5 * 2 = 10  6 * 2 = 12  7 * 2 = 14
5 * 3 = 15  6 * 3 = 18  7 * 3 = 21

8 * 1 = 8   9 * 1 = 9
8 * 2 = 16  9 * 2 = 18
8 * 3 = 24  9 * 3 = 27
```
