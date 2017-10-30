---
layout: post
title: "코틀린 시간 계산 관련 라이브러리"
description: "코틀린 시간 계산 관련 라이브러리"
categories: [kotlin, library]
tags: [kotlin, library, time]
redirect_from:
  - /2017/10/30/
---

# 코틀린 시간 계산 관련 라이브리러

* [https://github.com/kizitonwose/Time](https://github.com/kizitonwose/Time)

* 아래와 같이 사용하던 코드를...

~~~ kotlin
var SECOND = 10 * 1000
var MINUTE = SECOND * 60
...
~~~

* 이렇게 바꿀 수 있다.

~~~ kotlin
val SECOND = 1.seconds
val fiveMinutes = 5.minutes

// 아래와 같이 연산자도 사용 가능하다.
val tempSeconds = 10.seconds + 3.minutes

// 변환도 가능하다.
val fourDaysInHours = 4.days.inHours

// 비교도 된다.
50.seconds < 2.hours // true
120.minutes == 2.hours // true
100.milliseconds > 2.seconds // false
48.hours in 2.days // true

// 필요한건 만들어 써도 된다.
class Week : TimeUnit {
    // number of seconds in one week
    override val timeIntervalRatio = 604800.0
}
val fiveWeeks = Interval<Week>(5)
...

// Calendar 와 Thread, Android Handler를 위해 몇몇 함수를 확장해두었다.
~~~