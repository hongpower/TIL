# Crawling

## Json

#### Starbucks 매장정보 크롤링 하기:

- 스타벅스 공식홈페이지에서 STORE-매장찾기-지역찾기
- 지역검색의 원하는 시도 -> 구/군을 선택하면 선택한 지역 내의 매장 리스트가 출력되는걸 확인 가능. (url바뀜 없이 새로운 창이 열리니 ajax 사용한 것을 파악 가능)
- 개발자 도구를 열어서 내가 특정 버튼을 눌렀을 때 자바스크립트의 어떤 함수가 실행되는지 확인. (크롬말고 파이어폭스에서는 쉽게 확인 가능. 크롬에서는 해당 버튼만 확인 가능 ㅜㅜ)
- 함수를 확인해보면:

```java
__ajaxCall("/store/getSidoList.do", {}, true, "json", "post",function(){})
```

- 아마도 스타벅스 내에서 ajaxCall이라는 함수를 만들었을 텐데, 
  - 첫번째 인자는 url주소
  - 두번째는 해당 url로 보낼 데이터
  - 세 번째는 동기/비동기 여부
  - 네 번째는 파일 형태
  - 다섯 번째는 전송 방식
  - 여섯 번째는 성공적으로 전송했을 때 실행할 함수
- 이런식으로 대충 예측만 한 상태로 파이참 실행



##### 원하는 지역의 매장정보 가져오기:

1) 사용자가 원하는 시/도의 코드와 이름 가져오기

```python
# -*- coding:utf-8 -*-
import json
import requests

def getSido():
    url = 'https://www.starbucks.co.kr/store/getSidoList.do'
    resp = requests.post(url)
    sido_list = resp.json()['list']

    # 시도 코드
    sido_code = list(map(lambda x: x['sido_cd'],sido_list))

    # 시도 이름
    sido_nm = list(map(lambda x: x['sido_nm'],sido_list))

    # 시도 코드와 시도 이름을 묶기 ; 시도 코드가 키가 된다.
    sido_dict = dict(zip(sido_code, sido_nm))

    return sido_dict

if __name__ == '__main__':
    print(getSiDo())
    sido = input('도시 코드를 입력 : ')
```

- url 변수는 endpoint(https://www.starbucks.co.kr) + 개발자도구에 적혀있던 추가 주소
- post방식으로 해당 url의 데이터를 돌려받았고,
- `print(resp.text)` 해보았을 때, json 형태인 것을 파악 가능하므로 `json()` 으로 해당 데이터를 json 객체로 변환! 
  - requests 객체는 `json()` 이라는 메소드가 내장되어있어서 간편!
- `resp.json()`의 결과 값을 보면 {'list':[{},{},{}...]} 이렇게 되어있는 것이 확인 가능. 그러면 list라는 키의 값을 불러와야하니 끝에 ['list'] 추가해서 값만 가져오게끔 하자
- 데이터를 확인하면 'sido_code'라는 변수 이름으로 시도의 코드, 'sido_nm'으로 시도 이름이 저장되어있는 것을 알 수 있음. 
- zip 함수로 코드리스트와 이름리스트의 같은 인덱스끼리 묶어서 dict형태로 만들어주고 반환



이런식으로 가져와졌고 :

```python
{'01': '서울', '08': '경기', '02': '광주', '03': '대구', '04': '대전', '05': '부산', '06': '울산', '07': '인천', '09': '강원', '10': '경남', '11': '경북', '12': '전남', '13': '전북', '14': '충남', '15': '충북', '16': '제주', '17': '세종'}
도시 코드를 입력 : 
```

사용자는 이 데이터를 확인하고 원하는 __도시 코드__만 입력!



2. 사용자가 선택한 시도의 구/군 가져오기

- 사용자가 도시 코드를 입력하면 해당 도시 코드의 구/군을 추출해야한다

```javascript
__ajaxCall("/store/getGugunList.do", {"sido_cd":sido}, true, "json", "post",
                							function (){}) 
```

=> 아까 전이랑 다른 점이, sido라는 데이터를 가지고 서버에 전송하고 있음. 



- 그러면 우리는 사용자가 입력한 도시 코드를 서버에 함께 전송해서 알맞는 데이터를 받아야함

```python
# -*- coding:utf-8 -*-
import json
import requests

def getSiDo():
    sido_code = list()
    sido_nm = list()

    url = 'https://www.starbucks.co.kr/store/getSidoList.do'
    resp = requests.post(url)

    sido_list = resp.json()['list']

    sido_code = list(map(lambda x : x['sido_cd'], sido_list))
    sido_nm = list(map(lambda x: x['sido_nm'], sido_list))
    sido_dict = dict(zip(sido_code, sido_nm))
    return sido_dict

def getGuGun(sido_code):
    url = 'https://www.starbucks.co.kr/store/getGugunList.do'
    
    # 전송 데이터를 이렇게 parameter내에 숨겨서 전송한다고 생각하면 됌.
    resp = requests.post(url,data={'sido_cd':sido_code})
  
    gugun_list = resp.json()['list']
    gugun_dict = dict(zip(list(map(lambda x: x['gugun_cd'], gugun_list)),
                          list(map(lambda x: x['gugun_nm'], gugun_list))))
    return gugun_dict

if __name__ == '__main__':
    print(getSiDo())
    sido = input('도시 코드를 입력 : ')
    if sido == '17':
        print(getStore(sido_code='17',gugun_code=''))
    else:
        print(getGuGun(sido))
        gugun = input('구군 코드 입력 : ')
```

- __get 방식일 때는 그냥 url 끝에 붙이면 되지만, post 방식일 때는 끝에 보낼 데이터를 추가해야함__
- 사용자가 선택한 도시 코드가 17, 즉 세종시를 선택하면, 세종시는 구군이 없기 때문에 이 절차를 건너뛰어야하기 때문에 if와 else로 구분을 해줘야함



3. 사용자가 선택한 구군내의 모든 스타벅스 매장을 json파일로 새로 저장

```python
# -*- coding:utf-8 -*-

import requests
import json

def getSiDo():
    sido_code = list()
    sido_nm = list()
    
    url = 'https://www.starbucks.co.kr/store/getSidoList.do'
    resp = requests.post(url)

    sido_list = resp.json()['list']

    sido_code = list(map(lambda x : x['sido_cd'], sido_list))
    sido_nm = list(map(lambda x: x['sido_nm'], sido_list))
    sido_dict = dict(zip(sido_code, sido_nm))
    return sido_dict

def getGuGun(sido_code):

    url = 'https://www.starbucks.co.kr/store/getGugunList.do'
    resp = requests.post(url,data={'sido_cd':sido_code})
    gugun_list = resp.json()['list']
    gugun_dict = dict(zip(list(map(lambda x: x['gugun_cd'], gugun_list)),
                          list(map(lambda x: x['gugun_nm'], gugun_list))))

    return gugun_dict


def getStore(sido_code='', gugun_code=''): # 디폴트 값 지정.
    url = 'https://www.starbucks.co.kr/store/getStore.do'
    resp = requests.post(url, data={'ins_lat': '37.5652352',
                                    'ins_lng': '127.0284288',
                                    'p_sido_cd': sido_code,
                                    'p_gugun_cd': gugun_code,
                                    'in_biz_cd': '',
                                    'set_date': ''})
    store_list = resp.json()['list']
    result_list = []
    for store in store_list:
        temp = dict()
        temp['s_name'] = store['s_name']
        temp['tel'] = store['tel']
        temp['doro_address'] = store['doro_address']
        temp['lat'] = store['lat']
        temp['lot'] = store['lot']
        result_list.append(temp)
    json_store = {}
    json_store['store_list'] = result_list

    result_json = json.dumps(json_store, ensure_ascii=False)
    with open('starbucks01.json','w',encoding='utf-8') as f:
        f.write(result_json)

    return result_json

if __name__ == '__main__':
    print(getSiDo())
    sido = input('도시 코드를 입력 : ')
    if sido == '17':
        print(getStore(sido_code='17',gugun_code=''))
    else:
        print(getGuGun(sido))
        gugun = input('구군 코드 입력 : ')
        print(getStore(gugun_code=gugun))
```

- 원하는 구군의 매장정보를 얻을 때는 서버에 전송시키는 데이터가 훨씬 많게 지정되어있음.

- 확인방법은 :

  - 같이 보내야 하는 데이터들은 해당 버튼을 눌렀을 때 관리자 도구의의 Network-해당 url에 어떤 데이터를 보냈는지를 확인가능

  - 만약 전송이 필수적이지 않은 데이터는 제외하고 싶다면, 일일이 하나씩 빼가면서 응답이 제대로 되는지 확인해야함 ㅠㅠ
  - 이 확인 과정을 거친 결과, 필수적으로 필요한 데이터는 'ins_lat', 'ins_lng', 'p_sido_cd', 'p_gugun_cd', 'in_biz_cd', 'set_date' 인 것으로 판명났음

- 나머지는 다 비슷하게 진행됌! 
- 우리는 매장이름, 핸드폰번호, 주소 등을 원하기 때문에 원하는 변수들만 가져온 것임
- result_json은 json 구조를 띄고 있으므로 이거를 string으로 변환하는게 json.dumps()이다!



##### 전국 매장정보 가져오기:

```python
# -*- coding:utf-8 -*-

import requests
import json

def getSiDo():
    sido_code = list()
    sido_nm = list()

    url = 'https://www.starbucks.co.kr/store/getSidoList.do'
    resp = requests.post(url)

    sido_list = resp.json()['list']

    sido_code = list(map(lambda x : x['sido_cd'], sido_list))
    sido_nm = list(map(lambda x: x['sido_nm'], sido_list))
    sido_dict = dict(zip(sido_code, sido_nm))
    return sido_dict

def getGuGun(sido_code):

    url = 'https://www.starbucks.co.kr/store/getGugunList.do'
    resp = requests.post(url,data={'sido_cd':sido_code})
    gugun_list = resp.json()['list']
    gugun_dict = dict(zip(list(map(lambda x: x['gugun_cd'], gugun_list)),
                          list(map(lambda x: x['gugun_nm'], gugun_list))))
    return gugun_dict

# ---------------------------- 여기까지는 위와 같음 -----------------------------------------

def getStore(sido_code='', gugun_code=''):
    url = 'https://www.starbucks.co.kr/store/getStore.do'
    resp = requests.post(url, data={'ins_lat': '37.5652352',
                                    'ins_lng': '127.0284288',
                                    'p_sido_cd': sido_code,
                                    'p_gugun_cd': gugun_code,
                                    'in_biz_cd': '',
                                    'set_date': ''})
    store_list = resp.json()['list']
    result_list = []
    
    ## 여기서 파일 생성하지않고 메인에서 생성할 것임
    for store in store_list:
        temp = dict()
        temp['s_name'] = store['s_name']
        temp['tel'] = store['tel']
        temp['doro_address'] = store['doro_address']
        temp['lat'] = store['lat']
        temp['lot'] = store['lot']
        result_list.append(temp)
 
    return result_list


if __name__ == '__main__':
    list_all = list()
    #sido_all은 딕셔너리 형태
    sido_all = getSiDo()
	# 만약 딕셔너리 형태를 loop를 돌리면 key만 loop돌음
    for sido in sido_all:
        if sido == '17':
            result = getStore(sido_code=sido)
            print(result)
            list_all.extend(result)
        else:
            gugun_all = getGuGun(sido)
            for gugun in gugun_all:
                result = getStore(gugun_code=gugun)
                list_all.extend(result)

    result_dict = dict()
    result_dict['list']=list_all

    result = json.dumps(result_dict, ensure_ascii=False)
    with open('starbucks_all.json','w',encoding='utf-8') as f:
        f.write(result)
```

- 하나의 매장 정보가 담긴 하나의 리스트가 생성되고, list와 list를 extend하면 기존의 list에 합쳐지기 때문에, 결과는 하나의 리스트가 'list'라는 키의 값이 되는 것임.



