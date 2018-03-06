---
layout: post
title: MongoDB Basics - <strong>Install</strong>
subtitle: MongoDB의 기본 개념과 설치
categories: mongodb
section: mongodb
description: MongoDB의 기본 개념과 설치 RDMS와 MongoDB의 비교
---

* TOC
{:toc}

![MongoDB Logo](/img/mongo-db-logo.png)

# 1. Introduction

[MongoDB](https://www.mongodb.com/)는 Document-Oriented(문서 지향적) [NoSQL](https://ko.wikipedia.org/wiki/NoSQL) 데이터베이스이다. 오픈 소스이며 엔진은 C++로 작성되었다.

NoSQL이란 Not Only SQL의 약자로서 기존의 RDBMS(관계형 데이터베이스)의 한계를 극복하기 위한 새로운 형태의 데이터베이스이다.

**Document**

[Document](https://docs.mongodb.com/manual/core/document/)는 RDMS의 record와 유사한 개념으로 JSON objects 형태의 key-value의 쌍으로 이루어진 데이터 구조로 구성된다.

value에는 다른 document, array, document array가 포함될 수 있다.

![MongoDB Document](/img/mongodb-document.png)
{: .w-450}

MongoDB Document
{: .desc-img}

Document의 형태를 살펴보자.

```
{
  _id: ObjectId("5099803df3f4948bd2f98391"),
  name: { first: "Alan", last: "Turing" },
  birth: new Date('Jun 23, 1912'),
  death: new Date('Jun 07, 1954'),
  contribs: [ "Turing machine", "Turing test", "Turingery" ],
  views : NumberLong(1250000)
}
```

각 Document는 `_id`라는 고유한 값을 갖는다. 이 고유한 값은 시간/머신ID/프로세스ID/순차번호로 구성되며 값의 고유성을 보장한다.

**Collection**

[Collection](https://docs.mongodb.com/manual/core/databases-and-collections/#collections)은 RDMS의 table과 유사한 개념으로 Document들의 집합으로 구성된다.

**Database**

[Database](https://docs.mongodb.com/manual/core/databases-and-collections/#databases)는 Collection들의 물리적인 컨테이너이다.

Database는 0개 이상의 Collection들의 집합으로 구성되며 Collection은 0개 이상의 Document로 구성되고 Document는 1개 이상의 field로 구성된다.

![MongoDB Structure](/img/mongodb-structure.png)
{: .w-450}

MongoDB Structure
{: .desc-img}

# 2. RDMS와 MongoDB의 비교

**Terms**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>Database</td>
    <td>Database</td>
  </tr>
  <tr>
    <td>Table</td>
    <td>Collection</td>
  </tr>
  <tr>
    <td>Tuple / Row</td>
    <td>Document or BSON document</td>
  </tr>
  <tr>
    <td>Column</td>
    <td>Field</td>
  </tr>
  <tr>
    <td>Table Join</td>
    <td>Embedded Documents & Linking</td>
  </tr>
  <tr>
    <td>Primary Key</td>
    <td>Primary Key (&#95;id)</td>
  </tr>
</table>

**Database Server**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>mysqld</td>
    <td>mongod</td>
  </tr>
</table>

**Database Client**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>mysql</td>
    <td>mongo</td>
  </tr>
</table>

**SQL**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <th colspan="2">Insert</th>
  </tr>
  <tr>
    <td>insert into users ("name", "city") values("lee", "seoul")</td>
    <td>db.users.insert({ name: "lee", city: "seoul" })</td>
  </tr>
  <tr>
    <th colspan="2">Select</th>
  </tr>
  <tr>
    <td>select * from users where name="lee"</td>
    <td>db.users.find({ name: "lee" })</td>
  </tr>
  <tr>
    <th colspan="2">Update</th>
  </tr>
  <tr>
    <td>update users set city="busan" where name="lee"</td>
    <td>db.users.update({ name: "lee" }, { $set: { city: "busan" }})</td>
  </tr>
  <tr>
    <th colspan="2">Delete</th>
  </tr>
  <tr>
    <td>delete from users where name="lee"</td>
    <td>db.users.remove({ name: "lee" })</td>
  </tr>
</table>

# 3. 특징

- Schema-less하다. 이는 RDMS처럼 고정 [Schema(스키마)](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%8A%A4%ED%82%A4%EB%A7%88)가 존재하지 않는다는 뜻으로 같은 Collection 내에 있더라도 document level의 다른 Schema를 가질 수 있다는 의미이다. RDBMS는 각 테이블마다 동일한 Schema를 가져야 하며 스키마를 변경하고자 한다면 모든 데이터를 변경하여야 한다. 하지만 MongoDB는 하나의 Collection내의 Document가 각각 다른 스키마를 갖을 수 있다. 이러한 특성을 동적 스키마라 한다. 이것은 collection 내의 document가 각자의 고유한 field를 가질 수 있다는 것을 의미한다.

- RDMS와 같은 JOIN이 없어 Table JOIN은 효과적이지 않지만(불가능하지는 않다) CRUD Query는 고속으로 동작한다. MongoDB는 스키마를 디자인할 때 하나의 document에 최대한 많은 데이터를 포함시킨다. 예를 들어 RDBMS의 경우, 포스트의 댓글에 대한 스키마를 각각의 테이블로 분리하고 JOIN하여 사용하는 것이 일반적이지만 MongoDB의 경우, 하나의 document에 포스트의 sub document로 댓글을 포함시킨다. 이러한 구조는 간편하고 빠른 Query를 가능하게 한다.

- Scalability(규모 가변성, 확장성)이 우수하며 Sharding(여러 개의 데이터베이스에 데이터를 분할하는 기술) 클러스터 구축도 가능하다.

![mongodb sharding](/img/mongodb-sharding.gif)

MongoDB Sharding Clustering
{: .desc-img}

# 4. Install

## 4.1 Windows

**1. download & install**

[MongoDB download](https://www.mongodb.com/download-center?jmp=homepage#community)에서 자신의 사양에 맞는 버전을 다운로드하여 install한다.

Community Server 3.2.9을 기준으로 설명한다.

**2. 기본 데이터베이스 디렉터리 생성**

기본 데이터베이스 디렉터리(C:\\data\\db)를 생성한다. 기본 데이터베이스 디렉터리에 데이터가 store된다.

```bash
mkdir C:\data\db
```

로그 파일이 저장될 디렉터리를 생성한다.

```bash
mkdir C:\mongodb\log
```

**3. 환경설정 파일 생성**

mongod.cfg 환경설정 파일을 생성하여 C:\\mongodb 내에 저장한다.

```bash
cd C:\\mongodb
C:\\mongodb echo >> mongod.cfg
```

mongod.cfg 파일을 열어서 아래와 같이 편집한다.

```bash
##Which IP address(es) mongod should bind to.
bind_ip = 127.0.0.1

##Which port mongod should bind to.
port = 27017

##I set this to true, so that only critical events and errors are logged.
quiet = true

##store data here
dbpath=C:\data\db

##The path to the log file to which mongod should write its log messages.
logpath=C:\mongodb\log\mongo.log

##I set this to true so that the log is not overwritten upon restart of mongod.
logappend = true

##log read and write operations
diaglog=3

##It ensures write durability and data consistency much as any journaling scheme would be expected to do.
##Only set this to false if you don’t really care about your data (or more so, the loss of it).
journal = true

##For mongodb 32 bit
storageEngine = mmapv1
```

**4. DB 기동**

MongoDB가 설치된 디렉터리로 이동한다. Community Server 3.2.9의 경우, 설치 디렉터리를 별도 지정하지 않았다면 `C:\Program Files\MongoDB\Server\3.2\bin\`에 설치된다.

```bash
cd C:\Program Files\MongoDB\Server\3.2\bin
```

MongoDB Server를 기동한다.

```bash
C:\Program Files\MongoDB\Server\3.2\bin>mongod --config c:\mongodb\mongod.cfg
2016-08-18T16:41:10.289+0900 W CONTROL  [main] --diaglog is deprecated and will be removed in a future release
2016-08-18T16:41:10.295+0900 I COMMAND  [main] diagLogging level=3
2016-08-18T16:41:10.303+0900 I COMMAND  [main] diagLogging using file C:\data\db/diaglog.57b56696
```

새로운 CMD 창에서 MongoDB Client를 기동한다.

```bash
C:\Program Files\MongoDB\Server\3.2\bin>mongo
MongoDB shell version: 3.2.9
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
Server has startup warnings:
2016-08-18T16:57:15.720+0900 W CONTROL  [main] --diaglog is deprecated and will be removed in a future release
2016-08-18T16:57:16.095+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] ** WARNING: This 32-bit MongoDB binary is deprecated
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2016-08-18T16:57:16.097+0900 I CONTROL  [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2016-08-18T16:57:16.097+0900 I CONTROL  [initandlisten]
>
```

**5. DB 중지**

MongoDB 클라이언트를 기동시킨 CMD창에서 admin 데이터베이스로 변경한 후 DB를 중지시킨다. 이때 MongoDB 서버 또한 중지된다.

```bash
> show dbs
local  0.078GB
> use admin
switched to db admin
> db.shutdownServer()
server should be down...
2016-08-18T17:08:07.726+0900 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2016-08-18T17:08:08.731+0900 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, reason: errno:10061 대상 컴퓨터에서 연결을 거부했으므로 연결하지 못했습니다.
2016-08-18T17:08:08.734+0900 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed
> quit()

C:\Program Files\MongoDB\Server\3.2\bin>
```

## 4.2 Mac

**1. download & install**

Mac의 경우, Homebrew를 사용하여 install할 수 있다.

```bash
$ brew update && brew install mongodb

$ mongo -version
MongoDB shell version: 3.2.8
```

**2. 기본 데이터베이스 디렉터리 생성**

기본 데이터베이스 디렉터리(/data/db)를 생성한다. 기본 데이터베이스 디렉터리에 데이터가 store된다.

```bash
$ mkdir -p /data/db
```

**3. DB 기동**

MongoDB Server를 기동한다.

```bash
$ mongod
```

새로운 터미널에서 MongoDB Client를 기동한다.

```bash
$ mongo
```

**4. DB 중지**

MongoDB 클라이언트를 기동시킨 터미널에서 DB를 중지시킨다. 이때 MongoDB 서버 또한 중지된다.

```bash
> use admin
switched to db admin
> db.shutdownServer()
server should be down...
2016-08-19T14:15:10.057+0900 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2016-08-19T14:15:10.058+0900 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, reason: errno:61 Connection refused
2016-08-19T14:15:10.058+0900 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed
> quit()

$ ps wuax | grep mongo
leeungmo    15609   0.0  0.0  2454296    820 s001  S+    2:16PM   0:00.00 grep mongo
```

## 4.3 mLab Hosting service

MongoDB를 직접 설치하지 않고 mLab Hosting service를 이용할 수도 있다.

[mLab](https://mlab.com/)에 접속한 후, 회원가입을 하고 로그인한다.

![mlab-1](/img/mlab-1.png)

데이터베이스 생성
{: .desc-img}

![mlab-2](/img/mlab-2.png)

플랜 선택
{: .desc-img}

![mlab-3](/img/mlab-3.png)

데이터베이스명 설정
{: .desc-img}

![mlab-4](/img/mlab-4.png)

데이터베이스 생성 확인
{: .desc-img}

![mlab-5](/img/mlab-5.png)

데이터베이스 선택
{: .desc-img}

![mlab-6](/img/mlab-6.png)

계정 생성 화면 진입
{: .desc-img}

![mlab-7](/img/mlab-7.png)

계정 생성
{: .desc-img}

![mlab-8](/img/mlab-8.png)

데이터베이스 생성 완료
{: .desc-img}

## 4.4 RoboMongo

터미널에서 mongo 명령어를 사용하여 데이터베이스에 접속하고 조회할 수 있으나 불편할 수 있으므로 GUI MongoDB 클라이언트인 Robomongo 설치하여 보자.

- [Robomongo 다운로드](https://robomongo.org/download)

![robomongo-1](/img/robomongo-1.png)

Robomongo 실행 화면
{: .desc-img}

![robomongo-2](/img/robomongo-2.png)

접속 정보 설정 화면
{: .desc-img}

![robomongo-3](/img/robomongo-3.png)

계정 정보 설정 화면
{: .desc-img}

![robomongo-4](/img/robomongo-4.png)

MongoDB 접속
{: .desc-img}

![robomongo-5](/img/robomongo-5.png)

MongoDB 접속 성공
{: .desc-img}

# Reference

* [The MongoDB 3.2 Manual](https://docs.mongodb.com/manual/)

* [Install MongoDB](https://docs.mongodb.com/manual/installation/)

* [mLab](https://mlab.com/)
