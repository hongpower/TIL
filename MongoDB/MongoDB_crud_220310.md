# MongoDB

==== CMD 창 ====

- mongo 입력해서 mongo활성화

- JS 함수만들 수 있음. 

  - js지만 console.log대신에 print 사용가능

  - 예) `function test(){print('test')}`

- JS구조니까 당연히 변수 선언도:

  - var로 변수 만들기
  - `var lee = {name : 'lee-ss', midterm : {kor:70, eng:100}, final: {kor:100, eng:90, math:20, sci:50}, class:'de'}`
  - **var로 만든 변수는 일회성이라 한번 호출하면 사라짐!!**

- 구조

  - db > collection > document
  - collection이 sql의 table, document가 row, column이 field

  
  
- 스키마 고정 X -> 새롭게 추가되는 doc이 모든 필드 내용은 가지고 있지 않아도 상관 X



## CRUD

- 데이터베이스 조회 : `show dbs`
  - admin : root db. 관리자 db
  - config : shard에 대한 정보가 들어감
  - local : shard를 통해서 복제되지 않는 (특정 데이터에서만 사용하고 싶은)데이터 
  - test : 말그대로 test

- 데이터베이스 사용 : `use test`
- 현재 내가 사용하는 데이터베이스 설명 확인 : `db` 
- 컬렉션 조회 : `show collections`





### 데이터 조회

- db.collection명.find({filter(where)},{project(select)})

  - sql의 select와 동일
  - filter로 걸려져 나온 doc중에 원하는 field만 추출
  
- `db.multi.find()`= `db.multi.find({})`

- filter문 

  - `db.multi.find({'midterm.kor' : {$gt:50}})` -> midterm의 kor 점수가 50 이상인 document출력
  - `db.multi.find({'midterm.kor': {$exists: true}})` -> midterm의 kor 점수가 존재하는 doc출력
  - `db.multi.find({$and: [{kor : {$gt: 50}}, {kor : {$lt : 100}}]})` = `db.multi.find({kor:{$gt:50}, kor:{$lt:100}})` -> 조건 여러개 묶으면 and로 자동으로 연결
  - `db.multi.find({$and: [{kor : {$gte: 50}}, {kor : {$lte : 100}}]})`

- sort: 정렬(1 : asc, -1 : desc)
  - `  db.multi.find().sort({name : 1})` -> name기준으로 오름차
  - `  db.multi.find().sort({name : -1})` -> name기준으로 내림차

- limit: 출력할 doc 개수 지정 

  - `  db.multi.find().sort({kor: 1}).limit(1)`  -> 국어점수가 없는 애가 하단에 나타나서 출력

  - ` db.multi.find().sort({kor:-1}).limit(1)` -> 국어점수 1등 출력

- skip: 건너뛰기
  - `  db.multi.find().skip(1)` -> 첫번째 결과 건너뛰고출력

- project:

    - `db.multi.find({}, {name:1}) `-> 다 찾을 건데 name만 출력
      - project에 숫자 1이 들어간 field는 출력, 숫자 0이 들어간 field는 미출력

    - `db.multi.find({}, {_id:0, name:1})` -> id는 출력안하고 name만 출력
      - 만약 해당 필드에내용이 없다하더라도 그냥 출력!

- pretty:
  - myfriends의 documents들을 이쁘게 보여줘요
  - find했을 때 한줄로 쭉 나오던게 이쁘게 정렬돼서 보여줌
  - `db.myfriends.find().pretty()`




### 데이터 삽입

- db.collection명.insertOne/insert/insertMany()
- db.collection명.insertOne:
  - `db.jstest.insertOne({name : 'test', age : 100, class : 'de'})`-> 내가 굳이 jstest라는 collection을 안만들었어도 이렇게 document를 insert하면 자동 생성
  - `var lee = {name : 'lee-ss', midterm : {kor:70, eng:100}, final: {kor:100, eng:90, math:20, sci:50}, class:'de'}`, `db.multi.insertOne(lee)`
- db.collection명.insert:
  - `db.multi.insert({name:'hong-gd', class:'de', midterm:{kor: 100, eng: 60, math: 80}})` -> insert는 예전에 사용하는 거라서 결과창이 다르게 뜸 
- db.collection명.insertMany:
  - insertMany처럼 여러 doc을 삽입하는 경우에는 리스트로 딕셔너리를 묶어서 작성
  - `db.multi.insertMany([ {name: 'kim-sd', class:'ds', kor:100, eng:40, math:100},{name:'kang-hd', class:'ds', kor:88, eng:50, math:70}])`

- insertOne/insertMany는 3.2버전 이후로 나온거라 이전은 insert만 입력.
  - 현재도 insert라는 명령어는 잘되지만 왠만하면 insertOne이나 insertMany사용



### 데이터 갱신

- db.collection명.updateOne(filter조건, update, option)

  - 해당 document의 **필드 한 개만** 수정
  -  sql의 where가 filter고, update위치에 set작성하면 sql의 set 수정될내용

  - `db.multi.updateOne({name: /hong/}, {$set: {name: '홍길동'}})` -> 이름에 hong이 들어간 doc의 이름을 '홍길동'으로 변경.
    - 중요한거 name안에 single quotation 지우기!!


- db.collection명.updateMany(filter, update, option)

  - `db.multi.updateMany({midterm : {$exists: true}}, {$set: {class : 'graduated'}})` -> 중간고사 점수가 잇는 모든 학생들의 class를 graduated로 변경

- db.collection명.update(filter, update, option)

  - `db.multi.update({final : {$exists: true}}, {$set: {class : 'job'}})` -> update는 지양. 되긴 하지만

- 함수 만들어서 실행하기 :

  - ```javascript
    function updateKor(){
    	var tmp = db.multi.updateMany({kor: {$lte: 60}}, {$set: {kor: 0}})
    	return tmp
    }
    
    updateKor()
    ```

  - updateKor실행하면 update

- 위 까지는 set이라는 update operator사용
- 이외에도: 

- push 

  - 기존데이터 + 하나의 데이터만 추가 할 때
    - `db.myfriends.updateOne({name: '슈퍼맨'},{$push:{buddy: '캡틴아메리카'}})` -> 이름이 슈퍼맨인 doc의 buddy field에 캡틴아메리카 끝에 추가
  - 기존데이터 + 여러 데이터 추가할 때  :  **each**사용!
    - `db.myfriends.updateMany({name: '슈퍼맨'}, {$push: {buddy: {$each : '캡틴아메리카', '블랙위도우'}})` -> 이름이 슈퍼맨인 doc의 buddy field에 캡틴아메리카와 블랙위도우 끝에 추가

- pop : 기존 데이터 삭제할 때

  - 첫번째 데이터 삭제 : -1

    - `db.myfriends.updateOne({name: '슈퍼맨'}, {$pop : {buddy: -1}})` ->  이름이 슈퍼맨인 doc의 buddy의 첫번째 데이터 삭제

  - 마지막 데이터 삭제 : 1

    - `db.myfriends.updateOne({name: '슈퍼맨'}, {$pop : {buddy: 1}})`

    


- db.collection명.replace

  - document **전체** 수정


  - `db.multi.replaceOne({final: {$exists: true}}, {class: 'job'})` -> final이 존재하는 document의 class가 job으로 변경되고 나머지는 다 삭제됨(아이디는 제외)




### 데이터 삭제

- db.collection명.delete()
  - `db.multi.deleteOne({name : '홍길동'})` -> 이름이 홍길동인 도큐먼트 1개 삭제하자


- db.collection명.deleteMany()
  - `db.multi.deleteMany({class : {$exists : true}})` -> 클래스가 존재하는 모든 doc은 삭제하자


