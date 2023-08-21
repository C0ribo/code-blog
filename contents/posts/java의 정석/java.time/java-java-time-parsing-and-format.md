---
title: "[Java] 파싱(parsing)과 포맷(format)"
description: "parsing, format"
date: 2023-08-21
update: 2023-08-21
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다.
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```



날짜와 시간을 원하는 형식으로 출력하고 해석(파싱, parsing)하는 방법

- 형식화에 관련된 java.time패키지 중 `DateTimeFormatter`가 핵심이다.

```java
LocalDate date = LocalDate.of(2016, 1, 2);
String yyyymmdd = DateTimeFormatter.ISO_LOCAL_DATE.format(date); 
// 2016-01-02
String yyyymmdd = date.format(DateTimeFormatter.ISO_LOCAL_DATE);
// 2016-01-02
```

- **format()** : <u>날짜와 시간 형식화에 사용</u>되는데, DateTimeFormatter뿐만 아니라 LocalDate와 LocalTime같은 클래스에도 있다.

| DateTimeFormatter | 설명 | 보기 |
| --- | --- | --- |
| `ISO_DATE_TIME` | Date and time with Zoneld | 2011-12-03T10:15:30+01:00[Europe/Paris] |
| `ISO_LOCAL_DATE` | ISO Local Date | 2011-12-03 |
| `ISO_LOCAL_TIME` | Time without offset | 10:15:30 |
| `ISO_LOCAL_DATE_TIME` | ISO Local Date and Time | 2011-12-03T10:15:30 |
| `ISO_OFFSET_DATE` | ISO Date with offset | 2011-12-03+01:00 |
| `ISO_OFFSET_TIME` | Time with offset | 10:15:30+01:00 |
| `ISO_OFFSET_DATE_TIME` | Date Time with Offset | 2011-12-03T10:15:30+01:00 |
| `ISO_ZONED_DATE_TIME` | Zoned Date Time | 2011-12-03T10:15:30+01:00[Europe/Paris] |
| `ISO_INSTANT` | Date and Time of an Instant | 2011-12-03T10:15:30Z |
| `BASIC_ISO_DATE` | Basic ISO date | 20111203 |
| `ISO_DATE` | ISO Date with or without offset | 2011-12-03+01:00<br>2011-12-03 |
| `ISO_TIME` | Time with or  without offset | 10:15:30+01:00<br>10:15:30 |
| `ISO_ORDINAL_DATE` | Year and day of year | 2012-337 |
| `ISO_WEEK_DATE` | Year and Week | 2012-W48-6 |
| `RFC_1123_DATE_TIME` | RFC 1123 / RFC 822 | Tue. 3 Jun 2008 11:05:30 GMT |

### 로케일에 종속된 형식화

DateTimeFormatter의 static메서드 `ofLocalizedDate()`, `ofLocalizedTime()`, `ofLocalized DateTime()`은 로케일(locale)에 종속적인 포맷터를 생성한다.

```java
DateTimeFormatter formatter = 
		DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
String shortFormat = formatter.format(LocalDate.now());
```

| FormatStyle | 날짜 | 시간 |
| --- | --- | --- |
| FULL | 2015년 11월 28일 토요일 | N/A |
| LONG | 2015년 11월 28일 (토) | 오후 9시 15분 13초 |
| MEDIUM | 2015.11.28 | 오후 9:15:13 |
| SHORT | 15.11.28 | 오후 9:15 |

### 출력형식 직접 정의하기

DateTimeFormatter의 `ofPattern()`으로 원하는 출력형식을 직접 작성할 수 있다.

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
```

| 기호 | 의미 | 보기 |
| --- | --- | --- |
| G | 연대(BC, AD) | 서기 또는 AD |
| y 또는 u | 년도 | 2015 |
| M 또는 L | 월(1~12 또는 1월~12월) | 11 |
| Q 또는 q | 분기(quarter) | 4 |
| w | 년의 몇 번째 주(1~53) | 48 |
| W | 월의 몇 번째 주(1~5) | 4 |
| D | 년의 몇 번째 일(1~366) | 332 |
| d | 월의 몇 번째 일(1~31) | 28 |
| F | 월의 몇 번째 요일(1~5) | 4 |
| E 또는 e | 요일 | 토 또는 7 |
| a | 오전/오후(AM, PM) | 오후 |
| H | 시간(0~23) | 22 |
| k | 시간(1~24) | 22 |
| K | 시간(0~11) | 10 |
| h | 시간(1~12) | 10 |
| m | 분(0~59) | 12 |
| s | 초(0~59) | 35 |
| S | 천분의 일초(0~999) | 7 |
| A | 천분의 일초(그날의 0시 0분 0초 부터의 시간) | 80263808 |
| n | 나노초(0~9999999999) | 475000000 |
| N | 나노초(그 날의 0시 0분 0초부터의 시간) | 810699920000000 |
| V | 시간대 ID(VV) | Asia/Seoul |
| z | 시간대(time-zone) 이름 | KST |
| O | 지역화된 zone-offset | GMT+9 |
| Z | zone-offset | +0900 |
| X 또는 x | zone-offset(Z는 +00:00의미) | +09 |
| '|  escape문자(특수문자 표현) | 없음 |

### 문자열을 날짜와 시간으로 파싱하기

- 문자열을 날짜 또는 시간으로 변환하면 static메서드 `parse()`를 사용

```java
static LocalDateTime parse(CharSequence text)
static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter)
```

- DateTimeFormatter에 상수로 정의된 형식으로 사용할 경우

```java
LocalDate date = 
		LocalDate.parse("2016-01-02", DateTimeFormatter.ISO_LOCAL_DATE);
```

- 자주 사용되는 기본적인 형식 문자열은 `ISO_LOCAL_DATE`와 같은 형식화 상수를 사용하지 않고 파싱 가능하다.

```java
LocalDate newDate = LocalDate.parse("2001-01-01");
LocalTime newTime = LocalTime.parse("23:59:59");
LocalDateTime newDateTime = LocalDateTime.parse("2001-01-01T23:59:59");
```

- `ofPattern()`을 이용해 파싱할 수 있다.

```java
DataTimeFormatter pattern = 
		DateTimeFormatter.ofPattern("yyyy-mm-dd HH:mm:ss");
LocalDateTime endOfYear =
		LocalDateTime.parse("2015-12-31 23:59:59", pattern);
```

## 참고

- 자바의 정석