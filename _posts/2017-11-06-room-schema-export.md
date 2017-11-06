---
layout: post
title: "Room - export schema"
description: "Room 라이브러리 사용시 schema export 하기"
categories: [library, kotlin]
tags: [room, kotlin, library, sqlite]
redirect_from:
  - /2017/11/06/
---

* room을 적용하고 빌드하니 아래와 같은 메시지가 출력되었다.

> warning: Schema export directory is not provided to the annotation processor so we cannot export the schema. You can either provide `room.schemaLocation` annotation processor argument OR set exportSchema to false.

* stackoverflow 에서 찾은 답변
 * [https://stackoverflow.com/questions/44322178/room-schema-export-directory-is-not-provided-to-the-annotation-processor-so-we](https://stackoverflow.com/questions/44322178/room-schema-export-directory-is-not-provided-to-the-annotation-processor-so-we)

* room doc
 * [https://developer.android.com/reference/android/arch/persistence/room/Database.html#exportSchema()](https://developer.android.com/reference/android/arch/persistence/room/Database.html#exportSchema())

# 정리하면..
* DB schema를 설정을 통해 export할 수 있다! 필수는 아니나, DB 버전 관리에 용이하다. export된 schema를 앱과 함께 배포하지는 말아라.

## Schema export

* room.schemaLocation을 build.gradle 설정에 추가

~~~ groovy
// build.gradle

android {
    //...
    defaultConfig {
        //...
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = ["room.schemaLocation": "$projectDir/schemas".toString()]
            }
        }
    }
}
~~~

* 위에서 설정한 디렉토리 밑에 RoomDatabase를 상속받은 클래스 패키지 폴더에 버전.json 파일이 생성된다.

~~~ json
{
  "formatVersion": 1,
  "database": {
    "version": 1,
    "identityHash": "099390f8b7fbb65c66ec03eb92989ebb",
    "entities": [
      {
        "tableName": "location_result",
        "createSql": "CREATE TABLE IF NOT EXISTS `${TABLE_NAME}` (`id` INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, `provider` TEXT NOT NULL, `latitude` REAL NOT NULL, `longitude` REAL NOT NULL, `accuracy` REAL NOT NULL, `create_date_time` TEXT NOT NULL)",
        "fields": [
          {
            "fieldPath": "id",
            "columnName": "id",
            "affinity": "INTEGER",
            "notNull": true
          },
          {
            "fieldPath": "provider",
            "columnName": "provider",
            "affinity": "TEXT",
            "notNull": true
          },
          {
            "fieldPath": "latitude",
            "columnName": "latitude",
            "affinity": "REAL",
            "notNull": true
          },
          {
            "fieldPath": "longitude",
            "columnName": "longitude",
            "affinity": "REAL",
            "notNull": true
          },
          {
            "fieldPath": "accuracy",
            "columnName": "accuracy",
            "affinity": "REAL",
            "notNull": true
          },
          {
            "fieldPath": "createDateTime",
            "columnName": "create_date_time",
            "affinity": "TEXT",
            "notNull": true
          }
        ],
        "primaryKey": {
          "columnNames": [
            "id"
          ],
          "autoGenerate": true
        },
        "indices": [],
        "foreignKeys": []
      }
    ],
    "setupQueries": [
      "CREATE TABLE IF NOT EXISTS room_master_table (id INTEGER PRIMARY KEY,identity_hash TEXT)",
      "INSERT OR REPLACE INTO room_master_table (id,identity_hash) VALUES(42, \"099390f8b7fbb65c66ec03eb92989ebb\")"
    ]
  }
}

~~~


## Schema export 하지 않기.

* RoomDatabase를 상속받은 클래스에 **exportSchema = false** 옵션을 추가한다.

~~~ kotlin
@Database(entities = arrayOf(LocationResult::class), version = 1, exportSchema = false)
abstract class Database : RoomDatabase() {
    //...
}
~~~