---
title: "[Java] 예외처리(exception handling)"
description: "예외처리, try-catch"
date: 2023-08-09
update: 2023-08-09
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## 1.1 프로그램 오류

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이런 원인을 프로그램 에러 또는 오류라고 한다.

- **컴파일 에러(compile-time error)** : 컴파일 할 때 발생하는 에러
- **런타임 에러(runtime error)** : 프로그램의 실행도중에 발생하는 에러
- **논리적 에러(logical error)** : 컴파일도 잘되고 실행도 잘되지만 의도한 것과 다르게 동작하는 것

자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 에러(error)와 예외(exception) 두가지로 구분한다.

- 에러: 메모리 부족(outofMemoryError)이나 스택오버플로우(StackOverflowError)와 같이 일단 발생하면 복구할 수 없는 심각한 오류. 프로그램의 비정상적인 종료를 막을 길이 없다.
- 예외: 발생하더라도 수습될 수 있는 비교적 덜 심각한 것이다. 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다.

## 1.2 예외 클래스의 계층구조

자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다. 모든 클래스의 조상은 Object클래스이므로 Exception과 Error클래스 역시 Object클래스의 자손들이다.

![](https://github.com/C0ribo/code-blog/assets/133131980/396a2372-7864-40c3-8cba-9df73633d4af)

모든 예외의 최고 조상은 Exception클래스이다.

![](https://github.com/C0ribo/code-blog/assets/133131980/53ca1f9b-d829-4a3a-aeb6-e36a97958339)

- Exception클래스들 : RuntimeException클래스들을 제외한 나머지 클래스들. 주로 외부의 영향으로 발생할 수 있는 것들로서, 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많다.
    - FileNotFoundException : 존재하지 않은 파일의 이름입력
    - ClassNotFoundException : 실수로 클래스의 이름을 잘못적었을 경우
    - DataFormatException : 입력한 데이터 형식이 잘못됐을 경우
- RuntimeException클래스들 : RuntimeException클래스와 그 자식들. 프로그래머의 실수에 의해서 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.
    - ArrayIndexOutOfBoundsException : 배열의 범위를 벗어났을 경우
    - NullPointerException : 값이 null인 참조변수의 멤버를 호출하려 했을 경우
    - ClassCastException : 클래스 간의 형변환을 잘못했을 경우
    - ArithmeticException : 정수를 0으로 나누려고 하는 경우

## 1.3 예외처리하기 -try-catch문

**예외처리(exception handling)**

- 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것.
- 예외의 발생으로 인한 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것

예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되고, 처리되지 못한 예외(uncaught exception)는 JVM의 예외처리기(UncaughtExceptionHandler)가 받아서 예외의 원인을 화면에 출력한다.

try-catch문

```java
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception e1) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다
} catch (Exception e2) {
	// Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다
} catch (Exception eN) {
	// ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장을 적는다
}
```

- 하나의 try블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch블럭이 올 수 있다.
    - 발생한 예외의 종류와 일치하는 단 한개의 catch블럭만 수행된다
    - 발생한 예외의 종류와 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.
- try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

```java
class ExceptionExample {
	public static void main(String[] args){
		try {
				1. try블럭에 또 다른 try-catch문이 포함
				try {  } catch (Exception e) { }
		} catch (Exception **e**) { 
				3. catch블럭 내 선언된 변수는 catch블럭 내에서 유효하기
						때문에 참조변수 'e' 하나만 사용해도 된다.
				2. catch블럭에 또 다른 try-catch문이 포함
				try {  } catch (Exception **e**) { } 
				// 에러. 변수 e가 중복 선언되었다.
		} // try-catch의 끝

		try {

		} catch (Exception e) {

		} // try-catch의 끝
	} // main메서드의 끝
}
```

### ArithmeticException

산술연산과정에서 오류가 있을 때 발생하는 예외이며, 정수는 0으로 나누는 것이 금지되어 있어서 발생한다. 하지만 실수를 0으로 나누는 것은 금지되어 있지 않아서 예외가 발생하지 않는다.

```java
class ExceptionExample {
	public static void main(String[] args) {
		int number = 100;
		int result = 0;
		
		for(int i = 0; i < 10; i++) {
			try {
					result = number / (int)(Math.random() * 10);
					System.out.println(result);
			} catch (ArithmeticException e) {
					System.out.println("0");
					// ArithmeticException이 발생하면 실행되는 코드
			} 
		}
	}
}
```

```java
16
20
11
0  <- ArithmeticException이 발생해서 0이 출력되었다.
25
100
25
33
14
12
```

4번째 0이 출력된 것은 for문의 4번째 반복에서 ArithmeticException이 발생했기 때문이다. ArithmeticException에 해당하는 catch블럭을 찾아서 그 catch블럭 내의 문장들을 실행한 다음 try-catch문을 벗어나 for문의 다음 반복을 계속 수행하여 작업을 모두 마치고 정상적으로 종료되었다. 

만일 예외처리 하지 않았다면, 세 번줄 까지만 출력되고 예외가 발생해서 종료되었을 것이다.