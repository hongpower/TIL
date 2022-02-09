# Django

## Paging

#### 더미 데이터 만들기

- 페이징 하기 전에 장고 셸 활용해서 dummy data를 만들고 시작

  ```sql
  python manage.py shell
  
  >>> from myboard.models import MyBoard
  >>> from django.utils import timezone
  >>> for i in range(1,101):
  		temp = MyBoard(myname=i, mytitle=i, mycontent=i, mydate=timezone.now())
  		temp.save()
  		
  >>> exit()
  ```

  

### Paginator

- Paginator는 장고에서 paging을 할 대 사용하는 클래스

```python
# views.py

from django.core.paginator import Paginator

def index(request):
    myboard = MyBoard.objects.all().order_by('-id')
    
    # Paginator 객체 생성
    paginator = Paginator(myboard, 5)
    
    # 초기화면은 page가 none이기 때문에 1이라는 값, 나머지는 화면(링크)는 page라는 변수에 저장된 int형의 데이터가 page_num 변수에 저장
    page_num = request.GET.get('page','1')

    # 페이지 번호를 가진 객체 생성 (여러 정보들을 담고있기 때문에, page_obj 단독으로만 사용하지는 않을것임)
    page_obj = paginator.get_page(page_num)

    return render(request, 'index.html', {'list': page_obj})
```

`p = Paginator(objects, 2)` 

- 1번째 인자는 a list of objects
- 2번째 인자는 number of items you'd like to have on each item

`page_num = request.GET.get('page','1')`

- `http://localhost:8000/`=> 이게 기본 index인데, 이처럼 위에 page=''이 없이 호출된 경우 디폴트를 1로 설정한 것임 (즉 페이지 1로 설정됨)

:heavy_check_mark: request.GET VS request.GET.get

| request.GET      | request.GET.get         |
| ---------------- | ----------------------- |
| 대상 없을 때오류 | 대상이 없으면 None 리턴 |



`page_obj = paginator.get_page(page_num)` => 지정한 숫자의 페이지에 해당되는 페이징 객체 생성. page_obj는 특정 페이지의 정보만 담은 페이징 객체인 것임.



- page_obj 관련 속성 및 메소드들 :

```python
    print(type(page_obj)) 
    	# <class 'django.core.paginator.Page'>
        
    print(page_obj) 
    	# <Page 1 of 21>
        
    print(page_obj.count) 
    	# bound method Sequence.count of <Page 11 of 21>
        
    print(page_obj.paginator.count)
    	# 전체 게시물 개수 : 21
    print(page_obj.paginator.num_pages) 
    	# 전체 게시물 개수 : 21
        
    print(page_obj.paginator.per_page)
    	# 페이지당 보여줄 게시물 개수
        
    print(page_obj.paginator.page_range) 
    	# 페이지범위 : range(1,22), 그래서 list형태임!!
        
 	print(page_obj.number)
    	# 현재 페이지 번호
        
    print(page_obj.has_next()) 
    	# true/false
        
    print(page_obj.next_page_number)
    	# 다음 페이지 번호
        
    print(page_obj.has_previous()) 
    	#true/false
        
    print(page_obj.previous_page_number)
        # 다음 페이지 번호
        
    print(page_obj.start_index()) 
    	# 해당 페이지 시작 인덱스(1 based index)
    
    print(page_obj.end_index())
        # 해당 페이지 끝 인덱스(1 based index)
```



  ```html
  <!--template-->
  <!--index.html-->
  	<!-- 처음으로 이동 -->
  	<a href="?page=1">처음</a>
  
      <!--이전페이지-->
      {% if list.has_previous %} 
          <a href="?page={{ list.previous_page_number }}">이전</a> => 만약 previous page가 있다면 해당 page_number로 이동하는 이전 anchor tag 생성
      {% else %}
          <a>이전</a> => previous page가 없다면 이동경로가 없는 '이전'이라는 비활성화 링크
      {% endif %}
  
      <!--페이지 리스트-->
  
      {% for page_num in list.paginator.page_range %} => 페이지를 루프 돌며 해당 페이지로 이동할 수 있는 링크 생성
          {% if page_num == list.number %} => 루프 돈 요소가 지금 현재 페이지 번호와 같다면
              <b>{{ page_num }}</b> => 볼드 처리해라
          {% else %} => 루프 돈 요소가 지금 현재 페이지 번호와 다르다면
              <a href="?page={{ page_num }}">{{ page_num }}</a> - => 다른 페이지로 이동할 수 있는 링크 생성해라
          {% endif %}
      {% endfor %}
  
      <!--다음 페이지-->
      {% if list.has_next %}
          <a href="?page={{ list.next_page_number }}">다음</a>
      {% else %}
          <a>다음</a>
      {% endif %}
  
      <!--끝으로-->
      <a href="?page={{ list.paginator.num_pages }}">끝</a>
  
  
      <br>
      <br>
      <!--회원가입-->
      <a href="/register">회원가입</a>
      <br>
  
      <!--로그인-->
      {% if not request.session.myname %}
      <a href="login/">로그인</a>
      {% else %}
      <a href="logout/">로그아웃</a>
      {% endif %}
  ```

- 현재 index함수에서 전달된 데이터는 list라는 key의 값인 page_obj 였다!

- 이전 페이지와 다음페이지는 

  - 이전 페이지나 다음 페이지를 확인할 수 있는 속성 `has_next` & `has_previous` 를 활용해서 여부 조사
  - 있다면 해당 페이지로 이동할 수 있는'이전', '다음' 링크를 생성. 그냥 비활성화 된 '이전', '다음' 링크 생성

- 페이지 리스트는
  - 첫번째로 현재 range(1-21)로 구성되어있는 리스트의 요소를 하나씩 꺼내가면서 if 조건문 확인 (총 21번 돈다)
  - 만약 내가 현재 있는 페이지 숫자와 루프돌고 있는 리스트의 요소와 같다면
    - 볼드 처리된 텍스트만 있는 숫자를 출력
  - 만약 다르다면
    - 해당 리스트 요소의 페이지로 이동할 수 있는 링크 생성

  