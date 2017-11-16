---
layout: post
title: "Sealed Classes"
description: "Sealed Classes"
categories: [kotlin]
tags: [kotlin, sealed, class]
redirect_from:
  - /2017/11/16/

---

# Sealed Classes

* [kotlin docs(sealed classes)](https://kotlinlang.org/docs/reference/sealed-classes.html)



이름, 전화번호, 이메일 데이터를 표현하는 interface와 클래스를 정의하고 해당 데이터를 출력하는 메소드를 정의한다고 하면..

~~~ kotlin
interface Data

class NameData(val value: String) : Data
class PhoneData(val value: String, val type: String) : Data
class EmailData(val value: String, val type: String) : Data


fun getDisplayString(data: Data) =
        when(data) {
            is NameData -> data.value
            is PhoneData -> "${data.type} : ${data.value}"
            is EmailData -> "${data.type} : ${data.value}"
            else -> throw IllegalArgumentException("Unknown data")
        }
~~~

> getDisplayString 함수에서 어떤 데이터 타입이 올지 모르니 **else**를 추가해줘야 한다.



sealed 클래스를 적용해보면..

~~~ kotlin
sealed class Data {
    class NameData(val value: String) : Data()
    class PhoneData(val value: String, val type: String) : Data()
    class EmailData(val value: String, val type: String) : Data()
}

fun getDisplayString(data: Data) =
        when(data) {
            is Data.NameData -> data.value
            is Data.PhoneData -> "${data.type} : ${data.value}"
            is Data.EmailData -> "${data.type} : ${data.value}"
            // else를 사용하지 않아도 된다.
        }

fun main(args: Array<String>) {
    val tempData = Data.PhoneData("01011112222", "휴대폰")
    println(getDisplayString(tempData))
}
~~~



Data 클래스에 새로운 NickNameData가 추가된다고 하면..

~~~ kotlin
sealed class Data {
	// ...
    class NickNameData(val value: String) : Data()
}

fun getDisplayString(data: Data) =
        when(data) { // <-- NickNameData의 분기나, else문을 추가하라고 에러가 발생한다.
            is Data.NameData -> data.value
            is Data.PhoneData -> "${data.type} : ${data.value}"
            is Data.EmailData -> "${data.type} : ${data.value}"
            // else를 사용하지 않아도 된다.
        }
~~~



다른 파일에서 Data 클래스를 상속하려고 하면 에러가 발생한다. 즉, 같은 파일내에서만 sealed class를 상속한 클래스를 정의 할 수 있다.

* sealed class는 private 생성자를 가진다.

위 예시처럼 클래스를 중첩하지 않고 아래와 같이 사용할 수 있다.(data 클래스도 사용이 가능하다. ) - 코틀린 1.1부터..

~~~ kotlin
sealed class Data
data class NameData(val value: String) : Data()
data class PhoneData(val value: String, val type: String) : Data()
data class EmailData(val value: String, val type: String) : Data()
data class NickNameData(val value: String, val type: String) : Data()


fun getDisplayString(data: Data) =
        when(data) {
          	// "Data."이 제거됐다.
            is NameData -> data.value
            is PhoneData -> "${data.type} : ${data.value}"
            is EmailData -> "${data.type} : ${data.value}"
            is NickNameData -> data.value
            // else를 사용하지 않아도 된다.
        }
~~~





> sealed interface는 만들 수 없다.

