---
title: "🥸 6. 코틀린 루프"
description: "코틀린의 루프, while, do-while, for loop, Iterable, break, continue"
date: 2023-07-08
update: 2023-07-08
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## while과 do-while 루프

while과 do-while 루프는 자바와 같은 구조이다. 이 두 루프는 조건이 충족되는 동안 지속적으로 실행된다. 

### while

for 반복문이 범위를 정해서 하도록 강제한다면, while 반복문의 경우 **조건식이 참일 경우에만 반복한다.** 하지만 실행하기 전에 루프 실행 전 조건을 먼저 검사하고 처음부터 조건이 거짓이면 루프 조건이 한번도 실행되지 않는다.
```kotlin
while(조건식){
    // 반복 수행될 작업
}
```
예를 들어 1부터 10까지 출력한다고 했을때 마지막에 11까지 출력이 되는데 i의 깂이 10이 되더라도 조건식이 참이므로 반복 수행될 작업이 이루어지고 11에서 거짓이 되어 끝이 나는 것이다.
```kotlin
var i = 1
while(i <= 10){
    println(i)
    ++i
}
println(i)
```

while은 무한 반복을 하는 경우에 사용한다. 예를 들면 디지털시계나 채팅 프로그램의 채팅 메시지 수신처럼 어떤 대상을 반복적으로 검사할 때 사용한다. 
```kotlin
while(true){
    // 반복 수행될 작업
}
```

### do-while
앞의 while과 다르게 do-while의 경우에는 조건이 달라도 최소 한번은 실행하야 할 때 사용한다. 
```kotlin
do {
    // 반복 수행될 작업
} while(조건식)
```
사용자로부터 어떤 입력을 받았을 때 입력으로 특정한 문자열이 들어오면 종료하는 경우에 do-while 구문을 이용한다.


## for루프와 이터러블
for루프는 자바 for-each와 비슷하다. 코틀린의 모든 루프는 식이 아니고 문이기 때문에 어떤 값으로 평가되지 않으며 부수 효과를 발생시킬 수만 있다.

### for loop
for루프를 사용하면 컬렉션과 비슷하게 여러 값이 들어있을 수 있는 값에 대한 루프를 수행할 수 있다. 
코틀린의 경우 자바 for루프 `for(int i = 0; i< length; i++)`에 해당하는 루프가 없어서 while 루프로 표현하거나 범위 `..`와 진행을 사용하는 for 루프를 사용한다.

`..`의 경우 우항을 포함하는데 포함하고 싶지 않으면 `until`을 사용한다.
```kotlin
for(x in 0 until size) == for(x in 0..size - 1)
```

```kotlin
for(bar in (min)..(max)){
    // commands
}
```
bar는 for 구문이 반복되며 증감하는 값을 가지는 변수이다. 루프 변수에서는 val이나 var를 붙이지 않고 자동으로 불변 값이 된다. for문에서도 반복하는 명령문이 하나라면 다음과 같이 중괄호를 제거할 수 있다.
```kotlin
for(i in 1..10) println(i)
```
자바에서는 문자열의 각 문자에 대해 루프를 직접 수행할 수 없다. 

### Iterable
Iterable은 컬렉션(List, Set, Map)에 저장되어 있는 것을 일관성있게 꺼내 오는 방법이다. Iterator() 함수를 호출하여 Iterable 인터페이스의 상속자에 대해 Iterator를 얻을 수 있다.

**자세한 것은 컬렉션 단원에서 나온다**

## 제어 흐름(control flow)

### break

### continue