# MongoDB

## Aggregation

- 집계함수와 연관

- nosql은 규모가 큰 데이터를 분산저장해놓기 때문에 형태가 다양 -> 가지고 오는 데도 시간이 오래걸리고 원하는거 가지고 나와서 뭐 처리하는 것도 오래 걸리고 한 작업을 수행하는데 시간이 오래 소요되기 때문에 집계함수를 잘 안씀

- 그래도 배워보자!

  

- `db.collection.arregate()`

  - `match` -> where/having
  - `group의 첫번째 절`->  groupby
  - `group의 두번째 절` -> 집계함수 작성



- ```javascript
  db.multi.aggregate(
      {$match: {kor: {$gt:70}}}, // kor가 70점보다 큰 doc을 찾아서
      {$project: {kor: 1}}, // kor field만 뽑아낸 후
      {$group: {_id: 'test', 'average':{$avg: '$kor'}}} // _id 필드의 값이 test이고 kor의 평균을 계산한 average라는 field를 가진 doc을 생성해라
  ) 
  ```

-> kor이 70보다 큰 document를 가져와서 kor 컬럼만 가져오고 test라는 id로 묶어서 평균을 낸 값.

- $group:
  - 만약 _id를 `$필드명` 하면 해당 필드명 별로 group이 되고 _id이름은 해당 필드명이 되는 것임.
    - 예) '`{$group: {_id: '$class', 'average': {'$avg' : '$kor'}}}`

- ```javascript
  db.multi.aggregate(
  	{$match: {name: /s/}} // name에 s가 들어간 doc을 찾아서
  	{$group: {_id : "test", sumup : {$sum : '$kor'}}} // 모든 doc의 kor점수를 더해서 sumup이라는 필드명으로 만들어라
  )
  ```

- `db.score.aggregate()` -> stage지정안해주면 find 결과랑 똑같이 나옴

- `db.score.aggregate({$project: {_id: 0, name: 1, kor: 1, eng: 1, math: 1}})` -> 점수만 나옴
- `db.score.aggregate({$match: {kor: {$gt: 80}}})` -> where절과 동일

- `db.score.aggregate({$group: {_id : '$test', average: {$avg: '$kor'}}})` -> 모든 doc에 적용됌



## Mapreduce

- 복잡한 집계 작업에 사용(aggregation에서 해결못하는)
  - 5.0버전부터는 deprecated되고 aggregate에서 사용 권장됨

- query -> map -> reduce -> out

  - map은 데이터를 가져와서 내가 원하는 것끼리 묶기
  - reduce : 집계 연산 실행
- test가 final인 doc들의 이름 국어 영어 출력하고 '국어와 영어의 합'도 같이 출력 (aggregation가지고는 안됨)

```javascript
function myMap(){
	//emit(key, val)
	emit(this.score, {name: this.name, kor: this.kor, eng: this.eng, test: this.test})
} // this는 myMap()을 실행해주는 collection의 모든 document를 칭함
// => score라는 key로 value가 각 doc의 {name, kor, eng, test}가 들어있는 자료가 쌓일 예정

// map함수를 통해서 수집된 자료를 하나씩 순차 입력 :
function myReduce(key, values){
	var result = {name: new Array(), kor: new Array(), eng: new Array(), total : new Array()}
    // 배열로 value로 전달된 값을 담음
    values.forEach(function(val){
        // map함수에서 넘어온 field중에 test도 있었지?
    	if (val.test == 'final'){
    		result.name += val.name + " ";
    		result.kor += val.kor + " ";
    		result.eng += val.eng + " ";
    		result.total += val.kor + val.eng + " ";
		}
	});
return result;
}

// myRes라는 collection으로 결과 추출 : 
// mapReduce(map함수, reduce함수, out : {action: 'collectionName'})
db.score.mapReduce(myMap, myReduce, {out: {replace: 'myRes'}}) 

// myRes 확인
db.myRes.find();
```

- out의 parameter 중 하나인 action:
  - replace : 만약 지정한 collection이름의 collection이 있다면 replace the contents해라
  - merge : 만약 지정한 collection이름의 collection이 있다면 merge해라. 동일한 key이름이라면 새로운 컬렉션은 기존꺼 위에 overwrite해라
  - reduce : merge와 동일. but merge와 다르게 동일한 key이름이면 reduce function을 기존 collection과 new collection에 적용하고 기존 collection이 overwrite을 함



