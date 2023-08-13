---
title: "[Java] java.util.StringTokenizer"
description: "StringTokenizer"
date: 2023-08-13
update: 2023-08-13
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## StringTokenizer 생성자

| 생성자 | 설명 |
| --- | --- |
|StringTokenizer(String s)| 매개변수로 전달받은 문자열에 대해 문자열 토큰을 생성. <br> 문자열 구분하는 기호 탭(\t), 줄바꿈(\n), 캐리지 리턴(\r), 폼 피트(\f) |
|StringTokenizer(String s, String delim) | 문자열(s)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성. |
|StringTokenizer(String s, String delim, boolean returnDelims)| 문자열(s)을 지정된 구분된(delim)로 나누는 StringTokenizer를 생성.<br>returnDelims의 값을 true로 하면 구분자도 토큰으로 간주된다. |

## StringTokenizer 메서드

| 메서드 | 설명 |
| --- | --- |
| int countTokens() | 전체 토큰의 수를 반환한다 |
| boolean hasMoreTokens() | 토큰이 남아있는지 알려준다<br>토큰이 존재 시 true, 토큰이 없으면 false 반환 | 
| boolean hasMoreElements() | 내부적으로 hasMoreTokens() 메서드를 호출하므로 동작 방식으로 반환 결과는 hasMoreTokens() 메서드와 동일하다. |
| String nextToken() | 사용 가능한 다음 토큰을 문자열로 반환한다 | 
| String nextToken(String delim) | 매개변수로 전달받은 구분 기호를 기준으로 분활된 다음 토큰을 문자열로 반환한다 |
|Object nextElement() | 내부적으로 nextToken() 메서드를 호출하지만, 반환 타입이 Object 라는 차이점이 있다. | 