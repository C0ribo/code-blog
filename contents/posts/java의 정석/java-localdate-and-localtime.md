---
title: "[Java] LocalDate와 LocalTime"
description: "now(), of(), LocalDate, LocalTime, get(), TemporalField"
date: 2023-08-18
update: 2023-08-18
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

LocalDate와 LocalTime은 java.time패키지의 가장 기본 클래스이다.

### 객체 생성방법

- **now()** : 현재의 날짜와 시간을 LocalDate와 LocalTime으로 각각 반환

- **of()** : 지정된 날짜와 시간으로 LocalDate와 LocalTime객체를 생성

- 둘 다 **static메서드**이다.

```java
LocalDate today = LocalDate.now(); // 오늘 날짜
LocalTime now = LocalTime.now(); // 현재 시간

LocalDate birthDate = LocalDate.of(1999, 12, 31); // 1999년 12월 31일
LocalTime birthTime = LocalTime.of(23,59, 59); // 23시 59분 59초
```

`of()메서드`는 다양한 형태로 오버로딩되어 제공된다.

```java
static LocalDate of(int year, Month month, int dayOfMonth)
static LocalDate of(int year, int month, int dayOfMonth)

static LocalTime of(int hour, int min)
static LocalTime of(int hour, int min, int sec)
static LocalTime of(int hour, int min, int sec, int nanoOfSecond)
```

> 일 단위나 초 단위로 지정가능하다.
<br>1999년의 365번째 날 → 1999년 12월 31일
<br>86399초 → 23시 59분 59초
> 

`parse()`를 이용해 문자열을 날짜와 시간으로 변환가능하다.

```java
LocalDate birthDate = LocalDate.parse("1998-10-22");
// 1999년 12월 31일
LocalTime birthTime = LocalTime.parse("23:59:59");
// 23시 59분 59초
```

### 특정 필드의 값 가져오기 get(), getXXX()

- Calender : 월 0~11(0, 1월), 요일 1~7(1, 일요일)

- java.time패키지 : 월(Month) 1~12, 요일 1~7(1, 월요일)

#### **LocalDate&LocalTime 클래스에서 제공하는 대표적인 getter메서드**

| 클래스 | 메서드 | 설명(1999-12-31 23:59:59) |
| --- | --- | --- |
| **LocalDate** | int get(TemporalField field)<br>long getLong(TemporalField field) | 해당 날짜, 시간 객체의 명시된 필드의 값을 int형이나 long형으로 반환 |
|  | int getYear() | 해당 날짜 객체의 연도(YEAR)필드의 값 반환 |
|  | int getMonthValue() | 해당 날짜 객체의 월(MONTH_ OF_YEAR) 필드의 값을 반환함. (1~12) |
|  | Month getMonth() | 해당 날짜 객체의 월(MONTH_ OF_YEAR) 필드의 값을 Month 열거체를 이용하여 반환함. |
|  | int getDayOfMonth() | 해당 날짜 객체의 일(DAY_ OF_MONTH) 필드의 값을 반환함. (1~31) |
|  | int getDayOfYear() | 해당 날짜 객체의 일(DAY_ OF_YEAR) 필드의 값을 반환함. (1~365, 윤년이면 366) |
|  | DayOfWeek getDayOfWeek() | 해당 날짜 객체의 요일(DAY_ OF_WEEK) 필드의 값을 DayOfWeek 열거체를 이용하여 반환함. |
|  | int lengthOfMonth() | 같은 달의 총 일수(31) |
|  | int lengthOfYear() | 같은 해의 총 일수(365), 윤년이면 366 |
|  | boolean isLeapYear() | 윤년 여부 확인(false) |
| **LocalTime** | int getHour() | 해당 시간 객체의 시(HOUR_ OF_DAY) 필드의 값을 반환함. |
|  | int getMinute() | 해당 시간 객체의 분(MINUTE_OF_ HOUR) 필드의 값을 반환함. |
|  | int getSecond() | 해당 시간 객체의 초(SECOND_ OF_ MINUTE) 필드의 값을 반환함. |
|  | int getNano() | 해당 시간 객체의 나노초(NANO_ OF_ SECOND) 필드의 값을 반환함. |

#### **TemporaField 인터페이스**

월(month-of-year)과 시(hour-of-day)와 같이 날짜와 시간과 관련된 필드를 정의해놓은 인터페이스이다.

| TemporaField(ChronoField) | 설명 |
| --- | --- |
| ERA | 시대 |
| YEAR_ OF_ERA, YEAR | 연도 |
| MONTH_ OF_YEAR | 월 |
| DAY_ OF_WEEK | 요일(1:월,..,7:일) |
| DAY_ OF_MONTH | 일 |
| AMPM_ OF_DAY | 오전/오후 |
| HOUR_ OF_DAY | 시간(0~23), 밤 12시 → 0 |
| CLOCK_ HOUR_ OF_DAY | 시간(1~24), 밤 12시 → 24 |
| HOUR_ OF_AMPM | 시간(0~11) |
| CLOCK_ HOUR_ OF_AMPM | 시간(1~12) |
| MINUTE_ OF_HOUR | 분 |
| SECOND_ OF_MINUTE | 초 |
| MILLI_ OF_SECOND | 천분의 일초(10^-3초) |
| MICRO_ OF_SECOND | 백만분의 일초(10^-6초) |
| NANO_ OF_SECOND | 10억분의 일초(10^-9초) |
| DAY_ OF_YEAR | 해당 연도의 몇 번째 날(1~365, 윤년 366) |
| EPOCH_DAY | EPOCH(1970년 1월 1일)을 기준으로 몇 번째 날 |
| MINUTE_ OF_DAY | 그 날의 몇 번째 분(시간을 분으로 환산) |
| SECOND_ OF_DAY | 그 날의 몇 번째 초(시간을 초로 환산) |
| MILLI_ OF_DAY | 그 날의 몇 번째 밀리초(10^-3초) |
| MICRO_ OF_DAY | 그 날의 몇 번째 마이크로초(10^-6초) |
| NANO_ OF_DAY | 그 날의 몇 번째 나노초(10^-9초) |
| ALIGNED _ WEEK_OF_MONTH | 그 달의 n번째 주(1~7일 1주, 8~14일 2주,..) |
| ALIGNED_ WEEK_ OF_YEAR | 그 해의 n번째 주(1월 1~7일 1주, 8~14일 2주,..) |
| ALIGNED_ DAY_OF _WEEK _IN _MONTH | 요일(그 달의 1일을 월요일로 간주해 계산) |
| ALIGNED_DAY _OF _WEEK _IN _YEAR | 요일(그 해의 1월 1일을 월요일로 간주해 계산) |
| INSTANT_SECONDS | 년월일을 초단위로 환산(1970-01-01 00:00:00 UTC를 0초로 계산) Instant에만 사용가능 |
| OFFSET_SECONDS | UTC와 시차, ZoneOffset에만 사용가능 |
| PROLEPTIC_MONTH | 년월을 월단위로 환산(2015년11월=2015*12+11) |

> 해당 클래스가 지원하지 않은 필드 사용 시, `UnsupportedTemporalTypeException` 발생
> 

특정 필드가 가질 수 있는 값 범위를 알고 싶을 경우 예제

```java
System.out.println(ChronoField.CLOCK_HOUR_OF_DAY.range()); // 1 - 24
System.out.println(ChronoField.HOUR_OF_DAY.range()); // 0 - 23
```

### 필드 값 변경 with(), plus(), minus()

#### with()

- 날짜와 시간에서 특정 필드 값 변경하기 위해 with()메서드를 사용
- with() 사용 시, 필드를 직접 지정가능

```java
date = date.withYear(2000); // 년도 2000년으로 변경
time = time.withHour(12); // 시간 12시로 변경
```

| 클래스 | 메서드 | 설명 |
| --- | --- | --- |
| LocalDate | LocalDate with(TemporalField field, long newValue) | 해당 날짜 객체에서 특정 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환 |
|  | LocalDate withYear(int year) | 해당 날짜 객체에서 연도(YEAR) 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환 |
|  | LocalDate withMonth(int month) | 해당 날짜 객체에서 월(MONTH_ OF_YEAR) 필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환 |
|  | LocalDate withDayOfMonth(int dayOfMonth) | 해당 날짜 객체에서 일(DAY_ OF_MONTH) 필드를 전달된 새로운 값으로 설정된 새로운 날짜 객체를 반환 |
|  | LocalDate withDayOfYear(int dayOfYear) | 해당 날짜 객체에서 DAY_ OF_YEAR필드를 전달된 새로운 값으로 설정한 새로운 날짜 객체를 반환 |
| LocalTime | LocalTime with(TemporalField field, long newValue) | 해당 시간 객체에서 특정 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환 |
|  | LocalTime withHour(int hour) | 해당 시간 객체에서 시(HOUR_ OF_DAY) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환 |
|  | LocalTime withMinute(int minute) | 헤당 시간 객체에서 분(MINUTE_ OF_HOUR) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환 |
|  | LocalTime withSecond(int second) | 해당 시간 객체에서 초(SECOND_ OF_MINUTE) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환 |
|  | LocalTime withNano(int nanoOfSecond) | 해당 시간 객체에서 나노초(NANO_ OF_SECOND) 필드를 전달된 새로운 값으로 설정한 새로운 시간 객체를 반환 |

#### plus()

특정 필드에 값을 더한다.

```java
LocalTime plus(TemporalAmount amountToAdd)
LocalTime plus(long amountToAdd, TemporalUnit unit)

LocalDate plus(TemporalAmount amountToAdd)
LocalDate plus(long amountToAdd, TemporalUnit unit)
```

`plus()`로 만든 메서드

```java
LocalDate plusYears(long yearsToAdd)
LocalDate plusMonths(long monthsToAdd)
LocalDate plusDays(long daysToAdd)
LocalDate plusWeeks(long weeksToAdd)

LocalTime plusHours(long hoursToAdd)
LocalTime plusMinutes(long minutesToAdd)
LocalTime plusSeconds(long secondsToAdd)
LocalTime plusNanos(long nanosToAdd)
```

#### minus()

`minus()메서드`는 특정 필드에 값을 빼는 메서드로, `plus()메서드`와 동일하다

> LocalDate에는 `truncatedTo()`가 없는데, LocalDate의 필드인 **년, 월, 일**은 0이 될 수 없기 때문이다.
> 

#### **열거형 ChronoUnit에 정의된 상수 목록**

| TemporalUnit(ChronoUnit) | 설명 |
| --- | --- |
| FOREVER | Long.MAX_VALUE초(약 3천억년) |
| ERAS | 1,000,000,000년 |
| MILLENNIA | 1,000년 |
| CENTURIES | 100년 |
| DECADES | 10년 |
| YEARS | 년 |
| MONTHS | 월 |
| WEEKS | 주 |
| DAYS | 일 |
| HALF_DAYS | 반나절 |
| HOURS | 시 |
| MINUTES | 분 |
| SECONDS | 초 |
| MILLIS | 천분의 일초(10^-3) |
| MICROS | 백만분의 일초(10^-6) |
| NANOS | 10억분의 일초(10^-9) |

### 날짜와 시간의 비교 isAfter(), isBefore(), isEquals()

LocalDate와 LocalTime도 `compareTo()`가 오버라이딩되어 있어 비교할 수 있다.

```java
int result = date1.compareTo(date2); // 같으면 0, date1이 이전이면 -1, 이후 1
```

하지만 `compareTo()`보다 편한 메서드가 제공된다.

```java
boolean isAfter(ChronoLocalDate other)
boolean isBefore(ChronoLocalDate other)
boolean isEqual(ChronoLocalDate other) // LocalDate에만 있음
```

1. `isEqual()`을 제공된 이유는 연표가 다른 두 날짜를 비교하기 위해서이다. LocalDate 클래스에만 제공한다. `equals()`은 모든 필드가 일치해야하지만, `isEqual()`은 날짜만 비교한다.

```java
LocalDate kDate = LocalDate.of(1999, 12, 31);
JapaneseDate jDate = JapaneseDate.of(1999, 12, 31);

System.out.println(kDate.equals(jDate)); // false, YEAR_OF_ERA가 다름
System.out.println(kDate.isEqual(jDate)); // true
```

1. **isBefore()메서드** : 두 개의 날짜와 시간 객체를 비교하여 현재 객체가 명시된 객체보다 앞선 시간인지를 비교한다.
2. **isAfter()메서드** : 두 개의 날짜와 시간 객체를 비교하여 현재 객체가 명시된 객체보다 늦은 시간인지를 비교한다.

## 참조

- 자바의 정석