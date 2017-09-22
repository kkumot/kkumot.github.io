---
layout: post
title: "Kotlin 핵심파악하기 - 심재철"
description: "핵심만 골라 배우는 안드로이드 스튜디오3&프로그래밍 특별부록"
categories: [kotlin]
tags: [kotlin]
redirect_from:
  - /2017/09/14/
---

* 3주만에 다시 "kotlin in action"을 읽으려고 열었더니 어떨떨.. 이것부터 해보자.
* ***이 내용에 대한 저작권은 심재철님에게 있으며 상업적 목적이 아니리면 누가나 공유하고 배포할 수 있습니다.*** 라고 써있음!! 무료다!! ㅋㅋ 아래는 링크!
* [https://github.com/Jpub/AndroidStudio3](https://github.com/Jpub/AndroidStudio3)

# 개요와 실습환경 구축
## 개요
* 2011년부터 오픈소스로 JetBrains에서 개발 시작, 2016년 1.0 정식 버전 발표. 현재도 진화중.

* 코틀린 특징
> * JVM에서 실행되므로 자바와 완전히 호환, 크로스 플랫폼 지원
> * 정적 타입 언어. type inference(타입 추론) 기능이 있음
> * 객체지향 프로그래밍을 지원. extension 함수와 확장 속성을 사용할 수 있음.(objective-c의 그 기능..)
> * 함수형 프로그래밍 지원.
> * 문법과 코드가 간결
> * NPE(NullPointerException) 예외가 생기지 않도록 언어 자체에서 배려하고 있어 자바보다 안전
> * OSS이므로 무상 사용 가능
> * 자바 코드를 kotlin코드로 변환해주는 변환기 제공(Intellij, Android Studio, Eclipse)

* kotlin compile

~~~
kotlin hello.kt -include-runtime -d hello.jar
~~~
* -include-runtime : 코틀린 런타임 라이브러리를 애플리케이션에 포함시키라는 의미, 다른 코드에서 라이브러리로 사용하기 위해 컴파일할 때는 필요 없음.
* 코틀린 컴파일러는 소스 파일 이름의 첫 자를 대문자로 바꾸고 그 뒤에 Kt를 붙인 .class파일을 생성한다.
* 기본 함수는 **public static final**로 생성된다.
* 클래스는 **public final** 로 생성된다. 즉 상속이 안된다. 상속이 가능하게 하려면 class 키워드 앞에 **open** 키워드를 추가해야 한다.

# 구성 요소와 문법

~~~ kotlin
// 함수는 fun 으로 시작한다.
// 파라메터는 (파라메터명: 타입) 으로 쓴다
fun main(args: Array<String>) {
    // ;은 사용하지 않는다.
    // println은 System.out.println을 래핑한 함수이다.
    println(makeMessage1(1))
    println(makeMessage1(2))
    println(makeMessage2(1))
    println(makeMessage2(2))
}

// 리턴 타입은 : Type 형태로 쓴다
fun makeMessage1(msgType: Int) : String {
    return if (msgType == 1) "안녕하세요?" else "또 만났군요!"
}

// 아래와 같이 함수 선언과 동시에 반환값을 생략하고 대입문에 정의할 수 있다.
fun makeMessage2(msgType: Int) = if (msgType == 1) "날씨 좋죠?" else "참 맑군요!"
~~~

## B.3 코틀린 타입
### B.3.1 기본타입
* 기본(primitive)타입은 클래스로 정의되어 있다.
* Byte(1), Short(2), Int(4), Long(8), Float(4), Double(8), Char(2). Boolean

 ~~~ kotlin
val a = 100
// 자동으로 변환되지 않는다.
val b: Long = a.toLong()
// toByte(), toShort(), toInt(), toLong(), toFloat(), toDouble(), toChar()
~~~

### B.3.2 문자열 타입
* String 사용, 자바와 동일

### B.3.3 기본 타입의 리터럴
* Long : L
* Float : F,f
* Double : 실수는 기본적으로 double 타입으로 간주됨. 별도의 표기 없음
* 16진수 : 0x, 0X
* 문자 : '1', 'ㅁ' 홀 따옴표 사용. '\t' '\u009'

### B.3.4 문자열 리터럴
* val s = "삶이 그대를 속일지라도\n슬퍼하거나 노하지 말라\n"
* 겹따옴표 사용
* 아래와 같이 형태로 줄바꿈을 나타낼 수도 있다.

 ~~~ kotlin
val s = """
    삶이 그대를 속일지라도
    슬퍼하거나 노하지 말라
    슬픈 날엔 참고 견디라
    즐거운 날이 오고야 말리니
    """
~~~

 ~~~ kotlin
 // trimMargin() 여백 제거 문자를 전달하면 해당 문자까지 여백을 제거, default 문자는 "|"
val s = """
    >삶이 그대를 속일지라도
    >슬퍼하거나 노하지 말라
    >슬픈 날엔 참고 견디라
    >즐거운 날이 오고야 말리니
    """.trimMargin(">")
~~~

* 정수, 실수 변환
* 
 ~~~ kotlin
// 정수, 실수 타입 변환
val c = "77".toInt()
val d = "123.4".toDouble()
// 문자열 템플릿을 만들어 사용
println("$c, $d")
~~~
* 문자열 템플릿

 ~~~ kotlin
    val count = 77
    val s1 = "Count = $count"
    // 배열과 표현식의 경우는 중괄호({})를 앞뒤로 붙인다.
    val s2 = "$s1 의 길이는 ${s1.length}"
    println(s1)
    println(s2)
~~~
* 리터럴 값을 알아보기 쉽게 값의 중간에 _를 사용할 수 있다.

 ~~~ kotlin
    val oneMillion = 1_000_000
    val creditCardNumber = 1234_5678_9012_3456
    val socialSecurityNumber = 999_99_9999
    val hexBytes = 0xff_fe_ad_de
    val bytes = 0b00000000_11111111_10101010_01011100
    println("$oneMillion, $creditCardNumber, $socialSecurityNumber, $hexBytes, $bytes")
~~~

### B.3.5 배열
* 배열은 Array로 클래스로 정의되어 있음
* Array<String> 과 같이 배열에 저장되는 요소의 타입을 제네릭 타입으로 나타냄
* [] 연산자는 배열을 선언할때는 사용하지 않음. 읽거나 쓸 때만 사용

 ~~~ kotlin
    // arrayOf()를 사용하여 배열 생성
    val item = arrayOf("사과", "바나나", "키위")
    for (fruit in item) {
        println(fruit)
    }

    // Array클래스 생성자를 사용
    val num = Array<String>(5, { i -> (i * i).toString() })
    // 위와 동일한 코드
    val num2 = Array<String>(5) { i -> (i * i).toString() }
    // <String>은 생략 가능. 컴파일러가 추론할 수 있음
    for (item in num) {
        println(item)
    }

    // 기본 타입의 요소를 저장하는 별도의 클래스
    // IntArray, ByteArray, ShortArray, LongArray, FloatArray, DoubleArray, CharArray, BooleanArray
    // 위 클래스들은 코틀린 컴파일러가 컴파일할 때 자바의 기본 타입 배열로 변환한다.
    val intItem: IntArray = intArrayOf(1, 2, 3, 4, 5)
    val intItem2 = IntArray(5, { i -> (i * i) })
    val intItem3 = IntArray(5) { i -> (i * i) }
    
    // index를 사용하여 읽거나 쓸수 있다
    intItem[0] = intItem[1] + intItem[2]
~~~


## B.4 연산자와 연산자 오버로딩
### B.4.1 산술 연산자
* +, -, *, /, % 연산자는 알고 있는 그것. 우선순위도 알고 있는 대로.(\* / % >>> + -)
* 단, 코틀린에서는 해당 연산자를 오버로딩한 함수를 사용해서 연산을 처리함
* plus, minus, times, div, rem/mod
* 오버로딩한 함수를 사용하므로 객체간 연산도 가능함

 ~~~ kotlin
	fun main(args: Array<String>) {
	    val m1 = Score(100, 200)
	    val m2 = Score(300, 400)
	    // 객체간 + 연산을 사용하면 plus 함수를 사용한다. plus함수를 오버로딩 하지 않으면 컴파일에러가 발생.
	    println(m1 + m2)
	    println(m1 * m2)
	}
	
	// data 키워드는 나중에 자세히 알아보자
	data class Score(val a: Int, val b: Int) {
	    // 연산자를 오버로딩 하는 함수는 앞에 operator 키워드를 붙여준다.
	    operator fun plus(other: Score): Score {
	        return Score(a + other.a, b + other.b)
	    }
	}
	
	// 확장 함수를 사용하여 아래와 같이 정의할 수도 있다.
	operator fun Score.times(other: Score): Score {
	    return Score(a * other.a, b * other.b)
	}
 ~~~
 
### B.4.2 단항연산자
* +a, -a, !a, ++a, a++, --a, a--
* unaryPlus, unaryMinus, not, inc, dec

 ~~~ kotlin
	operator fun Score.unaryMinus(): Score {
	    return Score(-a, -b)
	}
	
	operator fun Score.inc(): Score {
	    return Score(a + 1, b + 1)
	}
 ~~~

### B.4.3 복합 대입 연산자
* +=, -=, *=, /=, %=
* plusAssign, minusAssign, timesAssign, divAssign, modAssign

### B.4.4 비트 연산자
* \<\<, \>\>, \>\>\>, &, \|, ^, ~ 가 없다. -_-;
* shl, shr, ushr, and, or, xor, inv

### B.4.5 논리 연산자
* &&, \|\|, ! 아직은 사용가능하다.
* and, or, not

### B.4.6 동등 비교 연산자
* a == b >>>> a?.equals(b) ?: (b==null)
 * null 체크 안해도 된다. 야호!
* a === b 변수의 참조 값(같은 객체를 참조하는지)을 비교. 오버로딩 안된다.
* a !=b >>>> !(a?.equals(b) ?: (b == null))
* a !== b 오버로딩 안됨.

### B.4.7 그 밖의 비교 연산자
* \>, <, >=, <=
* a.compareTo(b) > 0, a.compareTo(b) < 0, a.compareTo(b) >=0, a.compareTo(b) <= 0
 * compareTo함수는 a가 b보다 크면 양수, 작으면 음수값이 반환됨.

 ~~~ kotlin
	fun main(args: Array<String>) {
	    val p1 = Customer("홍길동", "010-1111-2222")
	    val p2 = Customer("김선달", "010-1111-3333")
	    println(p1 < p2)
	    println(p1 > p2)
	
	}
	
	class Customer(val name: String, val phone: String) : Comparable<Customer> {
	    override fun compareTo(other: Customer): Int {
	        return compareValuesBy(this, other, Customer::name, Customer::phone)
	    }
	}
~~~

### B.4.8 In 연산자
* 컬렉션 객체에 사용
* a in b >>>> b.contains(a)
* a !in b >>>> !b.contains(a)


### B.4.9 범위(..) 연산자
* 범위 값을 가지며 a..b로 표기하면 컴파일러가 a.rangeTo(b)코드로 생성

 ~~~ kotlin
    val start = LocalDate.now()
    val end = start..start.plusDays(15)

    println(start.plusWeeks(1) in end)
    println(end)
    
    // 결과
    // true
    // 2017-09-14..2017-09-29 
~~~


### B.4.10 인덱스 연산자
* 배열과 컬렉션 클래스에서 사용
* a[i] => a.get(i)
* a[i, j] => a.get(i, j) : 왼쪽과 같이 쓰고 싶으면 오른쪽과 같은 operator 함수를 만들자.
* a[i_1, ..., i_n] => a.get(i_1, ..., i_n) 
* a[i] = b => a.set(i, b)
* a[i, j] = b => a.set(i, j, b) 
* a[i_1, ..., i_n] = b => a.set(i_1, ..., i_n, b) 

### B.4.11 Invoke 연산자
* 코틀린에서는 클래스 인스턴스에 괄호()를 붙여서 호출할 수 있다.
* operator 키워드를 지정하여 연산자를 오버로딩한 invoke() 함수를 정의하면 된다.
* a() ==> a.invoke()
* a(i) ==> a.invoke(i)
* a(i, j) ==> a.invoke(i, j)
* a(i_1, ..., i_n) ==> a.invoke(i_1, ..., i_n)

 ~~~ kotlin
	fun main(args: Array<String>) {
    	val instance = InvokeOperator("코틀린을")
    	instance("배우자")
    }
    
	class InvokeOperator(val message1: String) {
	    operator fun invoke(message2: String) {
	        println("$message1 $message2")
	    }
	}
~~~

### B.4.12 타입 확인 연산자: is, !is

~~~ kotlin

    val b: String = "코틀린을 배우자"
    if(b is String) {
        println("String 타입")
    } else {
        println("String 타입아님")
    }

	open class A{}
	class B : A() {}

    val x = B()
    if(x is A) println("A type") else println("not A type")
    if(x is B) println("B type") else println("not B type")
    
    // 결과
    // String 타입
	 // A type
	 // B type
~~~

## B.5 Null 타입 처리 메커니즘
* java에서 런타임에 흔히 발생하는 NullPointerException을 컴파일 시점에 미리 방지할 수 있게함

### B.5.1 Null 가능 타입
* 코틀린에서는 기본적으로 모든 타입에 null 값을 허용하지 않는다.
* null을 사용하려면 타입 이름 끝에 물음표(?)를 붙여야 한다.

~~~ kotlin
val s1: String? = null
val s2: String = null // Error: Kotlin: Null can not be a value of a non-null type String
val s3: String = s1 // Error: Kotlin: Type mismatch: inferred type is String? but String was expected
~~~

### B.5.2 Null 처리 연산자 #1: "?."
* **?.** 연산자는 객체의 속성이나 함수를 안전하게 참조/호출할 수 있게 해준다.

 ~~~ kotlin
 var a: String? = "코틀린을 배우자"
 // a가 널이 아닌 경우 substring을 호출하고 호출한 결과가 널이 아닌 경우 length를 호출
 println(a?.substring(5)?.length)
 ~~~
 
### B.5.3 Null 처리 연산자 #2 "?:"
* 엘비스 프레슬리 이모티콘과 유사하다고 하여 **엘비스 연산자** 라고함.
* 왼쪽 피연산자 값이 null이 아니면 그 연산자의 결과를 반환하고 null이면 오른ㅉ/ㅗㄱ 피연산자의 결과 값을 반환

 ~~~ kotlin
 var a: String? = "코틀린을 배우자"
 val b = a?.length ?: 0 // val b: Int = if (a != null) a.length else 0
 println(b)
 ~~~
 
### B.5.4 Null 처리 연산자 #3: "!!"
* **!!** 연산자는 참조 변수의 값이 null이 아니면 정상적으로 코드를 수행하고, null인 경우 런타임에 NPE를 발생 시킨다.
* NPE를 컴파일 시점에 방지하고자 하는 코틀린에서 이녀석은 왜 쓰는가?
 * **우리가 작성하는 클래스와 함수에서는 왠만하면 쓰지 말자**
 * 다른 라이브러리의 변수나 함수를 참조하거나 호출할 때 명시적으로 NPE를 파악하고자 하는 경우 사용하자.

### B.5.5 Null 처리 연산자 #4: as, as?
* 객체의 타입을 캐스팅할 때는 as 연산자를 사용한다. (premitive 타입은 변환 함수를 써야한다. toLong(), toDouble()...)

 ~~~ kotlin
 // s2를 nullable string으로 캐스팅. s2가 String타입에 부적합한경우 ClassCastException이 발생
 val s1: String? = s2 as String?
 
 // s2가 String타입에 부적합하더라도 ClassCastException이 발생하지 않고 null 값이 반환
 val s1: String? = s2 as? String?
 ~~~
 
 
## B.6 코드 실행 제어
* if, when, for, while, do


### B.6.1 if
* 자바의 if와 동일하게 사용가능하다.
* 코틀린에서는 if문을 하나의 표현식으로 간주하므로, 변수의 대입문에 if를 사용할 수 있다.

 ~~~ kotlin
	 val result = if (param == 1) {
	 	"one"
	 } else if (param == 2) {
	 	"two"
	 } else {
	 	"three"
	 }
 ~~~

### B.6.2 when
* 자바의 switch~case와 유사하지만, 더 간결하고 기능도 뛰어나다.

~~~ kotlin
fun main(args: Array<String>) {
    whenUsage(2, 50, "서울시")
}

fun whenUsage(inputType: Int, score: Int, city: String) {
    // -------------- #1
    when (inputType) {
        // 값 -> 실행 코드 블록 형태로 작성
        1 -> println("type-1")
        // , 를 사용하여 여러 조건을 검사할 수 있다
        2, 3 -> println("type-2,3")
        // 모두 일치하지 않는 경우 else의 -> 코드를 실행한다.
        // 자바의 break는 필요 없다
        else -> {
            println("미확인")
        }
    }

    // --------------- #2
    when (inputType) {
        // 함수를 실행하여 그 결과를 검사할 수도 있다
        checkInputType(inputType) -> {
            println("타입정상")
        }
        else -> println("타입 비정상")
    }

    // --------------- #3
    val start = 0
    val end = 100
    when (score) {
        // in연산자와 범위 연산자 ..을 사용하여 검사할 수 있다.
        in start..(end / 4) -> println("우수함")
        50 -> println("평균임")
        in start..end -> println("범위에 있음")
        else -> println("범위를 벗어남")
    }

    // ---------------- #4
    // 결과를 변수에 대입할 수 있다.
    val isSeoul = when (city) {
        // is 연산자를 사용하여 타입을 확인할 수 있다.
        is String -> city.startsWith("서울")
        // 값을 대입하는 경우 else가 없으면 컴파일 에러가 발생한다
        else -> false
    }
    println(isSeoul)

    // ----------------- #5
    // if-else 문처럼도 사용가능하다.
    when {
        city.length == 0 -> println("도시명을 입력하세요")
        city.substring(0, 2).equals("서울") -> println("서울이군요")
        else -> println(city)
    }
}

fun checkInputType(inputType: Int): Int {
    // 아래 식은 inputType in 1..2 로 변경할 수 있다. 
    if (inputType >= 1 && inputType < 3) {
        return inputType
    }
    return -1
}
~~~


### B.6.3 for 루프

~~~ kotlin
// in 연산자를 사용하여 반복을 처리
val item1 = listOf("사과", "바나나", "키위")
for (item in item1) {
    println(item)
}

// index를 사용하여 반복
val item2 = listOf("사과", "바나나", "키위")
for (index in item2.indices) {
    println("item at $index is ${item2[index]}")
}

// array도 동일하게 사용 가능
val item3 = arrayOf("사과", "바나나", "키위")
for (index in item3.indices) {
    println("item at $index is ${item3[index]}")
}

// 1부터 100까지 반복
for (i in 1..100) {
    print(i)
}

// 1부터 100까지 반복. 100은 제외
for (i in 1 until 100) {
    print(i)
}

// 2부터 10까지 반복. i는 2씩 증가
for (i in 2..10 step 2) {
    print(i)
}

// 10부터 1까지 1씩 감소
for (i in 10 downTo 1) {
    print(i)
}
~~~


### B.6.4 while과 do-while 루프
* while과 do-while 루프는 다른 언어와 동일

~~~ kotlin
val items = listOf("사과", "바나나", "키위")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}
    
index = 0
do {
    println("item at $index is ${items[index]}")
    index++
} while (index < items.size)
~~~


## B.7 함수
### B.7.1 함수 선언과 호출
* 함수를 선언할 때 **fun** 키워드를 함수 이름 앞에 지정
* 함수는 소스 코드 파일의 어디든 바로 정의 할 수 있음
* 함수의 매개변수는 **변수이름:타입**의 형태로 지정
* 리턴 값의 타입은 함수 정의 문장 끝에 콜론을 추가한 후 지정

~~~kotlin
fun min(valueLeft: Int, valueRight: Int): Int {
	return if(valueLeft < valueRight) valueLeft else valueRight
}

// 코틀린에서 if는 표현식으로 간주하므로 대입문에 바로 추가 가능
// min 함수의 리턴 타입이 생략되어 있지만 컴파일러가 추론하므로, 에러가 생기지 않는다.
fun min(valueLeft: Int, valueRight: Int) = if(valueLeft < valueRight) valueLeft else valueRight


// 매개변수의 기본값을 정의할 수도 있다.
fun min(valueLeft: Int = 0, valueRight: Int = 0) = if(valueLeft < valueRight) valueLeft else valueRight

// 사용 예시
min(100,50)
min(100) // valueRight는 기본값인 0이됨
min() // valueLeft, valueRight 모두 0
min(valueLeft=50, valueRight=100) // 지명인자(named argument)를 사용하여 값을 전달
min(valueRight=100, valueLeft=50) // 지명인자를 사용하야 정의된 순서와 무관하게 전달
min(valueLeft=50) // 지명인자와 기본값을 사용하여 호출
~~~

### B.7.2 가변 인자
* 함수를 호출할 때 인자의 개수를 가변적으로 전달할 수 있다.
* **vararg** 키워드를 사용한다.
* 가변 인자들은 배열로 전달 된다.

~~~ kotlin
// 가변인자를 배열로 받아 arrayList를 생성하여 저장한 후 반환
fun <T> newList(vararg ts: T): List<T> {
    val result = ArrayList<T>()
    // 아래 for문은 아래 연산자 오버로딩 함수로 변경 가능하다
    // result += ts
    for (t in ts) {
        result.add(t)
    }
    return result
}


val list = newList(1, 2, 3)

val e = arrayOf(7, 8, 9) // Int 타입의 요소 3개를 저장한 배열 생성
val list2 = newList(1, 2, 3, *e, 5) // 1,2,3,7,8,9,5 를 가진 ArrayList 생성
// *e는 배열의 요소를 하나씩 가져와서 인자로 전달하라는 의미. *를 확산(spread)연산자라고함
~~~


### B.7.3 함수의 종류
* 최상위 함수
 * 함수를 클래스 외부에 정의. 소스 코드 파일(.kt)에 바로 정의 하여 사용
 * 애플리케이션에서 공통으로 사용하는 기능을 처리. (자바의 경우 모든 것이 클래스에 정의되어야 하므로, 특정 클래스에 static 메서드로 모아두고 사용)
 * 위 함수는 public final static으로 컴파일됨.
* 최상위 수준의 속성(property)
 * (.kt)파일에 선언
 * private final static으로 컴파일됨. getter는 public으로 생성됨
 * **const** 키워드를 사용하는 경우 public final static으로 컴파일됨 
 * ex) const val ERROR_CODE = 1 ==> public final static Int ERROR_CODE = 1;

* 클래스 내부에 정의하여 사용
 * 멤버 함수(자바의 인스턴스 메서드)

  ~~~ kotlin
  class Customer() {
      fun checkId() {..}
  }
  // 인스턴스를 생성(Customer())하여 호출
  Customer().checkId()
  ~~~ 
  
* 지역(local) 함수를 선언하여 사용
 * 다른 함수 내부에 포함된 함수

 ~~~ kotlin
	fun main(args: Array<String>) {
	    println(calcCombination(45, 6)) // 로또 복권이 모든 조합 가능한 번호 개수 출력
	}
	
	fun calcCombination(whole:Int, selected: Int): Double {
	    if (selected > whole || selected <= 0 || whole <= 0) {
	        return -1.0
	    } else if (selected == whole) {
	        return 1.0
	    }
	    
	    // 함수 내부에 factorial을 계산하는 지역함수를 정의하여 사용
	    // 지역 함수에서는 자신을 포함하는 외부 함수의 인자나 변수를 그냥 사용할 수 있다.(여기서는 안썻지만...)
	    fun calcFactorial(num: Int): Double {
	        var total: Double = 1.0
	        for (i in num downTo 1) {
	            total *= i
	        }
	        return total
	    }
	
	    return calcFactorial(whole) / (calcFactorial(whole - selected) * calcFactorial(selected))
	}
 ~~~ 
 
 
* 제네릭(generic) 함수
 * fun <T> newList(vararg ts: T): List T
 * 위 예에서 <T>가 generic 타입이다. 즉, list에 저장할 요소의 타입을 미리 정하지 않고 함수 호출 코드에서 타입을 지정할 수 있다는 의미이다.

* 확장(extension) 함수
 * 특정 클래스로부터 상속받지 않고 해당 클래스의 기능을 확장할 때 사용

 ~~~ kotlin
    // 코틀린 MutableList 클래스를 상속받지 않고 추가로 swap 기능을 하는 함수를 확장
    fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
        val tmp = this[index1]   // this는 MutableList의 객체를 나타냄
        this[index1] = this[index2]
        this[index2] = tmp
    }


    // 사용예시
    val l = mutableListOf(1, 2, 3)
    l.swap(0, 2)
 ~~~ 
 
* 중위(infix) 함수
 * 두 개의 피연산자 사이에 함수 호출을 넣어서 사용
 * 새로운 함수라기 보다는 사용을 편리하게 하기 위해 코틀린에 추가된 기능
 * 대표적인 예시로는 비트 연산자가 있다. ==> 8 shr 2 

 ~~~ kotlin
 // Int 클래스에 정의되어 있는 shl 함수 예시
 infix fun shl(bitCount: Int): Int {
     ...
 }
 val a = 8
 // a+8은 (a+8)로 괄호를 씌우지 않아도 됨. shr()함수 호출보다 +연산자의 우선순위가 더 높음.
 println(a+8 shr 2)
 ~~~
 
 * 8 shl 2, 8.shl(2) 두 가지 방법으로 호출하여 사용 가능
 * 중위 함수 정의 3가지 조건
 
  > 1. 클래스의 멤버 함수이거나 확장 함수이어야함
  > 2. 매개변수가 한 개여야 함
  > 3. infix 키워드로 함수가 정의되어야함 
  
* 재귀(tail recursive)함수
 * 일련의 코드를 반복해서 실행해야 하는 알고리즘의 경우 루프나, 재귀함수를 사용할 수 있다.
 * 재귀 함수를 사용하는 경우 스택 오버플로의 위험이 따르고, 시스템의 부담도 커진다.
 * 코틀린에서는 이런 단점을 줄이고 실행 속도도 빠르며, 효율적인 루프를 사용하는 재귀 함수를 제공한다.
 * **fun** 키워드 앞에 **tailrec** 키워드를 붙인다.

 ~~~ kotlin
	fun main(args: Array<String>) {
	    println(calcFactorial(10000))
	
	    println(getFibonacciNumber(10000, BigInteger("1"), BigInteger("0")))
	}
	
	// 아래 함수는 tailrec와 무관하게 컴파일 후 재귀함수 형태로 보여진다.
	tailrec fun calcFactorial(num: Int): Double {
	    if (num == 1) {
	        return 1.0
	    }
	    else {
	        return (num * calcFactorial(num -1)).toDouble()
	    }
	}
	
	// 아래 함수는 tailrec 키워드 여부에 따라 컴파일 후 재귀함수형태 or while루프(tailrec키워드가 있는 경우) 형태로 변경된다.
	tailrec fun getFibonacciNumber(n: Int, a: BigInteger, b:BigInteger): BigInteger {
	    if(n==0) {
	        return b
	    } else {
	        return getFibonacciNumber(n - 1, a + b, a)
	    }
	}
 ~~~ 
 
 
## B.8 클래스와 인터페이스
### B.8.1 클래스 선언과 생성자
~~~ kotlin
// 클래스 이름의 첫 자는 대문자
class Friend {}

// 본체가 없는 경우 {}도 생략이 가능하나.. 이렇게 쓸일이 없을듯.
class Friend
~~~

~~~ kotlin
fun main(args: Array<String>) {

    val f1 = Friend1()
    f1.name = "박문수"
    f1.tel = "010-123-4567"
    f1.type = 5
    println(f1.name + "," + f1.tel + "," + f1.type)

    val f2 = Friend2()
    f2.name = "홍길동"
    f2.tel = "010-456-4567"
    f2.type = 5 // set() 함수가 실행되므로 4값이 들어간다.
    println(f2.name + "," + f2.tel + "," + f2.type)


    // named argument를 사용하여 생성자의 인자를 전달
    val f3 = Friend3(tel = "010-678-1234", name = "김선달", type = 5)
    println(f3.name + "," + f3.tel + "," + f3.type)

    val f4 = Friend4("전신주", "010-1111-1234", 3)
    println(f4.name + "," + f4.tel + "," + f4.type)
    
    
    // 실행 결과
    박문수,010-123-4567,5
    홍길동,010-456-4567,4
    김선달,010-678-1234,4
    전신주,010-1111-1234,3
}

class Friend1 {
    // 생성자 정의가 없어 기본 생성자가 자동으로 생성된다

    // 아래의 속성들은 자동으로 getter/setter가 생성된다.
    var name: String = ""
    var tel: String = ""
    var type: Int = 4
}

class Friend2 {
    var name: String = ""
    var tel: String = ""

    // 아래와 같이 커스텀 세터를 정의할 수 있다
    var type: Int = 4
        set(value: Int) {
            // set 함수에서 해당 속성을 액세스 하려면 field 키워드를 사용한다
            if(value < 4) field = value else field = 4
        }
}

// 생성자를 아래와 같의 정의 할 수 있다. 아래 코드와 동일하다
// class Friend3 constructor(var name: String, var tel: String, var type: Int)
// 기본 생성자에서는 constructor 키워드를 생략할 수 있다
// private 와 같은 접근 제한자를 사용하는 경우 constructor 키워드를 생략할 수 없다.
class Friend3 (var name: String, var tel: String, var type: Int)
{
    // 초기화 블록을 사용하여 속성의 값을 초기화 할 수 있다.
    init {
        type = if(type < 4) type else 4
    }
}

class Friend4 {
    var name: String
    var tel: String
    var type: Int

    // java 처럼 보조 생성자를 정의할 수 있다.
    constructor(name: String, tel: String, type: Int) {
        this.name = name
        this.tel = tel
        this.type = if(type < 4) type else 4
    }
}
~~~


### B.8.2 멤버 함수
* 클래스 내부의 멤버로 가질 수 있는 속성, 생성자, 초기화 블록, 멤버 함수, 중첩/내부 클래스 다른 객체 중에서 멤버함수에 대해 알아본다.
* 클래스 내부에 정의된 함수는 멤버 함수로 간주된다.
* 멤버 함수는 해당 클래스의 인스턴스를 생성하여 호출할 수 있다.
* 접근 제한자를 추가로 지정할 수 있다.
* 기본 접근 제한자는 public 이다.

~~~ kotlin
class Friend2 {
...
	// 멤버 함수 정의
	fun printName(): Unit = println(this.name)
}

fun main(args: Array<String>) {
	val f2 = Friend2()
	// 인스턴스.함수명() 으로 호출
	f2.printName()
}
~~~

### B.8.3 접근 제한자
* 코틀린에서틑 클래스, 클래스 멤버, 최상위 수준 변수/함수에 다음의 접근 제한자를 사용할 수 있다.

 |접근 제한자|클래스 멤버일 때|최상위 수준으로 선언되었을 때|
|---|---|---|
|public(기본값)|어디서든 사용 가능|어디서든 사용 가능|
|internal|같은 모듈에서만 사용 가능|같은 모듈에서만 사용 가능|
|protected|서브 클래스에서만 사용 가능|해당 없음|
|private|클래스 내무에서만 사용 가능|코틀린 파일 내부에서만 사용 가능|

 * internal의 모듈 : Intellij의 모듈, Gradle의 Project

* 클래스와 클래스 멤버에만 사용 가능한 접근 제한자 

 |접근 제한자|클래스|클래스 멤버|
|---|---|---|
|final(기본값)|서브 클래스를 만들 수 없음|슈퍼 클래스의 멤버를 오버라이딩 할 수 없음|
|open|서브 클래스를 만들 수 있음|슈퍼 클래스의 멤버를 오버라이딩할 수 있음|
|abstract|추상 클래스를 의미|함수에만 해당되며 몸체(실행코드)가 없음|
|override|해당 없음|슈퍼 클래스의 멤버를 오버라이딩함|

 * 코틀린에서는 멤버 함수는 물론 속성도 오버라이딩할 수 있다.
 * 속성을 오버라이딩 하는 경우 슈퍼 클래스의 속성과 타입이 같아야 한다.

### B.8.4 클래스 상속과 멤버 오버라이딩
* 코틀린은 다중상속을 지원하지 않는다.
* 코틀린의 모든 클래스는 기본적으로 **Any**라는 이름의 슈퍼 클래스로부터 상속 받게 되어 있다.

 ~~~ kotlin
 open class Father(nameF: String, ageF: Int)
 // 상속관계는 콜론(:)을 사용한다
 // 서브 클래스에서는 슈퍼클래스의 속성도 초기화해주어야 한다.
 class Child(nameC: String, ageC: Int) : Father(nameC, ageC)
 
 // 서브 클래스에 기본 생성자가 정의되지 않고 보조 생성자가 정의된 경우에는 : 과 super키워드를 
 // 사용하여 슈퍼 클래스의 속성을 초기화 한다.
 class Child : Father {
     constructor(nameC: String, ageC: Int) : super(nameC, ageC)
 }
 ~~~
 
 ~~~ kotlin
	 open class Father(nameF: String, ageF: Int) {
	    
	    // 속성의 경우도 상속이 가능하다. open 키워드를 사용해야 한다.
	    open val height: Int = 170
	
	    // 속성의 경우도 상속이 가능하다. open 키워드를 사용해야 한다.
	    open var weight: Int = 70
	    
	    // 서브 클래스에서 오버라이딩을 하게 하려면 open 키워드를 사용해야 한다
	    open fun sleep() {}
	    fun looseWeight() {}
	}
	
	class Child : Father {
	    constructor(nameC: String, ageC: Int) : super(nameC, ageC)
	    
	    // 서브 클래스에서는 override 키워드를 사용해야 한다.
	    // val로 정의된 속성은 var로 override 가능하다.
	    override var height: Int = 180
	    
	    // var로 정의된 속성은 val로 override 할 수 없다.
	    // 슈퍼 클래스에 var 속성으로 정의되면 setter가 추가로 생성되어 있게된다. 서브 클래스에서 val로 override하게 되면 슈퍼클래스의
	    // setter가 무의미해진다.
	 // override val weight: Int = 65
	    
	    // 서브 클래스에서는 override 키워드를 사용해야한다
	    override fun sleep() {}
	}
 ~~~
 
 
### B.8.5 인터페이스 구현과 어버라이딩
* 자바처럼 **interface** 키워드로 인터페이스를 정의한다.
* 인터페이스는 주로 추상 함수를 갖지만 구현 코드가 있는 함수도 가질 수 있다.

 ~~~ kotlin
	interface PlayMusic {
	    var musicalInstrument: String
	    fun play()
	    fun stop() {
	        println("stop")
	    }
	}
	
	// 클래스를 상속할때처럼 : 을 사용한다. 클래스 상속처럼 ()를 붙이지는 않는다.
	class Professional(override var musicalInstrument: String) : PlayMusic {
	    override fun play() {
	
	    }
	}
 ~~~
 
 > 자바 코드로 변환해보면, interface의 속성은 getter, setter(var)로 표현됨.
 >
 > java8의 default method 와 비슷한 구현이 있는 메서드는 DefaultImpl static class가 생성되어 표현된다.
 
 ~~~ java
 
   // 변환된 java 코드.
	public interface PlayMusic {
	   @NotNull
	   String getMusicalInstrument();
	
	   void setMusicalInstrument(@NotNull String var1);
	
	   void play();
	
	   void stop();
	
	   public static final class DefaultImpls {
	      public static void stop(PlayMusic $this) {
	         String var1 = "stop";
	         System.out.println(var1);
	      }
	   }
	}
	
	public final class Professional implements PlayMusic {
	   @NotNull
	   private String musicalInstrument;
	
	   public void play() {
	   }
	
	   @NotNull
	   public String getMusicalInstrument() {
	      return this.musicalInstrument;
	   }
	
	   public void setMusicalInstrument(@NotNull String var1) {
	      Intrinsics.checkParameterIsNotNull(var1, "<set-?>");
	      this.musicalInstrument = var1;
	   }
	
	   public Professional(@NotNull String musicalInstrument) {
	      Intrinsics.checkParameterIsNotNull(musicalInstrument, "musicalInstrument");
	      super();
	      this.musicalInstrument = musicalInstrument;
	   }
	
	   public void stop() {
	      PlayMusic.DefaultImpls.stop(this);
	   }
	}
 ~~~
 
* 상속과 인터페이스 구현을 함께 하는 경우

 ~~~ kotlin
 open class MusicType {
	    open fun sing() {}
	    open fun play() {
	        println("play MusicType")
	    }
	}
	
	interface PlayMusic2 {
	    var musicalInstrument: String
	    fun play() {
	        println("play PlayMusic2")
	    }
	}
	
	// ,을 사용하여 상속과 구현을 나열하면 된다.
	class Professional2(override var musicalInstrument: String) : MusicType(), PlayMusic2 {
	    override fun sing() {}
	    override fun play() {
	    	 // 인터페이스와 슈퍼클래스에 동일 이름의 메서드가 정의되어 있는 경우 super<타입> 키워드를 사용해서 참조하면 된다.
	        super<MusicType>.play()
	    }
	}
 ~~~
 
 
### B.8.6 추상 클래스와 오버라이딩
* 추상 클래스는 인터페이스처럼 추상 함수를 가진다.
* 서브 클래스에서는 추상 함수를 반드시 오버라이딩해야 한다
* 일반 함수와 속성도 가질 수 있다

 > 이쯤되면 속성과 함수 구현을 가질 수 있는 interface와 뭐가 다른지 생각하게 된다.
 
* 추상 클래스는 **abstract** 키워드로 지정한다.
* 자바처럼 인스턴스를 생성할 수 없다.

 ~~~ kotlin
	abstract class PlayMusic3 {
	    val musicalInstrument: String = "피아노"
	    abstract fun play()
	    fun sing() {}
	}
	
	class Professional3() : PlayMusic3() {
	    // abstract 멤버 함수를 반드시 오버라이딩 해야 한다.
	    override fun play(){}
	}
 ~~~
 
### B.8.7 "object" 키워드
> 객체 선언(object declaration), 동반 객체(companion object), 객체 표현식(object expression)

* 객체 선언(object declaration) : singleton을 구현할 때 좋은 방법

 ~~~ kotlin
	object StateManager {
	    var msgNumber: Int = 0
	    var msgContent: String = ""
	
	    fun storeMessage() {
	        // 데이터를 저장하는 코드
	        println("메시지 번호 = $msgNumber, 내용 = $msgContent")
	    }
	}
	
	fun main(args: Array<String>) {
	    for (i in 1..10){
	        StateManager.msgNumber += 1
	        StateManager.msgContent = StateManager.msgNumber.toString() + "번째 메시지"
	        StateManager.storeMessage()
	    }
	}
 ~~~
 
 ~~~ java
   // java code
	public final class StateManager {
	   private static int msgNumber;
	   @NotNull
	   private static String msgContent;
	   public static final StateManager INSTANCE;
	
	   public final int getMsgNumber() {
	      return msgNumber;
	   }
	
	   public final void setMsgNumber(int var1) {
	      msgNumber = var1;
	   }
	
	   @NotNull
	   public final String getMsgContent() {
	      return msgContent;
	   }
	
	   public final void setMsgContent(@NotNull String var1) {
	      Intrinsics.checkParameterIsNotNull(var1, "<set-?>");
	      msgContent = var1;
	   }
	
	   public final void storeMessage() {
	      String var1 = "메시지 번호 = " + msgNumber + ", 내용 = " + msgContent;
	      System.out.println(var1);
	   }
	
	   private StateManager() {
	      INSTANCE = (StateManager)this;
	      msgContent = "";
	   }
	
	   static {
	      new StateManager();
	   }
	}
 ~~~
 
 * 인터페이스를 구현하거나 다른 클래스로부터 상속 받을 수 있다.
 * 클래스 내부에서도 선언 할 수 있다.
 
 ~~~ kotlin
 	class Outer() {
 		object InnerObject {
 			var count: Int = 0
 			fun printCount() {
 				println(count)
 			}
 		}
 	}
 	
 	// 아래와 같이 사용 가능하다.
 	Outer.InnerObject.printCount()
 ~~~
 
* 동반 객체(companion object)
 * 코틀린에는 자바의 static과 같은 키워드가 없다.
 * companion 키워드를 추가하여 object 객체 선언과 같이 사용할 수 있다.
 * 이러한 동반 객체는 이 객체를 포함하는 클래스에서는 자신의 멤버인 것처럼 인식한다.

 ~~~ kotlin
 	fun main(args: Array<String>) {
 		OuterClass.printMsg()
 		val m = OuterClass.create()
 		m.printMyClass()
 	}
 	
 	class OuterClass {
 	   private constructor()
 		companion object ComObj { // ComObj는 companion object의 이름이다. 생략할 수 있다.
 			fun printMsg() {
 				println("동반 객체의 함수가 호출됨")
 			}
 			// 동반 객체에서는 private 멤버도 액세스 할 수 있다.
 			fun create(): OuterClass = OuterClass()
 		}
 		
 		fun printMyClass() {
 			println("팩토리 객체의 함수가 호출됨")
 		}
 	}
 ~~~ 
 
 ~~~ java
    // java code..
	 public final class OuterClass {
	   public static final OuterClass.ComObj ComObj = new OuterClass.ComObj((DefaultConstructorMarker)null);
	
	   public final void printMyClass() {
	      String var1 = "팩토리 객체의 함수가 호출됨";
	      System.out.println(var1);
	   }
	
	   private OuterClass() {
	   }
	
	   // $FF: synthetic method
	   public OuterClass(DefaultConstructorMarker $constructor_marker) {
	      this();
	   }
	
	   @Metadata(
	      mv = {1, 1, 7},
	      bv = {1, 0, 2},
	      k = 1,
	      d1 = {"\u0000\u0018\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0018\u0002\n\u0000\n\u0002\u0010\u0002\n\u0000\b\u0086\u0003\u0018\u00002\u00020\u0001B\u0007\b\u0002¢\u0006\u0002\u0010\u0002J\u0006\u0010\u0003\u001a\u00020\u0004J\u0006\u0010\u0005\u001a\u00020\u0006¨\u0006\u0007"},
	      d2 = {"LOuterClass$ComObj;", "", "()V", "create", "LOuterClass;", "printMsg", "", "production sources for module Kotlin01"}
	   )
	   public static final class ComObj {
	      public final void printMsg() {
	         String var1 = "동반 객체의 함수가 호출됨";
	         System.out.println(var1);
	      }
	
	      @NotNull
	      public final OuterClass create() {
	         return new OuterClass((DefaultConstructorMarker)null);
	      }
	
	      private ComObj() {
	      }
	
	      // $FF: synthetic method
	      public ComObj(DefaultConstructorMarker $constructor_marker) {
	         this();
	      }
	   }
	}
 ~~~
 
 
* 객체 표현식 : 익명 객체를 생성하는 방법

	> 코틀린에서 좀 아쉬운 부분이다. 더 간결하게 할 수 있지 않았을까..?

 ~~~ kotlin
 	fun countClicks(window: Window) {
 		var count = 0
 		window.addMouseListener(
 		   // 매번 새로운 객체가 생성된다. 싱글톤이 아니다.
 			object : MouseAdapter() {
 				override fun mouseClicked(e: MouseEvent) {
 					count++
 				}
 				override fun mouseEntered(e: MouseEvent) {
 					..
 				}
 			}
 		}
 	}
 ~~~
 
 
### B.8.8 중첩 클래스와 내부 클래스
* 중첩(nested)클래스와 내부(inner)클래스는 모두 다른 클래스 내부에 선언된 클래스를 말한다.
* 중첩/내부 클래스의 멤버는 이들을 포함하는 외부 클래스에서 엑세스 할 수 있다.
* 중첩 클래스에서는 외부 클래스의 멤버를 참조할 수 없다. (자바의 static 중첩 클래스와 동일하다.)
* 내부 클래스에서는 외부 클래스의 멤버를 참조할 수 있다.
* 중첩 클래스는 별도의 키워드를 사용하지 않고, 내부 클래스는 **inner** 키워드를 사용한다.

 ~~~ kotlin
	class ClassOuter {
	    private val bar: Int = 1
	    class Nested {
	        fun funcNested() = 2 // fun funcNested() = bar 는 에러이다.
	    }
	
	    inner class Inner {
	        fun funcInner() = bar
	    }
	}
	
	fun main(args: Array<String>) {
	    val result1 = ClassOuter.Nested().funcNested()
	    println(result1) // 2가 출력
	
	    val result2 = ClassOuter().Inner().funcInner()
	    println(result2) // 1이 출력
	}
 ~~~
 
 ~~~ java
   // java code
	public final class ClassOuter {
	   private final int bar = 1;
	
	   public static final class Nested {
	      public final int funcNested() {
	         return 2;
	      }
	   }
	
	   public final class Inner {
	      public final int funcInner() {
	         return ClassOuter.this.bar;
	      }
	   }
	}

 ~~~
 
 
### B.8.9 데이터 클래스
* 클래스를 정의할 때 **data** 키워드를 사용하면 데이터 클래스로 간주된다.
* equals(), hashCode(), toString()이 자동 생성된다.
* 자동 생성되는 함수들이 데이터 클래스의 속성을 처리하게 하려면 반드시 **기본 생성자에 속성을 지정**해야 한다.
* **해체 선언(destructuring declaration)** 이라는 신기한 기능이 있다.
 * val (fname, fage, ftel) = f1 

 ~~~ kotlin
 
    data class Friend7(val name: String, val age: Int, val tel: String)
     
     
    fun main(args: Array<String>) {
	    val test = Friend7("이름", 10, "010-1111-2222")
	    println(test)
	
	    // 아래 java로 변환된 코드로 이해하자. 
	    // test객체의 속성을 추출하여 좌측의 변수에 대입한다.
	    val (fname, fage, ftel) = test
	    println("$fname, $fage, $ftel")
	}
 ~~~
 
 ~~~ java
	public final class Friend7 {
	   @NotNull
	   private final String name;
	   private final int age;
	   @NotNull
	   private final String tel;
	
	   @NotNull
	   public final String getName() {
	      return this.name;
	   }
	
	   public final int getAge() {
	      return this.age;
	   }
	
	   @NotNull
	   public final String getTel() {
	      return this.tel;
	   }
	
	   public Friend7(@NotNull String name, int age, @NotNull String tel) {
	      Intrinsics.checkParameterIsNotNull(name, "name");
	      Intrinsics.checkParameterIsNotNull(tel, "tel");
	      super();
	      this.name = name;
	      this.age = age;
	      this.tel = tel;
	   }
	
	   // 각각의 속성에 대해 componentN 형태의 메소드를 생성한다.
	   @NotNull
	   public final String component1() {
	      return this.name;
	   }
	
	   public final int component2() {
	      return this.age;
	   }
	
	   @NotNull
	   public final String component3() {
	      return this.tel;
	   }
	
	   @NotNull
	   public final Friend7 copy(@NotNull String name, int age, @NotNull String tel) {
	      Intrinsics.checkParameterIsNotNull(name, "name");
	      Intrinsics.checkParameterIsNotNull(tel, "tel");
	      return new Friend7(name, age, tel);
	   }
	
	   // $FF: synthetic method
	   // $FF: bridge method
	   @NotNull
	   public static Friend7 copy$default(Friend7 var0, String var1, int var2, String var3, int var4, Object var5) {
	      if ((var4 & 1) != 0) {
	         var1 = var0.name;
	      }
	
	      if ((var4 & 2) != 0) {
	         var2 = var0.age;
	      }
	
	      if ((var4 & 4) != 0) {
	         var3 = var0.tel;
	      }
	
	      return var0.copy(var1, var2, var3);
	   }
	
	   public String toString() {
	      return "Friend7(name=" + this.name + ", age=" + this.age + ", tel=" + this.tel + ")";
	   }
	
	   public int hashCode() {
	      return ((this.name != null ? this.name.hashCode() : 0) * 31 + this.age) * 31 + (this.tel != null ? this.tel.hashCode() : 0);
	   }
	
	   public boolean equals(Object var1) {
	      if (this != var1) {
	         if (var1 instanceof Friend7) {
	            Friend7 var2 = (Friend7)var1;
	            if (Intrinsics.areEqual(this.name, var2.name) && this.age == var2.age && Intrinsics.areEqual(this.tel, var2.tel)) {
	               return true;
	            }
	         }
	
	         return false;
	      } else {
	         return true;
	      }
	   }
	}
	
	public static final void main(@NotNull String[] args) {
      Friend7 test = new Friend7("이름", 10, "010-1111-2222");
      System.out.println(test);
      // val (fname, fage, ftel) = test 가 아래와 같이 동작한다.
      String fname = test.component1();
      int fage = test.component2();
      String ftel = test.component3();
      
      
      String var7 = null;
      var7 = "" + fname + ", " + fage + ", " + ftel;
      System.out.println(var7);
   }
 ~~~
 
 
### B.8.10 클래스 위임
* 코드를 재사용하는 **클래스 상속**은 슈퍼 클래스가 변경되면 서브 클래스에 영향을 줄 수 있다. 또한 슈퍼 클래스의 구현 부분이 서브 클래스에 노출 된다.
* 상속의 단점을 보완하면서 코드를 재사용하는 방법이 **위임(delegation)**이다.
 * 해당 일을 처리할 수 있는 객체의 코드를 재사용
 * 상속처럼 클래스 간의 계층 관계를 구성하지 않아도 된다
 * 위 내용을 직접 구현할 수도 있으나, 코틀린의 **by** 키워드를 사용하면 적은 양의 코드로 쉽게 구현할 수 있다.
* 도형 클래스로 알아보자.

 ~~~ kotlin
	open class Figure() {
	    open fun draw() {}
	    open fun fill() {}
	}
	
	class Rectangle() : Figure() {
	    override fun draw() {
	        println("Draw rectangle")
	    }
	
	    override fun fill() = println("Fill rectangle")
	}
	
	class Circle() : Figure() {
	    override fun draw() = println("Draw circle")
	
	    override fun fill() = println("Fill circle")
	}
	
	class Window(val figure: Figure) {
	    fun drawFigure() {
	        figure.draw()
	    }
	
	    fun fillFigure() {
	        figure.fill()
	    }
	}
	
	fun main(args: Array<String>) {
	    val r = Rectangle()
	    val c = Circle()
	
	    // 클래스의 상속과 다형성으로 위임 구현
	    Window(r).drawFigure()
	    Window(r).fillFigure()
	    Window(c).drawFigure()
	    Window(c).fillFigure()
	
	}
 ~~~
 
* **by** 키워드를 사용하여 구현한 위임 예시

 ~~~ kotlin
	interface Figure1 {
	    fun draw() {}
	    fun fill() {}
	}
	
	class Rectangle1() : Figure1 {
	    override fun draw() {
	        println("Draw rectangle")
	    }
	
	    override fun fill() = println("Fill rectangle")
	}
	
	class Circle1() : Figure1 {
	    override fun draw() = println("Draw circle")
	
	    override fun fill() = println("Fill circle")
	}
	
	// Window클래스(delegate)에서는 위임을 받아 실행되는 인스턴스의 함수를 호출하는 코드가 필요 없다.
	// 함수를 오버라이딩 할 수도 있다.
	class Window1(val figure: Figure1) : Figure1 by figure {}
	
	fun main(args: Array<String>) {
	    val r = Rectangle1()
	    val c = Circle1()
	
	    Window1(r).draw()
	    Window1(r).fill()
	    Window1(c).draw()
	    Window1(c).fill()
	}
 ~~~
 
 ~~~ java
     // 위 Windows1 클래스의 변환 코드
     // 아래 코드가 by 키워드 하나로 정리됐다.
     public final class Window1 implements Figure1 {
	   @NotNull
	   private final Figure1 figure;
	
	   @NotNull
	   public final Figure1 getFigure() {
	      return this.figure;
	   }
	
	   public Window1(@NotNull Figure1 figure) {
	      Intrinsics.checkParameterIsNotNull(figure, "figure");
	      super();
	      this.figure = figure;
	   }
	
	   public void draw() {
	      this.figure.draw();
	   }
	
	   public void fill() {
	      this.figure.fill();
	   }
	}
 ~~~
 
 
### B.8.11 enum 클래스

~~~ kotlin
// enum 키워드를 사용한다
enum class Friendtype {
    SCHOOL, COMPANY, SNS, OTHERS
}

fun getFriendTypeName(friend: Friendtype) =
        when (friend) {
            Friendtype.SCHOOL -> "학교 친구"
            Friendtype.COMPANY -> "회사 친구"
            Friendtype.SNS -> "SNS 친구"
            Friendtype.OTHERS -> "기타 친구"
        }


fun main(args: Array<String>) {
    println(Friendtype.SCHOOL)
    // 0부터 시작한다.
    println(Friendtype.SCHOOL.ordinal)
    println(Friendtype.COMPANY.ordinal)
    // name이라는 기본 속성이 있다.
    println(Friendtype.COMPANY.name)
    println(Friendtype.valueOf("COMPANY"))

    // values() : 모든 항목의 이름을 배열로 반환한다.
    val friends = Friendtype.values()

    for (item in friends) {
        println(item)
    }

    println(getFriendTypeName(Friendtype.OTHERS))
}

// 아래와 같이 enum 클래스 항목의 속성을 우리가 정의할 수도 있다.
enum class RGBColor(val r: Int, val g: Int, val b: Int) {
    RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238);  // enum클래스의 멤버 함수를 정의하는 경우 세미콜론(;)을 써야한다.

    fun rgbValue() = (r * 256 + g) * 256 + b
}
~~~


### B.8.12 sealed 클래스
* enum 클래스의 확장
* enum과는 다르게 여러 인스턴스를 가질 수 있다.
* **sealed**(실드??) 키워드를 사용한다.

 > 언제, 어떻게 써먹지..?

~~~ kotlin
// 코틀린 1.1 이전에는 하위 클래스가 중첩되어야 했으나, 현재는 아래와 같이 사용할 수 있다.
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()

// when에 else가 필요 없다.
fun eval(expr: Expr): Double = when(expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
    // the `else` clause is not required because we've covered all the cases
}
~~~

## B.9 람다식
* 함수를 따로 정의하지 않고 간결하게 나타날 수 있으며, 이름을 지정하지 않아도 된다.
* 람다식은 값으로 처리되므로 변수에 저장하고 실행시킬 수 있다.
* 람다식은 다른 함수의 안자로 전달되어 실행될 수 있다.

### B.9.1 람다식 작성 방법

~~~ kotlin

// 기존 형태의 덧셈 함수
fun sum1(a: Int, b: Int): Int{
    return a + b
}

// 코틀린의 대입문 형태로 정의된 뎃셈 함수
fun sum2(a: Int, b: Int): Int = a + b

fun main(args: Array<String>) {
    println("합계는 ${sum1(10, 20)} 입니다.")
    println("합계는 ${sum2(10, 20)} 입니다.")

    // 람다식으로 작성된 덧셈 함수 #1
    val sum3: (Int, Int) -> Int = {x, y -> x + y}
    println("합계는 ${sum3(10, 20)} 입니다.")

    // 람다식으로 작성된 뎃셈함수 #2
    val sum4 = {a:Int, b:Int -> a + b}
    println("합계는 ${sum3(10, 20)} 입니다.")

}
~~~

### B.9.2 람다식 활용
~~~ kotlin
button.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View view) {
		/* 버튼 클랙시 실행될 코드 */
	}
}

button.setOnClickListener { view -> /* 버튼 클랙시 실행될 코드 */ }
~~~
 * 위와 같이 사용하려면 람다식에서 구현하는 인터페이스에 정의된 추상 함수가 하나만 있어야 한다.

~~~ kotlin
val list = listOf(1, 2, 3, 4, 5)
// 하나의 매개변수만 갖는 람다식의 경우 기본으로 생성되는 it으로 매개 변수를 대체할 수 있다.
println(list.filter { it % 2 == 0})

// 각각의 요소에 10을 더한 값을 갖는 새로운 list를 생성하고 출력
println(list.map {it + 10})


data class Friend (val name: String, val age: Int, val tel: String)

val fr = listOf(Friend("김선달", 30, "010-1111-2222"), Friend("홍길동", 25, "010-2222-3333"))
// list에서 30세 이상인 친구의 이름만 출력
// 객체의 멤버(속성이나 함수)는 클래스이름::멤버이름의 형태로 참조가 가능하다.
println(fr.filter {it.age >= 30}.map(Friend::name))
~~~


~~~ java
// friend 필터링 및 출력 관련 변환된 java code

      fr = CollectionsKt.listOf(new Friend[]{new Friend("김선달", 30, "010-1111-2222"), new Friend("홍길동", 25, "010-2222-3333")});
      Iterable $receiver$iv = (Iterable)fr;
      Collection destination$iv$iv = (Collection)(new ArrayList());
      Iterator var22 = $receiver$iv.iterator();

      Object item$iv$iv;
      while(var22.hasNext()) {
         item$iv$iv = var22.next();
         Friend it = (Friend)item$iv$iv;
         if (it.getAge() >= 30) {
            destination$iv$iv.add(item$iv$iv);
         }
      }

      $receiver$iv = (Iterable)((List)destination$iv$iv);
      destination$iv$iv = (Collection)(new ArrayList(CollectionsKt.collectionSizeOrDefault($receiver$iv, 10)));
      var22 = $receiver$iv.iterator();

      while(var22.hasNext()) {
         item$iv$iv = var22.next();
         String var24 = ((Friend)item$iv$iv).getName();
         destination$iv$iv.add(var24);
      }

      List var20 = (List)destination$iv$iv;
      System.out.println(var20);
~~~


### B.9.3 익명 함수
* 실행 가능한 코드 블록을 다른 함수의 인자로 전달하는 또 다른 방법
* 일반 함수와 유사하지만 함수 이름과 매개변수 타입을 갖지 않는다

~~~ kotlin
println(fr.filter { it.age >= 30})

// 위 코드를 익명함수를 사용하면 다음과 같이 작성할 수 있다.
println(fr.filter(fun (friend) = friend.age >= 30))

println(
	fr.filter(
		fun (friend): Boolean {
			return friend.age >= 30
		)
	)
}
~~~

* 람다식과 익명 함수 모두 사용 가능 범위의 외부 변수를 액세스할 수 있다.

~~~ kotlin
    val value: Int = 100
    val list2 = listOf(1, 2, 3, 4, 5)
    println(list2.map {it + value})
    println(list2.map(fun (it): Int { return it + value}))
~~~


### B.9.4 상위차 함수
* 상위차(higher-order) 함수는 다른 함수나 람다식을 인자로 받거나 반환할 수 있는 함수를 말한다.
* 위의 filter()나 map()함수는 람다식이나 익명 함수를 인자로 받아서 실행시킨다. 
* 인자로 받는 함수의 타입을 매개변수로 지정해야 한다.

~~~ kotlin
val sum3: (Int, Int) -> Int = {x, y -> x + y}
~~~
* 위 코드에서는 두 개의 Int 값을 인자로 받아 더한 값을 반환하여 sum3 변수에 저장한다.
* 여기서 **(Int, Int) -> Int**를 함수 타입이라고 한다.
* 매개 변수가 없는 경우는 **() -> Int**처럼 쓸수 있다.
* 아무것도 반환하지 않는 경우 **() -> Unit**처럼 반드시 Unit를 지정해야 한다.

~~~ kotlin
fun calc(value1: Int, value2: Int, execCode: (Int, Int) -> Int): Int {
    return execCode(value1, value2)
}

fun main(args: Array<String>) {
    val v1 = calc(2, 7, {a, b -> a * b})
    val v2 = calc(2, 7, {a, b -> a + b})
    val v3 = calc(2, 7, {a, b -> a - b})

    println(v1)
    println(v2)
    println(v3)
}
~~~


### B.9.5 인라인 함수
* 상위차 함수를 사용할 때는 런타임 시에 단점이 생길 수 있다.
* 각 함수가 객체로 동작하므로 메모리 할당과 사용 및 호출에 따른 시스템의 부담이 따른다.
* 함수 외부의 변수를 사용가능하므로 변수 액세서에 따른 부담도 추가된다.
* 이럴 때 **인라인(inline)**함수를 사용하면 그런 단점을 해소할 수 있다.

~~~ kotlin
inline fun calc(value1: Int, value2: Int, execCode: (Int, Int) -> Int): Int {
    return execCode(value1, value2)
}
~~~

* **inline** 함수를 호출하는 모든 코드는 컴파일러에 의해 해당 함수의 바이트 코드로 복사 및 대체된다.(인라인 처리된다.)
* 따라서 **컴파일된 코드의 크가가 증가한다.**
* 그러나 **런타임 시의 성능은 향상된다.**
* 반복 실행되는 루프에 람다식을 포함하는 함수는 인라인 함수로 지정하는 것이 좋다.
* 람다식이 인라인 함수의 인자로 전달될 때는 해당 함수와 람다식 모두 인라인 처리된다.
* 인자로 전달되는 람다식이 다른 객체에 저장되는 경우 해당 람다식은 인라인 처리 될 수 없다.
* 람다식을 인라인 처리하고 싶지 않거나, 또는 인라인 처리가 불가능한 경우 **noinlin**키워드를 지정하면 된다.

 ~~~ kotlin
inline fun makeMoney(lambda1: () -> Int, noinline lambda2: (Int) -> Unit) {
	// 실행될 코드
}
~~~
* 인라인 처리된 람다식은 해당 인라인 함수 내부에서만 호출될 수 있으며, 인라인 인자로만 전달 될 수 있다.
* noinline 지정된 람다식은 일반 람다식과 동일하게 변수에 저장하거나 함수의 인자로 전달할 수 있다.

## B.10 예외 처리
* 코틀린 프로그램에서 발생하는 모든 에러는 클래스로 정의된 **예외(Exception)로 처리된다.
* 예외 발생과 처리 매커니즘이 언어 자체에 배려되어 있다.(자바와 거의 동일, 실제로 대부분 자바의 예외 클래스를 사용)
* 코틀린의 모든 에러 및 예외 클래스는 Throwable을 상속 받았다.
* 예외 객체와 메시지 및 스택 기록을 속성으로 가지고 있다.

### B.10.1 try~catch~finally
* 자바와 마찬가지로, throw문을 사용해서 해당 예외 객체를 던지면 된다.

~~~ kotlin
throw MyException("예외 발생!")

try {
	// 실행중 예외가 발생할 수 있는 코드
} catch (e: SomeException) {
   // SomeException이 발생한 경우 처리하는 코드
} finally {
   // finally 블록은 생략 가능하다.
}
~~~


### B.10.2 Checked와 UnChecked 예외
* Checked 예외
 * 자바의 경우 메서드에서 throw 문으로 예외를 발생 시키는 경우 해당 메서드 헤더에 throws 키워드로 명시해야 한다.
 * 위 메서드를 사용하는 다른 메서드에서 예외를 처리하지 않는 경우 컴파일 에러가 발생
* 코틀린에서는 발생하는 예외 처리를 강제하지 않는다.
* 자바와의 호환을 위해 @Throw(SomeExceptin::class) annotation을 사용할 수 있다.

### B.10.3 try~catch는 표현식이다.

* 아래 처럼 대입문에 사용 가능하다.

~~~ kotlin
val err: Int? = try { parseInt(vale)}
					catch (e: NumberFormatException) { null }
~~~

* throw도 표현식으로 간주된다.
 
~~~ kotlin
val s = fr.name ?: throw IllegalArgumentException("이름을 입력하세요")
~~~
* fr.name의 값이 null이 아니면 그 값을 s에 넣고 null이면 IllegalArgumentException을 발생 시킨다.
* 이때 throw의 표현식의 타입은 **Nothing**이라는 키워드로 나타내는 특별한 타입이다.

~~~ kotlin
val err = fr.name ?: fail("이름을 입력하세요)

fun fail(message: String): Nothing {
	throw IllegalArgumentException(message)
}
~~~


## B.11 package와 import
* 코틀린에서는 하나의 소스 코드(.kt) 파일에 하나 이상의 클래스, 함수, 속성을 포함시킬 수 있다.
* 이것들을 최상위 수준의 클래스, 함수, 속성이라고 한다.
* 자바와 마찬가지로 패키지 단위로 클래스, 함수, 속성들을 모아 두고 사용할 수 있다.
* 이때는 소스 코드 파일의 맨 앞에 package 키워드를 지정하면 된다.
* 클래스, 함수, 속성들이 다른 소스코드 파일에 있더라도 패키지가 같으면 바로 사용할 수 있다.
* 자바와 다르케 패키지 구조와 디렉토로 구조가 일치하지 않아도 된다.(가급적 일치시켜 사용하는게 좋다.)
* 다른 패키지에 있는 클래스, 함수, 속성들을 사용하려면 **import**문을 사용해서 그것들의 위치를 알려주어야 한다.


---
끝.