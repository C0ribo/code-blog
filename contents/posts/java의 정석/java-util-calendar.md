---
title: "[Java] Date 클래스"
description: "Calendar클래스, getInstance(), Date클래스, add(), roll() "
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
|`YEAR` | 년 |
|`MONTH` | 월(0~11)<br>0 -> 1월, 11 -> 12월 |
|`WEEK_OF_YEAR` | 그 해의 몇번째 주 |
|`WEEK_OF_MONTH` | 그 달의 몇번째 주 |
|`DATE` | 일 |
|`DAY_OF_MONTH` | 그 달의 몇번째 일 |
|`DAY_OF_YEAR` | 그 해의 몇번째 일 |
|`DAY_OF_WEEK` 요일(1~7)<br>1 -> 일요일 |
|`DAY_OF_WEEK_IN_MONTH` | 그 달의 몇 번째 요일 |

**시간 필드**
| 필드명 | 설명 |
|--- | --- |
|`HOUR` | 시간(0~11) | 
|`HOUR_OF_DAY` | 시간(0~23) |
|`MINUTE` | 분 |
|`SECOND` | 초 |
|`MILLISECOND` | 1/1000초 |
|`ZONE_OFFSET` | GMT기준시차(-12~12) |
|`AM_PM` | 오전/오후 |

```java
getActualMaximum(Calendar.Date) // 이 달의 마지막 일
```

### 두 날짜 간의 차이

두 날짜를 최소단위인 초단위 변경 후 차이를 구한다.

```java
void set(int field, int value) // 해당 field를 value값으로
void set(int year, int month, int date) // 년, 월, 일
void set(int year, int month, int date, int hourOfDay, int minute) // 년, 월, 일, 시, 분
void set(int year, int month, int date, int hourOfDay, int minute, int second) // 년, 월, 일, 시, 분, 초
```

> `getTimeInMillis()` 초단위로 얻기 위해 **1000** 나눠야하고, 일단위를 얻기 위해 **24시간 * 60분 * 60초 * 1000** 으로 나눠야 한다.

### add() 메서드

add(int field, int amount)

- 지정 필드의 값을 원하는 만큼 증가 또는 감소
- 특정 날짜 또는 시간을 기점으로 일정기간 전후의 날짜와 시간을 알 수 있음
- 다른 필드에 영향을 줌
- 음수 사용 가능

```
날짜필드(Calendar.Date)값 31 증가 시 다음 달로 넘어가므로 월 필드(Calendar.Month)도 1증가
```

### roll() 메서드

roll(int field, int amount)

- 지정 필드의 값을 증가 또는 감소시키는 메서드
- 다른 필드에 영향을 주지 않음

```
월 필드의 값은 변하지 않고 일 필드의 값만 바뀜 
```

> 일 필드가 말 일(end of month)일 때, roll 메서드를 이용해서 월 필드를 변경하면 일 필드에 영향을 미칠 수 있다.<br> 3월 31일에서 Month -1 하면 2월 28일

다음 달의 1일에서 하루를 빼면 이번 달의 마지막 일을 알 수 있다.

### 날짜계산을 위한 메서드

```java
boolean isLeapYear(int year)
    // 매개변수 year가 윤년이면 true, 그렇지 않으면 false 반환
int dayDiff(int y1, int m1, int d1, int y2, int m2, int d2)
    // 두 날짜 간의 차이를 일단위로 반환
int getDayOfWeek(int year, int month, int day)
    // 지정된 날짜의 요일을 반환(1~7, 1이 일요일)
String convertDayToDate(int day)
    // 일단위의 값을 년월일의 형태의 문자열로 변환하여 반환
int convertDateToDay(int year, int month, int day)
    // 년월일을 입력받아서 일단위로 변환
```

- 두 날짜의 차이 : 일단위로 변환 뒤, 두 값을 빼줌
- 요일 구하기 : 일단위로 변환 후 요일의 개수인 7 나누고, 요일이 1부터 시작하기 떄문에 1을 더한다.
    - Calendar의 요일범위가 1~7이기 때문에 동일처리


## 참조

- 자바의 정석