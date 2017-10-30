---
layout: post
title: "Kotlin in Action - Chapter 2"
description: "Kotlin in Action Chapter 2"
categories: [kotlin]
tags: [kotlin]
redirect_from:
  - /2017/09/27/
---

# Chapter 2. Kotlin basics
* 이번 장에서 다룰 내용
 * 함수, 변수, 클래스, enums, Properties 선언
 * 제어문
 * Smart casts
 * 예외 처리 

# 2.1. BASIC ELEMENTS: FUNCTIONS AND VARIABLES

## 2.1.1. Hello, world!

~~~ kotlin
fun main(args: Array<String>) {
	println("Hello, world!")
}
~~~

위 코드로 알 수 있는 것들...

* **fun** 키워드는 함수를 정의할 때 사용된다.
* 인자의 타입은 이름 뒤에 쓴다. **args: Array<String>**
* 함수는 파일의 최상위 레벨에 선언할 수 있다. (클래스 안에 넣지 않아도 된다.)
* 배열은 클래스이다. **Array**
* System.out.println 대신에 **println**을 쓴다. 코틀린 라이브러리에서 랩핑하고 있다.
* 라인 끝에 **;**을 생략한다.

## 2.1.2. Functions

함수의 리턴 타입은 어디에 써야하나..?

~~~ kotlin
fun max(a: Int, b: Int): Int {
	return if (a > b) a else b
}

>>> println(max(1, 2))
2
~~~

* **max** : 함수명
* **(a: Int, b: Int)** : 파라메터
* **: Int** : 리턴 타입
* **return if (a > b) a else b** 함수 body
* **if (a > b) a else b** 자바의 삼항식과 비슷하다.(코틀린에는 삼항식이 없다.)

> **Statements and Expression**
> 
> exression(표현식) : 값이 있는 코드, 값을 반환하는 코드, 다른 표현식의 부분으로 사용될 수 있다. side-effect가 없다.
> 
> statement(문장, 구문) : 실행되는 코드 단위, 그 자체로 값을 가지지 않는다. side-effectr가 있을 수도 있다.
> 
> 코틀린에서 if는 표현식이다.
> 
> java에서 assignment(할당)은 expression이지만 코틀린에서는 statement다.

~~~ kotlin
fun max(a: Int, b: Int) = if(a > b) a else b
~~~
* {} 와 return 구문을 생략하고 위처럼 함수를 정의 할 수 있다.
* 리턴 타입이 정의되어 있지만(파라메터 뒤에 :Int 가 없어도) 컴파일러는 **타입 추론**을 통해 리턴 타입을 알 수 있다.

## 2.1.3. Variables
자바에서 변수 선언은 타입이 먼저 온다. 코틀린에서는 대부분의 타입 선언을 생략할 수 있기 때문에 타입 선언은 뒤에 온다.

~~~ kotlin
val question =
    "The Ultimate Question of Life, the Universe, and Everything"
    
val answer = 42
val answer: Int = 42  // 타입을 명시적으로 쓸 수도 있다.

val yearsToCompute = 7.6e6  // Double형

val answer: Int  // 초기화 되지 않는 경우 타입을 명시적으로 써줘야 한다.
answer = 42
~~~

변수를 선언할때는 아래의 2개 키워드를 사용한다.

* val(from value) : 불변 참조. 변수가 한번 할당되고 나면 재할당 될 수 없다. java의 final
* var(from variable) : 가변 참조. 변수의 값이 변경 될 수 있다.

기본적으로, 변수 선언은 **val**로 하고 꼭 필요한 경우에만 **var**로 선언한다. 불변 변수, 객체, 함수를 사용하는 것은 함수형 스타일로 side-effect를 최소화 할 수 있다. 

