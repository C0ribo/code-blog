---
title: "[Java] 예외처리(exception handling)"
description: "예외처리, try-catch, 멀티 catch, try-catch-finally"
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

## 자바 에러와 예외 클래스

### 프로그램 오류

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이런 원인을 프로그램 에러 또는 오류라고 한다.

- **컴파일 에러(compile-time error)** : 컴파일 할 때 발생하는 에러
- **런타임 에러(runtime error)** : 프로그램의 실행도중에 발생하는 에러
- **논리적 에러(logical error)** : 컴파일도 잘되고 실행도 잘되지만 의도한 것과 다르게 동작하는 것

자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 에러(error)와 예외(exception) 두가지로 구분한다.

- 에러: 메모리 부족(outofMemoryError)이나 스택오버플로우(StackOverflowError)와 같이 일단 발생하면 복구할 수 없는 심각한 오류. 프로그램의 비정상적인 종료를 막을 길이 없다.
- 예외: 발생하더라도 수습될 수 있는 비교적 덜 심각한 것이다. 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다.

### 예외 클래스의 계층구조

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

## 자바 예외 처리(try-catch) 문법

**예외처리(exception handling)**

- 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것.
- 예외의 발생으로 인한 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것

예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되고, 처리되지 못한 예외(uncaught exception)는 JVM의 예외처리기(UncaughtExceptionHandler)가 받아서 예외의 원인을 화면에 출력한다.

### -try-catch문


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

- catch문의 `Exception e`에서 Exception은 변수의 클래스 타입이고 e는 변수이다.
- 하나의 try블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch블럭이 올 수 있다.
    - 발생한 예외의 종류와 일치하는 단 한개의 catch블럭만 수행된다
    - 발생한 예외의 종류와 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.
- try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.
- catch 블럭에서 괄호() 내에서는 처리하고자 하는 예외와 같은 타입의 참조변수 하나를 선언해야한다.
- 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.

```java
class ExceptionExample {
	public static void main(String[] args){
		try {
				// try블럭에 또 다른 try-catch문이 포함
				try {  } catch (Exception e) { }
		} catch (Exception e) { 
				// catch블럭에 또 다른 try-catch문이 포함
				try {  } catch (Exception e) { } 
				// 에러. 변수 e가 중복 선언되었다.
		} // try-catch의 끝

		try {
			// ...
		} catch (Exception e) {
			// ...
		}
	}
}
```

catch 블럭의 괄호 내에 선언된 변수는 catch 블럭 내에서만 유효하기 때문에, e를 하나만 사용해도 된다.

- try블럭 내에서 예외가 발생한 경우, 
	1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
	2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다. 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.
- try블럭 내에서 예외가 발생하지 않은 경우,
	1. catch 블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.

### try-catch-finally

finally블럭은 예외의 발생여부에 상관없이 실행되어야 할 코드를 포함시킬 목적으로 사용된다.

```java
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception e1) {
	// 예외처리를 위한 문장을 적는다
} finally {
	// 예외의 발생여부에 관계없이 항상 수행되어야 하는 문장들을 넣는다.
	// finally블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```

- 예외가 발생할 경우 : `try -> catch -> finally` 순 실행
- 예외가 발생하지 않은 경우 : `try -> finally` 순 실행

```java
try {
	startInstall();	// 프로그램 설치에 필요한 준비를 한다.
	copyFiles();	// 파일들을 복사한다
	deleteTempFiles();	// 프로그램 설치에 사용된 임시파일들을 삭제한다.
} catch (Exception e){
	e.printStackTrace();
	deleteTempFiles();	// 프로그램 설치에 사용된 임시파일들을 삭제한다.
}

static void startInstall() { /* 프로그램 설치에 필요한 준비하는 코드를 적는다 */ }
static void copyFiles() { /* 파일들을 복사하는 코드를 적는다 */ }
static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다 */}
```

try블럭의 문장을 수행하는 동안에(프로그램을 설치하는 과정), 예외의 발생여부에 관계없이 deleteTempFiles()메서드는 실행되어야 한다. 

```java
try {
	startInstall();	// 프로그램 설치에 필요한 준비를 한다.
	copyFiles();	// 파일들을 복사한다
} catch (Exception e){
	e.printStackTrace();
} finally {
	deleteTempFiles();	// 프로그램 설치에 사용된 임시파일들을 삭제한다.
}

static void startInstall() { /* 프로그램 설치에 필요한 준비하는 코드를 적는다 */ }
static void copyFiles() { /* 파일들을 복사하는 코드를 적는다 */ }
static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다 */}
```

그리고 try블럭에서 return문이 실행되는 경우에도 finally블럭의 문장들이 먼저 실행되고, 현재 실행 중인 메서드를 종료한다.
catch블럭의 문장 수행 중에 return문을 만나도 finally블럭의 문장들은 수행된다.

### 멀티 catch문

- JDK 1.7부터 여러 catch블럭을 `|` 기호를 통해 하나의 catch 블럭으로 합칠 수 있다.
- 중복된 코드를 줄일 수 있고, 연결할 수 있는 예외 클래스 개수에 제한이 없다.

```java
try {
	...
} catch (ExceptionA | ExceptionB e){
	e.printStackTrace();
}
```

멀티 catch 블럭으로 연결된 예외 클래스가 부모와 자식의 관계에 있다면 컴파일 에러가 발생한다.

```java
try {
	...
} catch (ParentException | ChildException e) { // 에러! -> (ParentException e)
	e.printStackTrace();
}
```

- 두 예외 클래스가 부모-자식의 관계라면, 부모 클래스만 써주는 것과 똑같기 때문이다. 
	- 멀티 catch는 **하나의 catch블럭으로 여러 예외를 처리하는 것**이기에, 발생한 예외를 멀티 catch블럭으로 처리했을 경우, 실제로 어떤 예외가 발생한 건지 알 수 없다.
	- 참조변수 `e`로 멀티 catch블럭에 연결된 예외 클래스들의 공통 분모인 부모 예외 클래스에 선언된 멤버만 사용할 수 있다.
- 각 예외마다 세세하게 제어하고 싶으면 `instanceof`로 확인하고 써야 한다.

```java
try {
	...
} catch (ExceptionA | ExceptionB e){
	if(e instanceof ExceptionA){
		// ...
	} else {	// if(e instanceof ExceptionB)
		// ...
	}
}
```

### 예외 메시지 출력

catch 블럭의 괄호()에 선언된 참조변수를 통해 접근할 수 있으며, 이 참조변수는 선언된 catch 블럭 내에서만 사용이 가능하다.
- **printStackTrace()** : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
- **getMessage()** : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

```java
try {
	...
	System.out.println(0/0); // ArithmeticException 예외발생
	...
} catch (ArithmeticException e){
	// 상세한 에러 추적 메시지
	e.printStackTrace();
	// 에러 메시지
	System.out.println("예외메시지 : " + e.getMessage());
}
```

ArithmeticException인스턴스의 printStackTrace()를 사용해서, 호출스택(call stack)에 대한 정보와 예외 메시지를 출력할 수 있다.

### 예외 발생

throw룰 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있다.

1. 먼저, 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
	
	`Exception e = new Exception("고의로 발생시켰음");`

2. 키워드 throw를 이용해서 예외를 발생시킨다.

	`throw e;`

```java
try {
	Exception e = new Exception("고의로 발생시켰음");// 발생시키려는 예외클래스의 객체 만듬
	throw e;	// 예외를 발생시킴
	
	throw new Exception("고의로 발생시킴"); // 위 두줄 한줄로 줄인 것
}	catch (Exception e){
	// ...
}
```

#### 메서드에 예외 선언

- 메서드의 선언부에 `throws`를 사용해서 메서드 내에서 발생할 수 있는 예외를 적어준다.
- 여러 개알 경우 쉼표`,`로 구분한다.

```java
void method() throws Exception1, Exception2, ... ExceptionN {
	// 메서드 내용
}
```

> 예외를 발생시키는 throw와 예외를 메서드에 선언할 때 쓰는 throws 구분



## 자바 Try with Resource 예외 처리

## 예외 던지기(throw)와 연결된 예외(chained exception)