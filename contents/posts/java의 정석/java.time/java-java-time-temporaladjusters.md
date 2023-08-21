---
title: "[Java] TemporalAdjusters"
description: "TemporalAdjusters"
date: 2023-08-20
update: 2023-08-20
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```


자주 쓰일만한 날짜 계산들을 대신 해주는 메서드를 정의한 것이 `TemporalAdjusters`클래스이다.

```java
LocalDate today = LocalDate.now();
// 다음 주 월요일의 날짜를 계산
LocalDate nextMonday = 
					today.with(TemporalAdjusters.next(DayOfWeek.MONDAY));
```

| 메서드 | 설명 |
| --- | --- |
| firstDayOfNextYear() | 다음 해의 첫 날 |
| firstDayOfNextMonth() | 다음 달의 첫 날 |
| firstDayOfYear() | 올 해의 첫 날 |
| firstDayOfMonth() | 이번 달의 첫 날 |
| lastDayOfYear() | 올 해의 마지막 날 |
| lastDayOfMonth() | 이번 달의 마지막 날 |
| firstInMonth (DayOfWeek dayOfWeek) | 이번 달의 첫 번째 ?요일 |
| lastInMonth  (DayOfWeek dayOfWeek) | 이번 달의 마지막 ?요일 |
| previous     (DayOfWeek dayOfWeek) | 지난 ?요일(당일 미포함) |
| previousOrSame (DayOfWeek dayOfWeek) | 지난 ?요일(당일 포함) |
| next  (DayOfWeek dayOfWeek) | 다음 ?요일(당일 미포함) |
| nextOrSame (DayOfWeek dayOfWeek) | 다음 ?요일(당일 포함) |
| dayOfWeekInMonth(int ordinal, DayOfWeek datOfWeek) | 이번 달의 n번째 ?요일 |

## TemporalAdjuster 직접 구현하기

- 필요 시 자주 사용되는 날짜계산을 해주는 메서드를 직접 만들 수 있다.

- `with()`을 사용하며, TemporalAdjuster인터페이스를 구현한 클래스의 객체를 매개변수로 제공되어야 한다.

```java
LocalDate with(TemporalAdjuster adjuster)
```

- with()는 **LocalTime, LocalDateTime, ZonedDateTime, Instant** 등 대부분의 날짜와 시간에 관련된 클래스에 포함
- TemporalAdjuster인터페이스는 **추상 메서드 하나만 정의**되어 있으며, 이 메서드만 구현하면 된다.

```java
@FunctionalInterface
public interface TemporalAdjuster {
	Temporal adjustInto(Temporal temporal);
}
```

- 실제 구현은 `adjustInto()`지만, TemporalAdjuster와 같이 사용하는 메서드는 `with()`이다.
- `adjustInto()`는 내부적으로 사용할 의도이므로 `with()`을 사용한다
- 다만, 날짜와 시간에 관련된 대부분의 클래스는 Temporal인터페이스를 구현하였으므로 `adjustInto()`의 매개변수가 될 수 있다.

```java
// 특정 날짜로부터 2일 후 날짜를 계산하는 DayAfterTomorrow
class DayAfterTomorrow implements TeporalAdjuster {
		@Override
		public Temporal adjustInto(Temporal temporal) {
				return temporal.plus(2, ChronoUnit.DAYS); // 2일 더한다
		}
}
```

## 참조

- 자바의 정석