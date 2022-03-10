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


- 



aggregation

- 집계함수와 연관
- nosql은 규모가 큰 데이터를 분산저장해놓기 때문에 형태가 다양하기 때문에 가지고 오는 데도 시간이 오래걸리고 원하는거 가지고 나와서 뭐 처리하는 것도 오래 걸려서 집계함수를 잘 안씀
- db.collection.arregate()
  - match -> where/having
  - group의 첫번째 절->  groupby
  - group의 두번째 절 -> 집계함수 작성



- multi collection 다시 생성

db.multi.insertMany([{name: 'hong-gd', kor:100, eng:30, math: 60}, {name: 'kim-sd', kor:40, eng:70, math:100}, {name: 'park-jy', kor: 100, eng: 100, eng: 100, math:100}, {name: 'huh-jy', kor: 100, eng: 100, math: 100}, {name: 'lee-ss', kor: 60, eng: 100, math: 70}])

db.multi.aggregate(
{$match: {kor: {$gt:70}}},
{$project: {kor: 1}},
{$group: {_id: 'test', 'average':{$avg: '$kor'}}}
) -> kor이 70보다 큰 document를 가져와서 kor 컬럼만 가져오고 test라는 id로 묶어서 평균을 낸 값.

- 만약 _id를 $컬럼이름 하면 해당 필드이름 별로 group이 되고 _id이름은 해당 컬럼이름이 되는 것임.
  - 예) '$class' ..

$project : 원하는 값만 가져오기

db.multi.aggregate(

{$match: {name: /s/}},

{$group: {_id: 'test', sum : {$sum: '$kor'}}}

) -> 새로운 변수명 만들 때 ''안에 꼭 쓸 필요 X



db.score.insertMany([
   {name:"홍길동",kor:90,eng:80,math:98,test:"midterm"},
   {name:"이순신",kor:100,eng:100,math:76,test:"final"},
   {name:"김선달",kor:80,eng:55,math:67,test :"midterm"},
   {name:"강호동",kor:70,eng:69,math:89,test:"midterm"},   
   {name:"유재석",kor:60,eng:80,math:78,test:"final"},
   {name:"신동엽",kor:100,eng:69,math:89,test:"midterm"},
   {name:"조세호",kor:75,eng:100,math:100,test:"final"}
])

db.score.aggregate() -> stage지정안해주면 find 결과랑 똑같이 나옴

db.score.aggregate(

{$project: {_id: 0, name: 1, kor: 1, eng: 1, math: 1}}

) -> 점수만 나옴

db.score.aggregate(

{$match: {kor: {$gt: 80}}}

)

db.score.aggregate(

{$group: {_id : '$test', average: {$avg: '$kor'}}}

)



mapreduce

-> 복잡한 집계 작업에 사용(aggregation에서 해결못하는)

- query -> map -> reduce -> out

  - map은 데이터를 가져와서 내가 원하는 것끼리 묶기

  - reduce : 집계 연산 실행

- test가 final인 doc들의 이름 국어 영어 출력하고 '국어와 영어의 합'도 같이 출력 (aggregation가지고는 안됨)

function myMap(){

emit(this.score, {name: this.name, kor: this.kor, eng: this.eng, test: this.test});

} -> my map을 실행해주는 collection이 this.

emit(key, value)



function myReduce(key, values){

var result = {name: new Array(), kor: new Array(), eng: new Array(), total : new Array()}

values.forEach(function(val){

if (val.test == 'final'){

result.name += val.name + " ";

result.kor += val.kor + " ";

result.eng += val.eng + " ";

result.total += val.kor + val.eng + " ";

}

});

return result;

}

db.score.mapReduce(myMap, myReduce, {out: {replace: 'myRes'}}) -> myRes라는 collection으로 결과가 나옴

db.myRes.find();



파이참 열어서 workspace_crawling

python_mongodb 폴더 내에 hello.py만들기

pip install pymongo로 다운받기



geojson objects -> lat, lon의 정보를 활용할 때는 geojson객체 모양으로 바꿔야함

- type : geoJSOn 타입을 정해주고
- coordinates : long 먼저 lat그 다음으로 지정  리스트



starbucks04까지 완료한 후에 

cmd 창에서 : 

db.starbucks02.createIndex({location : '2dsphere'})

![image-20220310162126766](C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220310162126766.png)



- 2dsphere 뜻 알아오기래... ㅋ 중요하대

- 내 위치 위치 찾아오기 : 37.56470408511381, 127.02882908258077

- 하단 넣을 때 위도 경도 위치 주의!!!!

```
db.starbucks02.find(
{
location:{
$near: {
$geometry:{
type: 'Point', coordinates: [127.02882908258077,37.56470408511381 ]
}
}
}
}
)
```

=> 가까운위치보다 찾아서 출력해주고 있음



```
db.starbucks02.find(
{
location:{
$near: {
$geometry:{
type: 'Point', coordinates: [127.02882908258077,37.56470408511381 ]
},
$maxDistance: 500
}
}
}
)
```

- maxDistance : 내 위치에서 최대 500 km안에만



현재 위치부터 ~ 서울 단어가 포함된 좌표들의 거리

db.starbucks02.aggregate([{$geoNear: {near: {type: 'Point', coordinates: [127.02882908258077,37.56470408511381 ]}, spherical: true, query: {s_name: /서울/}, distanceField: 'distance'}}])



왕십리 : 127.03831233365754, 37.56251765082396

신당 : 127.01925733492725, 37.566338856242474

신금호 : 127.0209654523433, 37.55526584812756

```
db.starbucks02.find(
	{
        location:{
            $geoIntersects: {
                $geometry: {
                type: 'Polygon',
                coordinates: [
                        [
                        [127.03831233365754, 37.56251765082396],
                        [127.01925733492725, 37.566338856242474],
                        [127.0209654523433, 37.55526584812756],
                        [127.03831233365754, 37.56251765082396]
                        ]
                	]
                }
            }
        }
    }
)
```

- 삼각형만들기
  - 꼭 4개 작성! 왜냐하면 끝맺음해야하니까
  - 몇각형이든 상관 ㄴㄴ



- 원

```
db.starbucks02.find(
    {
        location: {
            $geoWithin: {
                $centerSphere: [[127.03831233365754, 37.56251765082396], 0.5/3963.2]
            }
        }
    }
)
```

- centersphere: 중심위치, radius
- https://docs.mongodb.com/manual/tutorial/calculate-distances-using-spherical-geometry-with-2d-geospatial-indexes/ 여기 참고



ubuntu사이트 들어가서 20.04



강사님은 vmware하는데 wsl이 설치 쉽고 삭제 쉽지만 windows 특정 버전이상은 vmware됨.

1. wsl(windows sub머시기)

2. vmware/virtual box



zetbrain ultimate계정 대학교 계정으로 만들어서 DatagGrip이라는 툴 사용하면 nosql도 편하게 사용 가능하대

모듈/라이브러리 = 함수들을 모아서 배포

operator : https://docs.mongodb.com/manual/reference/operator/query/#std-label-query-selectors



기타 : 

- javascript에서는 field명에 single을 작성안해줘도 됐지만(오히려 쓰면 오류나는 경우도 있었음), pymongo에서는 field명에 무조건 single 작성. pymongo와 js차이 알아볼 것f 
