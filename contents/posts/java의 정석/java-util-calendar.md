---
title: "[Java] Date 클래스"
description: "Calendar클래스, getInstance(), Date클래스 "
date: 2023-08-16
update: 2023-08-16
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## Date와 Calendar

### Calendar

Calendar는 추상 클래스 -> 객체 생성X, 메서드를 통해 구현된 클래스의 인스턴스를 얻어야 한다.

```java
Calendar cal = new Calendar(); // 에러. 객체 생성X 

Calendar cal = new getCalendar(); 
// 자바의 getter, setter 느낌인 메서드를 통해 객체 반환
```

- getInstance() 메서드가  메서드 내 코드에서 인스턴스를 사용하거나 인스턴스 메서드를 호출하지 않기 때문에 static이다.
- getInstance()가 static이 아니라면 객체를 생성한 다음에 호출해야 하는데 Calendar는 추상이기에 객체 생성을 하지 않는다.

### Date와 Calendar간 변환

```java
1. Calendar -> Date

// Calendar.getInstance(): 사용자의 시스템 정보를 읽어와 자동으로 적합한 달력
Calendar cal = Calendar.getInstance();
...
// getTimeInMillis() : 해당 시간의 1/1000 초 단위 시간
Date d = new Date(cal.getTimeInMillis());

2. Date -> Calendar

Date d = new Date();
...
Calendar cal = Calendar.getInstance();
// Calendar 인스턴스 생성 후 setTime 메서드로 세팅
cal.setTime(d);
```
- getInstance()을 통해 얻은 인스턴스는 기본적으로 현재 시스템의 날짜와 시간에 대해 정보를 담고 있다.
- int get(int field) : 원하는 필드의 값을 얻어오는 방법을 보여주기 위한 것이다.

```java
public final static int YEAR = 1;
```

get 메서드의 매개변수로 사용되는 int값들은 Calendar에 정의된 static 상수이다. 

**날짜 필드**

| 필드명 | 설명 |
|--- | --- |
|YEAR | 년 |
|MONTH | 월(0~11)<br>0 -> 1월, 11 -> 12월 |
|WEEK_OF_YEAR | 그 해의 몇번째 주 |
|WEEK_OF_MONTH | 그 달의 몇번째 주 |
|DATE | 일 |
|DAY_OF_MONTH | 그 달의 몇번째 일 |
|DAY_OF_YEAR | 그 해의 몇번째 일 |
|DAY_OF_WEEK | 요일(1~7)<br>1 -> 일요일 |
|DAY_OF_WEEK_IN_MONTH | 그 달의 몇 번째 요일 |

**시간 필드**
| 필드명 | 설명 |
|--- | --- |
|HOUR | 시간(0~11) | 
|HOUR_OF_DAY | 시간(0~23) |
|MINUTE | 분 |
|SECOND | 초 |
|MILLISECOND | 1/1000초 |
|ZONE_OFFSET | GMT기준시차(-12~12) |
|AM_PM | 오전/오후 |

```java
getActualMaximum(Calendar.Date) // 이 달의 마지막 일
```