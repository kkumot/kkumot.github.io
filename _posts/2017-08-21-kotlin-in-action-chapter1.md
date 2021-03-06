---
layout: post
title: "Kotlin in Action - Chapter 1"
description: "Kotlin in Action Chapter 1"
categories: [kotlin]
tags: [kotlin]
redirect_from:
  - /2017/08/21/
---

# Chapter 1. Kotlin : what and why
* 자바 플랫폼을 대상으로 하는 새로운 프로그래밍 언어.
* concise, safe, pragmatic, 자바 코드와 상호 호환 됨.
* 자바가 사용되는 거의 모든 곳에 쓸수 있음 : server-side, Android App...
* 기존에 존재하는 자바 라이브러리, 프레임웍에서 잘 동작하고 자바와 동일한 성능으로 실행됨.


# 1.1 A Taste of Kotlin 
* [http://try.kotl.in](http://try.kotl.in) : 코틀린 코드 실행 가능한 페이지

~~~ kotlin
// "data" class
data class Person(val name: String,
                 val age: Int? = null) // Nullable type(Int?)

// top-level function
fun main(args: Array<String>) {
	val persons = listOf(Person("Alice"), Person("Bob", age = 29),
                        Person("Tom", 35), Person("Kkumot", 18))
    val oldset = persons.maxBy { it.age ?: 0 } // Lambda expression, Elvis operator
    println("The oldset is: $oldset") // string template
}

// The oldset is: Person(name=Tom, age=35)  <-- autogenerated toString
~~~
* [Elvis operator](https://en.wikipedia.org/wiki/Elvis_operator#cite_note-1)  
      
# 1.2 주요 특징

## 1.2.1 어디에 쓰나?
* server-side 코드
* 안드로이드 Application
* Intel Multi-OS Engine[^1]을 사용하면 iOS도 개발 가능
* JavaScript도 지원 예정

## 1.2.2 Statically typed
* Java와 동일하게 statically typed(<->Dynamically typed) 언어이다.
* 하지만, 자바처럼 모든 변수에 명시적으로 타입을 지정할 필요는 없다.

~~~ kotlin
val x = 1
~~~
* 위와 같이 선언하는 경우 자동적으로 Int 타입으로 결정된다. 
* 타입 추론(type inference) : Context에서 타입을 결정하는 컴파일러의 기능
* Benefits of static typeing:
  * Performance : 런타임에 어떤 메소드를 호출해야하는지 결정할 필요가 없으므로 빠르다.
  * Reliability : 컴파일시에 에러가 검출되므로, 런타임에 에러가 발생할 기회가 더 적다.
  * Maintainability : 익숙하지 않은 코드를 개발할 때, 어떤 객체의 코드가 동작하는지 확인 할 수 있으므로 더 쉽다.
  * Tool support : 신뢰할수 있는 리펙토링, 정밀한 코드 완성도 및 다른 IDE 기능들을 사용할 수 있다.(동적 타입 언어는 못하나..?)
* 타입 추론으로 정적 타입이 가지는 장황한 대부분의 것들이 없어졌다.
* Class, Interface, Generic등 자바와 매우 유사한 컨셉들이 있다.

## 1.2.3 Functional and object-oriented
자바 개발자는 OOP에는 익숙하나, functional programing에는 익숙하지 않을 수 있다. 주요 컨셉은 아래와 같다.

* First-class function : 함수를 values로 사용. 변수에 저장하거나, 파라메터로 넘기거나, 리턴타입으로 사용가능
* Immutablility : 불변 격체를 사용.
* No side effects : 동일한 input에 대해 동일한 return을 가지며, 다른 객체를 수정하거나 외부와 상호작용하지 않는 함수를 사용.

함수형 스타일로 얻는 이점?

1. Conciseness
 * 함수를 값으로 사용하면 추상화가 더 강력해지므로 코드가 더 간결해 질 수 있다.
2. Safe Multithreading
 * immutable data와 pure function으로 적절한 동기화 없이 멀티쓰레드 환경에서 일어나는 동일 데이터에 대에 신경쓰지 않아도 된다.
3. 테스트하기 쉽다. side effects 없는 functions은 많은 셋업 코드와 테스트 환경을 위한 구성에 의존하지 않고 테스트 가능하다.

~~~ kotlin
fun findAlice() = findPerson { it.name == "Alice" }
fun findBob() = findPerson { it.name == "Bob" }
~~~

일반적으로 대부분의 프로그래밍 언어는 함수형 스타일로 사용할 수 있다.
코틀린은 아래의 함수형 스타일을 지원하기 위해 아래 기능을 지원한다.

* Function Types : 함수를 parameter/return 타입으로 사용할 수 있다.
* Lambda expression : 람다 표현식을 사용하여 코드 블럭을 넘길 수 있다.
* Data classes : immutable objects 생성을 위한 간결한 문법을 제공한다.
* 표준 라이브러리에 함수형 스타일의 객체와 컬렉션 작업을 위한 풍부한 API 셋이 있다.

함수형 프로그래밍을 강요하지는 않는다. 기존 스타일대로 코딩을 해도 된다. 알아서 잘 적절하게 사용해라.

## 1.2.4 Free and open source
컴파일러, 라이브러리, 관련된 툴 모두다 오픈소스다.(Apache 2) IntelliJ, Android Studio, Eclipse를 지원한다.

* [GitHub](http://github.com/jetbrains/kotlin)


# 1.3 Kotlin Applications
* 서버사이드, 안드로이드 개발에 사용할 수 있다.

## 1.3.1 Kotlin on the server side
* 웹 어플리케이션, 모바일 어플리케이션의 backends, Microservices
* 기존 자바 코드와 완벽히 호환된다.
* HTML generation library이나 [Exposed framework](https://github.com/jetbrains/exposed)와 같은 깨끗하고 간결한 DSL로(을) 개발할 수 있다.

## 1.3.2 Kotlin on Android
* [Anko Library](https://github.com/kotlin/anko) : 안드로이드 개발을 더 빠르고 쉽게 하기위해코틀린 팀에서 만든 라이브러리
* NPE에서 보다 더 안전하다. "Progress Has Stopped" dialog를 덜 보게 된다.
* JAVA와 완벽하게 호환된다. (사용자가 안드로이드를 업그레이드 하지 않아도 코틀린의 새로운 기능을 쓸수 있다.)
* 성능상 불리한 점도 없다.
* 컴파일 후 어플리케이션 사이즈가 크게 증가하지도 않는다.
* 람다와 코틀린의 표준 라이브러리 함수들은 인라인 처리 된다.
* 인라인 처리된 람다는 새로운 오브젝트를 생성하지 않고, GC pauses를 겪지 않아도 된다.

# 1.4 The Philosophy of Kotlin
* pragmatic, concise, safe, interopeable

## 1.4.1 Pragmatic
* 실용적이다. 암튼, 실용적이다.
* research language가 아니다. 그런건 computer science나 줘버려라.
* 특정 프로그래밍 스타일, 패러다임을 강요하지 않는다.
* IntelliJ 라는 훌륭한 툴이 있다. 이것만으로도 실용적이다.

## 1.4.2 Concise
* 새로운 코드를 작성하는 시간보다 다른 사람(또는 오래전에 내가 작성했던)의 코드를 읽는 시간이 더 많다.
* 간단하고 간결한 코드는 더 빠르게 코드를 읽을 수 있게 한다.
* 코틀린은 작성되는 모든 코드가 의미를 담도록 노력했다.
* getter, setter, constructor의 파라메터 필드 할당 등의 잡다한 코드를 제거했다.
* 람다를 쓸 수 있다.
* 그렇다고 함수 등의 문자수를 막 줄이지는 않았다. 단어가 더 읽기 쉽다.

## 1.4.3 Safe
* 안전한 언어 : 프로그램의 오류를 방지하기 위한 설계.
* 모든 에러를 방지할 수는 없다. 컴파일러에게 적절한 정보를 줘야 한다.
* Java 보다 안전한 언어를 만들기 위해 노력했다.
* JVM에는 이미 많은 안전장치들이 있다.
* 모든 타입을 명시하지 않아도, 많은 케이스에 컴파일러가 자동적으로 타입을 추론한다.
* 런타임이 아닌 컴파일타임에 더 많은 오류를 체크한다.
* NPE을 제거하기 위해 노력한다.(최소한의 비용으로..)
* nullable data를 다루기 위해 많은 편한 방법을 제공한다.
* ClassCastException도 피할 수 있다.

 ~~~ kotlin
 if (value is String)   // 여기서 타입을 체크하면
 	println(value.toUpperCase()) // 따로 캐스팅 하지 않아도 된다.
 ~~~


## 1.4.4 Interoperable
* "기존 라이브러리를 쓸 수 있나요??" "Yes, absolutely."
* 자바 메소드를 호출하고, 클래스를 상속하고, 인터페이스를 구한할 수 있다.
* 자바 annotation을 코틀린 클래스에 쓸 수 있다.
* 한 프로젝트에서 자바 코드와 코틀린 코드를 함께 쓸 수 있다.
* 코틀린은 기존 자바 라이브러리를 가능한 많이 사용하고 있다.
 * 코틀린의 컬렉션은 자바의 컬렉션 라이브러리이다.
 * 사용하기 좀 더 편하도록 함수를 추가했다.
* IDE에서도 java, kotlin 관계없이 Navigate, Debug, Refactor할 수 있다.

# 1.5 Using the kotlin tools
* 환경설정은 [여기](https://kotlinlang.org/docs/tutorials)를 참고하자.

## 1.5.1 Compiling Kotlin code
![https://lh3.googleusercontent.com/IG7wxpKzmwnsHLlaO7NdZXmV_TE4V-cqqos6g-8hP_smrY2Pcwgx0552htBpV0-92enlRPKFmmXq2N0e8FgCyYGu-fs7LvCuqDtFZdWrzvXhcbfRz-mmQh1nzGhq0VzcQItVsQ6IgFhjjn9AFKKa2_4EbKytrBaLMYqeFVU4EW_2vLJcyzvHsEMfxzeQGW4OW45a7-oOX6706wGl4r6q9IcwkoeJATKTLMH0YGU-sYpuEdKH3idM78oNMoUH7ceVCVMDybgDBIl8bJtpAzcVom7VEEBBnaBE8pILVHQaO3WKOEheEU7dI0Rk3I4QC34Y3lRFCgsiiamq4v32I_AYReGbon-cP0ZlXVBZccWQ26WciQDe4jokmn8X5HcFZr5cWQKluVB_X4Nd26gNKuPCMiUpg9kzCjHoK4omkkNdzK9HH45xNk9W0_VyVLPOG2OsnnzuWNiElhHDLLOtAnshurDFip2ZnuK6MUmFRE81j19LzOQYbui0DuPaVUHZH_dGzbXVd_AfprVETVvUiQ5ORYPR-SSWOU7BHdsv-DKw60WN-CMPnLw_NE0ezKfA19y5bkOoGxw4_jwaTjbw47fRLhzVZjWNx05f_mrE-uYXhIwgKFFZSS-wh6a-t02eYCTuthmlaKfN0fJcEG06tmEHwe16T13tBmiAnw=w1226-h470-no](https://lh3.googleusercontent.com/IG7wxpKzmwnsHLlaO7NdZXmV_TE4V-cqqos6g-8hP_smrY2Pcwgx0552htBpV0-92enlRPKFmmXq2N0e8FgCyYGu-fs7LvCuqDtFZdWrzvXhcbfRz-mmQh1nzGhq0VzcQItVsQ6IgFhjjn9AFKKa2_4EbKytrBaLMYqeFVU4EW_2vLJcyzvHsEMfxzeQGW4OW45a7-oOX6706wGl4r6q9IcwkoeJATKTLMH0YGU-sYpuEdKH3idM78oNMoUH7ceVCVMDybgDBIl8bJtpAzcVom7VEEBBnaBE8pILVHQaO3WKOEheEU7dI0Rk3I4QC34Y3lRFCgsiiamq4v32I_AYReGbon-cP0ZlXVBZccWQ26WciQDe4jokmn8X5HcFZr5cWQKluVB_X4Nd26gNKuPCMiUpg9kzCjHoK4omkkNdzK9HH45xNk9W0_VyVLPOG2OsnnzuWNiElhHDLLOtAnshurDFip2ZnuK6MUmFRE81j19LzOQYbui0DuPaVUHZH_dGzbXVd_AfprVETVvUiQ5ORYPR-SSWOU7BHdsv-DKw60WN-CMPnLw_NE0ezKfA19y5bkOoGxw4_jwaTjbw47fRLhzVZjWNx05f_mrE-uYXhIwgKFFZSS-wh6a-t02eYCTuthmlaKfN0fJcEG06tmEHwe16T13tBmiAnw=w1226-h470-no)

## 1.5.2 Plug-in for Intellij IDEA and Android Studio
* 플러그인이 있다. 
* 최신 버전에는 포함되어 있다.

## 1.5.3 Interactive shell
* REPL(Read Eval Print Loop)로 코틀린 코드를 한 줄씩 실행해 볼 수 있다.
* 인자 없이 **kotlinc** 명령어를 사용하거나, 플러그인에서 해당 메뉴를 선택해라.

## 1.5.4 Eclipse plug-in
* 이클립스 플로그인도 있다. Marketplace에서 검색해라.

## 1.5.5 Online playground
* [여기](http://try.kotl.in)에서 온라인으로 코틀린 코드를 작성하고 실행해 볼 수 있다.

## 1.5.6 Java-to-Kotlin converter
* java 코드를 쉽게 kotlin 코드로 변경할 수 있다.
* Intellij, Android Studio, Eclipse, Online 다 가능하다.

---
1장 끝.

[^1]: [https://software.intel.com/en-us/multi-os-engine](https://software.intel.com/en-us/multi-os-engine)