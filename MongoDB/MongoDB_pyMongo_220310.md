# MongoDB

## pyMongo

- python으로도 MongoDB를 다룰 수 있게하는 모듈

- workspace_crawling에서 작업 (가상환경도 mycrawling)



- pyMongo 설치 :
  - `pip install pymongo`



### Connecting pyMongo to MongoDB

```python
from pymongo import MongoClient

## cmd창에서 mongo쳤을 때처럼 client와 연결하기
client = MongoClient('localhost', 27017)
# client = MongoClient('mongodb://localhost:27017') 도 가능. 기본 port는 27017!

## db 접근
# test라는 db접근
db = client.test
# db = client['test']도 가능

## collection 접근
collection = db.score
# collection = db['score']도 가능

## 커서 객체
result = collection.find()
print(result) # -> 커서 객체 출력. 커서 객체는 iterate하면서 출력해야함
for res in result:
    print(res)
```

- find : 커서 객체출력. 여러 데이터가져올 때 사용. 꼭 iterate해줘야 내용 확인 가능

- find_one : 하나의 데이터 가져올 때 사용. **iterate안하고 print해도 바로 내용 출력 가능**



### CRUD in PyMongo

#### 조회

- ```python
  .
  .
  .
  
  score = db.score
  
  data = score.find({'name' : '이순신'})
  for d in data:
  	print(data)
      
  # 파이썬이기 때문에 함수는 () 작성!
  # score collection 내 document 세기:
  print(score.count_documents({})) 
  
  # 국어 점수가 60점보다 큰 모든 doc _id 필드 제외하고 출력
  score_over_60 = score.find({'kor' : {'$gt' : 60}}, {'doc_id' : 0})
  for sc in score_over_60:
      print(sc)
  ```



#### 삽입

- insert_many / insert_one : js와 다르게 snake표기법 사용!
- insert_many & insert_one returns *(inserted_ids, acknowledged)*
  - inserted_ids 결과값만 갖고 싶다면 `return값들어있는변수명.inserted_ids`

- ```python
  .
  .
  .
  ## insert_many : 
  # 값 insert하고 (inserted_ids, acknowledged) return
  result01 = score.insert_many([
       {'name' : '권지용', 'kor': 100, 'eng': 30, 'math': 50},
       {'name' : '홍지수', 'kor': 50, 'eng': 100, 'math' : 70, 'mid_term' : {'kor': 30, 'eng': 90}},
       {'name' : '홍지윤', 'kor': 70, 'eng': 80, 'math' : 90}
  ])
  
  # 각 doc의 id를 print해줌
  print(result01.inserted_ids) 
  
  # insert_one:
  result02 = score.insert_one({'name' : ... })
  ```





#### 갱신

- update_one / update_many

- 갱신은 항상 주의!

- returns *raw_result, acknowledged*

- ```python
  ## update_one :
  # 이름이 홍지수인 doc 찾아서 mid_term의 eng 점수를 100으로 바꾸기
  result01 = score.update_one({'name': '홍지수'}, {'$set': {'mid_term.eng':100 }})
  
  # 해당 조건에 만족하는 doc 수:
  print(result01.matched_count)
  # 해당 조건에 만족해서 변경된 doc 수:
  print(result01.modified_count)
  
  
  ## update_many:
  # 영어 점수가 50점 이하인 doc의 영어 점수를 0점 처리
  result02 = score.update_many({'eng' : {'$lte' : 50}},{'$set'{'eng' : 0}})
  
  # 해당 조건에 만족하는 doc 수:
  print(result01.matched_count)
  # 해당 조건에 만족해서 변경된 doc 수:
  print(result01.modified_count)
  ```



#### 삭제

- delete_one/ delete_many

- 삭제는 항상 주의!

- returns *raw_result, acknowledged*

- ```python
  # midterm점수가 있는 doc은 삭제
  result01 = score.delete_many({'midterm': {'$exists': True}})
  
  # 삭제된 doc 출력
  print(result01.deleted_count)
  ```



### Aggregation

```python
from pymongo import MongoClient
client = MongoClient('localhost', 27017)
db = client.test
score = db.score

# 한국어 점수가 50보다 큰 그룹의 id를 kor라하고 총 합을 가진 sum이라는 field도 만들어서 출력해라:
aggr = score.aggregate(
	[
        {'$match': {'kor' : {'$gt' : 50}}},
        {'$group': {'_id' : 'kor', 'sum' : {'$sum' : 'kor'}}}
    ]
)
print(aggr) # cursor 객체 출력
print(list(aggr)) # `[{'_id': 'kor', 'sum': 515}]`
```

- :heavy_check_mark: **MongoDB에서는 aggregate안에 바로 match와 group 작성했지만 pyMongo에서는 리스트안에 작성!!!**





### Importing JSON file to MongoDB

```python
## starbucks01.json파일 내용 그대로 MongoDB에 올리기
import json
from pymongo import MongoClient

#1. json 파일 한줄씩 읽어와서 변수에 저장
with open('starbucks01.json', 'r', encoding='utf-8') as f:
    sb_json = f.readline()

#2. 해당 변수에 '문자열'로 저장된 json 데이터 -> '딕셔너리'로 변경
sb_dict = json.loads(sb_json)

#3. mongoDB와 연결
client = MongoClient('localhost', 27017)
db = client.test
# 해당 collection이 없다면 자동으로 생성: 
starbucks01 = db.starbucks01

#4. mongoDB에 데이터 저장
res = starbucks01.insert_Many(sb_dict['list']) # json파일은 list라는 키 아래에 값들어있는 구조였음
print(res)

#5. 잘들어갔는지 확인
all = starbucks01.find()
for a in all:
    print(a)
```



### GeoJson

> lat, lon의 정보를 활용할 때는 geojson 객체 모양으로 변경해야함

- geoJson
  - 참고: https://datatracker.ietf.org/doc/html/rfc7946#section-3.1.2  
  - type : geoJson 타입
    - Point : coordinate member가 single position일 때
    - MultiPoint : coordinate member의 position이 array일 때 , 즉 여러개일 때
    - LineString : coordinates member가 2개 또는 더 많은 position의 array일 때 
    - MultiLineString : coordinates member가 Linestring array의 array일 때 (2차원 array)
  - coordinates : **[longitude, latitude]** 처럼 list로 만들기! 순서 중요!!



```python
## starbucks01.json파일 내용 중 필요한 내용만 가져와서 geojson으로 만들기
import json
from pymongo import MongoClient

with open('starbucks01.json', 'r', encoding='utf-8') as f:
    sb_json = f.readline()

sb_dict = json.loads(sb_json)

geo_list = list()
for data in sb_dict['list']:
    geo_dict = dict()
    # 가게 이름이 담긴 key도 추가로 만들어주기 :
    geo_dict['s_name'] = data['s_name']
    # 가게 위치 정보 (geoJson핵심) : 

    coordinates = [float(data['lot']), float(data['lat'])]
    geo_dict['location'] = {'type' : 'Point', 'coordinates': coordinates}
    # 여기서 location 필드의 coordinates는 legacy coordinates pair 라고 부름!
    geo_list.append(geo_dict)

# client와 연결
client = MongoClient('localhost', 27017)
db = client.test
starbucks02 = db.starbucks02

# 데이터 추가
# geo_list는 이미 list안에 dict가 들어있는 형태로 잘 들어있기 때문에 그대로 insert 가능 (우리 데이터 여러개면 list로 삽입했잖아)
res = starbucks02.insert_Many(geo_list)
print(len(res.inserted_ids))
```



#### GeoJson 활용

- cmd 창에서 :
  - `db.starbucks02.find({}, {_id : 0})` : 잘 입력된 것 확인 가능!
  - CreateIndex()
    - `db.collection.createIndex({location field : index type})`
    - index type : 
      - 2dsphere : 2dsphere index는 지구형태의 구 상에서의 geometries를 계산하는 query를 도와주는 index임
      - 2d : 2차원 plane 에서의 geometries를 계산하는 query를 도와주는 index
    - location field : coordinates와 type이 지정되어있는 field
  - `db.starbucks02.createIndex({location : '2dsphere'})`: 인덱스 새로 생성됐다는 결과창 보여줌
  - ![image](https://user-images.githubusercontent.com/96896873/157679495-2ca89cc1-d370-4214-9c37-04f0f9e9a21d.png)



:white_check_mark: 꼭 내 위치 구글맵에서 찾아와서 *경도 위도 순서바꾸기* !!

- 내가 지정한 위치에서부터 가까운 순서대로 출력 :

```javascript
db.starbucks02.find(
	{
        location:{
        	$near: {
        		$geometry:{
                	type: 'Point', coordinates: [127.02882908258077,37.56470408511381]
                }
            }
        }
    }
)
```

=> location field의 $geometry에는 내 중심위치를 자체적으로 넣어주자

- $near : 자체적으로 지정한 geometry에 있는 내 위치에서 location필드에 넣었던 coordinates위치를 가까운 순서대로 출력

- $geometry : GeoJson의 geometry를 specify해주는데, query operator 중 `$geoWithin`, `$geoInteracts`, `$near`, `$nearSphere`를 사용할 때 사용



- 내가 지정한 위치에서부터 가까운 순서대로 출력하되 *반경 제한 주기*:

```javascript
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

- 위처럼 $near에 option도 줄 수 있음
- maxDistance : 내 위치에서 최대 500 km안에만

- 더 많은 옵션 : https://docs.mongodb.com/manual/reference/operator/query/near/#mongodb-query-op.-near



- ''~ 서울'' 단어가 포함된 좌표들을 가까운 순으로 출력:

```javascript
db.starbucks02.aggregate(
    [
        {
            $geoNear: {
                near: {
                    type: 'Point', coordinates: [127.02882908258077,37.56470408511381]},
                spherical: true, 
                query: {s_name: /서울/}, 
                distanceField: 'distance'}
        }
    ]
)
```

- `$geoNear` : output을 가까운순에서 먼순으로 정렬

- $geoNear 옵션들 :

  - `near` : 가까운 위치를 찾을 중심위치
  - `spherical` : 두 지점의 거리를 어떻게 계산할지 정함 (boolean값 입력)
    - true : $nearSphere semantic를 활용해서 계산
    - false : $near semantic를 활용해서 계산 

  - `query` : 해당 쿼리에 만족하는 document만 출력 (limitation거는거)
  - `distanceField` :  계산된 distance가 포함된 field. starbucks02에서는 'distance'라는 필드명에 값 저장해놨지?



- 여러 위치 point 지정해서 그 위치 포인트 내의 위치한 상점만 출력하기 (다각형 version):

```javascript
// 왕십리 : 127.03831233365754, 37.56251765082396
// 신당 : 127.01925733492725, 37.566338856242474
// 신금호 : 127.0209654523433, 37.55526584812756

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

- Polygon, 즉 다각형이기 때문에 위치 3개 말고도 추가로 더 해도 됨
  - 중요한건 **끝맺음 하기** ; 시작했던 위치를 마지막에 한번더 작성!


- 여러 위치 point 지정해서 그 위치 포인트 내의 위치한 상점만 출력하기 (원 version):

```javascript
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

- `$centersphere : [중심위치], radius` : 
  - radius는 radians로 calculate되기 때문에 radians로 단위 변경이 필요함
  - :white_check_mark: radians : 
    - 반지름의 길이가 호의 길이와 같아지는 각을 1 라디안 이라고 함(각도 단위중 하나임)
    - 원하는 distance/radius of the sphere를 해주면 해당 distance의 radian을 구할 수 있음
    - 이 예제에서는 지구를 기준으로 했음. 3963.2는 지구의 반지름
    - 중심위치에서 반지름이 0.5mile인 구의 반경 내에 있는  가게 정보 출력

- https://docs.mongodb.com/manual/tutorial/calculate-distances-using-spherical-geometry-with-2d-geospatial-indexes/ 여기 참고







## 기타 : 

- javascript에서는 field명에 single을 작성안해줘도 됐지만(오히려 쓰면 오류나는 경우도 있었음), pymongo에서는 field명에 무조건 single 작성. 

- typora에서 작성해서 cmd창에 쓰면 tab을 사용한 경우 오류가 잦음 (또 한줄씩 건너띄기가 됨) 그러므로 메모장 사용 or code block 사용!

- 기존 필드를 지정할 때는 `'$필드명'` 으로 작성

- python은 snake기법을 사용하니 이전 mongodb에서 작성하던(js로 작성) camel방식은 모조리 snake로 바꿔서 작성

- loads는 json string -> dict할 때 사용

- load는 file에서 json읽어올 때 사용

- operator : https://docs.mongodb.com/manual/reference/operator/query/#std-label-query-selectors

  