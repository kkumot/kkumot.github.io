---
layout: post
title: "Room 살펴보기 (feat. kotlin)"
description: "Room 라이브러리를 사용해보기"
categories: [library, kotlin]
tags: [room, kotlin, library, sqlite]
redirect_from:
  - /2017/10/31/
---

# 라이브러리 의존성 설정

## build.gradle

~~~
// kotlin annotation processing tool
apply plugin: 'kotlin-kapt'

dependencies {
    implementation 'android.arch.persistence.room:runtime:1.0.0-rc1'
    kapt 'android.arch.persistence.room:compiler:1.0.0-rc1'
}
~~~

# DB에 저장할 모델 만들기

## LocationResultModel.kt
* 위치 정보 정확도 파악을 위해 위치 데이터를 저장하는 모델을 만들었다.


~~~ kotlin
@Entity(tableName = "location_result")
data class LocationResult (
    val provider : String,
    val latitude : Double,
    val longitude : Double,
    val accuracy : Double,
    @ColumnInfo(name = "create_date_time")
    val createDateTime : String) {

    @PrimaryKey(autoGenerate = true)
    var id: Long = 0
}
~~~

# DAO 만들기
## LocaionResultDao.kt
* 데이터 조회/추가/수정/삭제를 하는 dao를 생성했다.

~~~ kotlin
@Dao
interface LocationResultDao {

    @Query("SELECT * FROM location_result")
    fun findAll(): List<LocationResult>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun create(locationResult: LocationResult)

    @Update
    fun update(locationResult: LocationResult)

    @Delete
    fun delete(locaionResult: LocationResult)
}
~~~

# Database 클래스 정의
## Database.kt

* entity클래스, version, schema export option등을 정의한다.

~~~ kotlin
@Database(entities = arrayOf(LocationResult::class), version = 1, exportSchema = false)
abstract class Database : RoomDatabase() {
    abstract fun locationResultDao(): LocationResultDao
}
~~~

# database 객체 생성
* Application 객체에 싱글톤으로 정의

## ApplicationImpl.kt

~~~ kotlin
class ApplicationImpl : Application() {

    companion object {
        private lateinit var INSTANCE: ApplicationImpl
        fun getInstance() = INSTANCE

        lateinit var database: Database
    }
    
    override fun onCreate() {
        super.onCreate()
        INSTANCE = this

        database = Room.databaseBuilder(this, Database::class.java, "location-result-db").build()
    }
}
~~~


# DB에 데이터 추가/조회하기
## 데이터 추가

~~~ kotlin
async {
	ApplicationImpl.database.locationResultDao().create(LocationResult("Fused", location.latitude, location.longitude, 0.0, getCurrentDateTime()))
}
~~~

## 데이터 조회

~~~ kotlin
async {
    val result = ApplicationImpl.database.locationResultDao().findAll()
    Log.d("LocationSample", result.toString())
}
~~~

