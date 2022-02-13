# Wordcloud

- `from wordcloud import WordCloud`

- `cloud = WordCloud(font_path=,background_color=,max_words,width=,height=)`
  - `font_path` => 원하는 폰트 경로 지정. 만약 같은 위치라면 '폰트파일이름' 이렇게만 작성!!



```python
from wordcloud import WordCloud
import json

cloud = WordCloud(font_path='Goyang.ttf',background_color='white', max_words=30, width=400, height=300)

# r 안쓰면 디폴트로 r
with open('webtoons.json', encoding='utf-8') as f:
    webtoons = json.load(f)

res = dict()
for webtoon in webtoons['webtoons']:
    res[webtoon['title']] = int(float(webtoon['star'])*100)

visual = cloud.fit_words(res)
visual.to_image()

visual.to_file('cloud01.png')
```

- 가져온 json파일에서 각 웹툰 이름을 키로, 웹툰 평점을 값으로 가진 res라는 딕셔너리 생성
- `변수이름 = wordcloud객체.fit_words(딕셔너리)` 해당 딕셔너리의 값을 frequency로 wordcloud객체 생성
- `변수이름.to_image()`이미지화 시작 
- `변수이름.to_file`(파일명) 원하는 파일 명 지정



### 지정한 이미지의 형태를 가진 워드클라우드 생성

- 간단히 mask 속성만 지정하면 됌!

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from PIL import Image
import numpy as np
import json

with open('webtoons.json', encoding='utf-8') as file:
    webtoons = json.load(file)

res = dict()
for webtoon in webtoons['webtoons']:
    res[webtoon['title']] = int(float(webtoon['star'])*100)

# 이미지를 numpy (3차원) 배열로 변환
masking_img = np.array(Image.open('kakao.png'))

# 마스크 속성 설정한 wordcloud생성
cloud = WordCloud(font_path='Goyang.ttf', max_font_size=40, mask=masking_img, background_color='white').fit_words(res)

cloud.to_file('cloud02.png')

```

- 원하는 이미지를 3차월 배열로 변환한후 워드클라우드 생성 때 mask 속성을 이 배열로 지정해준다.



```python
# pyplot 창에 image 띄우기
plt.imshow(cloud, interpolation='bilinear')
plt.axis('off')
plt.show()
```

- `plt.imshow(이미지)` 원하는 이미지 pyplot창에 띄우기
  - interpolation 속성은 이미지를 plt창에 띄울 때 어떻게 처리할 것인지 결정.
  - interpolation에는 None, nearest등등 많음

- `plt.axis('off')` axis 제거하기!

