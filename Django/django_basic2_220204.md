# Django

- 프레임워크 : 프로그램을 만들기 위해서 반쯤 만들어놓고 필요한 부분만 코드를 작성하면 완성시켜주는 코드 더미



### 코드 실행:

터미널 창에:

- cd 프로젝트명 예) `cd hello` 
- 결과값 : (myweb) C:\workspaces\workspace_django\프로젝트명>
  - `(myweb) C:\workspaces\workspace_django\hello>` => 이런식으로 뜸
- 항상 프로젝트 내에서 runserver 실행!



- python ...py => python 파일을 실행할 것이다라는 뜻

  - `python manage.py runserver`
  - python manage.py runserver => 이거는 꼭 원하는 프로젝트 내에서 실행 (tags든, mysite든)
  
  

##### 코드 닫기:

- 만약 django계속 사용하고 한 프로젝트만 닫으려면, `ctrl + c` 누르기 
- 만약 새로운 프로젝트 하고 싶다면, 위에꺼 진행한다음에 `cd`로 이동 후 `startproject`하면 된다



## 루트

`/` : 루트부터 찾기

`./` : 현재위치에서 찾기

`../` : 상위폴더에서 찾기



- 내가 현재 루트위치에 있다면 '/' or './'는 같은 위치이다.
- 만약 루트가 `hello01` 이런식으로 역슬래쉬 없이 작성되어있다면 `./hello01` 이 뜻이다. 즉 내 위치에서 찾는다



### settings.py

###### BASE_DIR

- 기본 루트 dir 설정하는 곳
- `BASE_DIR = Path(__file__).resolve().parent.parent` 
- 여기서 Path(\__file__)은 내가 있는 파일, 즉 settings.py이다
- 내 파일의 부모의 부모라는 뜻.  현재 (내가 하고 있는 파일의 부모의 부모는 프로젝트로 되어있음)



##### TEMPLATES

- TEMPLATES=[] 이걸 Settings에서 작성함... 뒤에나오는거처럼!!!!!!!!

- `'DIRS': [BASE_DIR/'templates']` : 이거 작성하면 내가 작성하고 있는 settings.py의 프로젝트(BASE_DIR이 프로젝트로 지정되어있기 때문에) 밑에 templates를 'DIRS'로 설정한 것임. 즉 templates와 관련된 

- 이거 작성 후에는 프로젝트 밑에 templates라는 폴더 생성

  



### Template

> Template는 텍스트 파일로, HTML, XML, CSV와 같은 텍스트 형태의 파일은 다 template으로 만들 수 있다.

- HTML코드를 작성해서 사용자에게 present하는 곳이다
- templates라는 폴더 하위에 html파일, css파일 등을 만들어서 자유롭게 작성한다



####  Variables

- {{}}는 django에서 변수를 작성하는 방법이다.

- 예) `<h1>Hello, {{ name }}</h1>` 

  - name이라는 변수에 있는 실제 값을 화면에 표현한다

- ```django
  <h1>{{ lst.0 }}</h1>
  <p>{{ lst.1 }}</p>
  ```

- VIEWS에서 render함수를 통해 lst라는 변수를 template에 전달하고 있는 상태라서 이렇게 작성 가능!

- 왜 lst의 인덱스를 '.'으로 표현하고 이런 거 등 자세한건 django 템플릿 언어 참고 => https://docs.djangoproject.com/en/4.0/ref/templates/language/



#### Tags

- {% tag %} => tag대신 명령문 작성

- 몇몇 태그는 ending tags({% endtag %})도 있음 

- for, if/elif/else 등

- for 구문 작성후 꼭 endfor 작성하기! 

- django의 template에서는  range 태그나, function이 없음!=> 그래서 views에서 생성. template이 아니라

  - ```django
    {% for i in number %}
    <p>{{ i }}</p>
    {% endfor %}
    ```


- https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#ref-templates-builtins-tags 참고


##### if 

  - if구문에서 변수값만 넣어놓으면 있으면 true,
  - if( a ==  "지수" )
  - elif( b == "병규")

- else()



##### url

- url 태그 뒤에 url name (별칭) 작성 가능.
- 별칭 주어서 작성하는 법이 방법 중 일부인 것임. url 태그 뿐만 아니라 a태그의 href=""에도 별칭으로 주는 것 가능

  ```django
  <!--urls.py-->
  path('', views.index, name='index') <!--=> 이거는 path의 첫 인자인 경로에 대한 별칭, 이름이라 생각하면 됨-->
  
  <!--template-->
  <a href="{% url 'index' %}">go index</a>
  ```

  -> name이 index인 애를 찾아라는 뜻





### views.py

- render함수

  - render는 최소 2가지의 argument를 가져오는데,

  - 첫번째는 request고 두번 째는 내가 띄우고 싶은 html파일이다.

  - 세번 째 인자(context)로 원하는 값을, 예를 들어 view에서 사용하던 파이썬 변수를 html 템플릿으로 넘길 수 있다. context는 딕셔너리형태로 작성한다. key는 템플릿에서 사용할 변수 이름, value값이 파이썬 변수가 된다

  - __중요한 점은, context작성시 키는 무조건 "" 씌워서 문자열로 전달하자!__

  - `{'name': 'jisu'}` 이런식으로 작성

    

- 함수 작성할 때 return render(request, "index.html", {}) =>이렇게 작성했을 때 index.html을 templates파일 하위에서 찾는데, 초반에 Settings.py에 'DIRS'의  root 경로를 이렇게 설정해놔서, 그냥 index.html쓰면 templates파일 내에서 찾게 됨

  

### urls.py

- `from . import views` => 같은 디렉토리 내 views라는 파일 가져오기

- path('',x) => 여기에 ''이거는 trailing slash가 포함된 것임

- 그 path의 경로에는 무조건 끝에 trailing slash 붙이자!

  

#### SSR

- 서버쪽에서 html문서를 만들고 이제 client로 넘김
- spring, django 등
- django는 views.py라는 파일 내 render()함수가 그려주니까 SSR



#### CSR (client side rendering)

- 서버에서 데이터를 응답해줄 것임.
- client파트에서 서버로부터 자료 받아서 응답 (서버입장에서는 이게 편하지) 즉 client파트에서 처리를 하는 것임.
- reactjs, vue.js, angular가 이런거하지



#### CSRF

- Cross site request forgery protection
- 익명의 어떤 접속자(공격자)가 클라이언트가 의도하지 않은 행위를 웹사이트에 요청하는 것을 CSRF라고 함. 즉 이 접속자는 권한을 직접 뺏을 수는 없지만 유저가 어떤 행위를 하게끔 만드는 것임.
- 서버 입장에서는 권한이 있는 유저가 접속을 했다고 인식을 하니 정상적으로 요청을 받고 응답하게 되는 것임.



- 디폴트로 CSRF middleware가 middleware setting에서 활성화됨.

- POST form을 사용하는 모든 template는 `csrf_token` tag를 form 안에 작성해야한다.

- ```django
  <form method="post">{% csrf_token %}
  ```

  

- 작동원리:

  - 사용자가 웹사이트 접속하면 Django에서 csrf_token을 client로 보내서 cookie로 저장하게 되고, 사용자가 form을 다 입력해서 제출버튼을 통해 submit하면 form뿐만 아니라 csrf_token이 POST 방식으로 서버로 같이 전송된다. 그리고 서버는 전송된 token의 유효성을 검증하고 유효한 요청이면 처리한다.

- 그래서 403error는 token이 유효하지 않거나 없을 때 나오는 에러이다.

- form태그 사용할 때는 `{% csrf_token %}` 이거 무조건 사용한다고 생각하자!



## Static files

정적인 파일 넣기:

강사님이 하신 절차 : 

- 새로운 app statics만들고, tags의 settings에 가서 `STATICFILES_DIRS = [BASE_DIR/'static']`추가 했음. static파일이 들어갈 경로를 지정한 것임. 즉 static file을 이 경로에 저장하면 된다 
  - 실제로 프로그램이 static파일을 찾을 경로라 생각하면 되는듯

- STATIC_URL 은 루트 경로라고 생각하면 된다.
  - template(.html파일) 에서 `{% load static %} ` 작성하고
  - `<link rel="stylesheet" href="{% static 'style.css' %}">` 여길 보면, static 태그 안에 'style.css' 만 작성했는데도 경로를 잘찾는걸 확인 할 수 있는다
  - 이것은 settings 내의 STATIC_URL = 'static/' 이기 때문에, 여기가 root경로가되어서 여기서부터 static파일을 찾게되는 것이다. (꼭 render에서는 root가 settings의 templates=[] 내의 dirs 키에 작성했던것 처럼!)
  - 즉 위에 static태그 명령어는 static file을 static/style.css에서 찾아라 이런 뜻.
  - 이거는 단순히 루트 url만 지정한다고 생각하면 된다



## Model

#### sqlite

> 굉장히 가벼운 데이터베이스로 이미 django에 내장되어있음.

__여기서부터 밑은 배운 순서 작성한 건데 순서가 100% 정확한게 아니라 수정 필요 :__

- 일단 dbtest라는 새 프로젝트 생성후에 installed app에 dbtest추가햇음 그 후에 dbtest에서 manage.py migrate실행

- 그 후 sqlite3 db.sqlite3 명령어 입력해서 SQlite접속 => .table통해 어떤 table이 생성되어있는지 확인

  ![image](https://user-images.githubusercontent.com/96896873/152560780-8c767a81-4850-4272-80ef-077661644358.png)

  => 생성된 테이블들을 확인할 수 있는데, 혹시 몰라서 지금 회색 박스로 가려놨지만 확인해보면 아직은 기본 table만 만들어져있는 상태였음.

- exit (quit도 가능)



- settings에 들어가서 :

- ```python
  INSTALLED_APPS = [
  .......
      'dbtest', # 이거 추가
  ]
  ```

=> 이부분은 app추가! (우리가 생각한 앱을 사용할 것이라는 것을 django에 알린 것임)



- models.py 파일에 클래스 생성

- makemigrations 명령어로 dbtest 하단에 migration file 생성!

- python manage.py sqlmigrate dbtest 0001> => 이 코드는 잘 만들어졌는지 확인하는 용도

- python manage.py migrate 한번 더 실행!
  - migrate 명령어는 셋팅의 installed-apps (현재 dbtest가 installed app에 추가되어있는 상황)확인하고 settings.py 파일과 기본 어플리케이션이 가지고 있는 데이터베이스 마이그레이션 파일에 따라 필요한 테이블을 만듦. 즉 처음에 이 명령어 입력하면 DB생성되고(지금 파이참에서는 보이지는 않지만 여튼 database하나 생성된 것임) Django에서 제공하는 테이블들(현재는 dbtest포함)이 자동으로 생성됨. 이름은 db.sqlite3로 저장



- 데이터베이스가 정상적으로 만들어졌는지 확인하기 위해 `sqlite3 db.sqlite3` 명령어 입력해서 SQlite 접속했음 (db실행방법임)
  - sqlite3 db.sqlite3 =>  sqlite파일 안에 있는 테이블 확인하는 명령어
  - 그 이후에 `.table`을 통해 어떤 table이 생성되었는지 확인했음!

![image](https://user-images.githubusercontent.com/96896873/152563621-d585d0d7-92f7-4d7f-960c-9cf938129348.png)

- 보다시피 dbtest_myboard가 추가되었음!
- exit()



이제 db다 생성했으니 db에서 데이터 가져와서 화면에 띄우는 작업 시작!

- `python manage.py shell` : shell창 띄워서 파이썬 언어로 db다루기시작
-  `from dbtest(앱이름).models import MyBoard(클래스) / from django.utils import timezone(날짜와 시간 사용하기 위해서)` : 파일 import하기; dbtest라는 앱의 models.py의 MyBoard라는 클래스 import했고 timezone이라는 객체 import했음
- `test = MyBoard(myname="testuser', mytitle...")` => MyBoard 클래스의 인스턴스 생성
- `test.save()` => dbtest_MyBoard라는 테이블에 인스턴스 값 저장
- 만약 여기서 MyBoard라는 객체의 정보를 가져오고 싶다면? `MyBoard.objects.all()` 입력!
- `exit()` => 나가기 



- views.py에 from .models import MyBoard 넣기! 그러면 이제 데이터베이스의 내용을 띄울 수 있음.



__여기서부터는 내가 공부해서 정리한 부분 :__

- 모델 : 데이터베이스 내의 테이블을 대표하는 class이다

- 한 모델이 한 테이블을 map한다, 
- 내가 아이디를 적지 않더라도 자동으로 테이블 내에 id를 생성한다

```python
from django.db import models

class MyBoard(models.Model): # 테이블 생성할 때 all model들이 inherit할 기본 기능들을 포함한게 models.Model임.
    myname = models.CharField(max_length=100)
    mytitle = models.CharField(max_length=500)
    mycontent = models.CharField(max_length=2000)
    mydate = models.DateTimeField()
```

- charfield()=> small amount of text

- textfild() => 큰 amount of text
- 하지만 이렇게 model이라는 py안에 작성만 해놓으면 아직까지는 DB입장에서 이 파일이 데이터로 저장되어야하는지 뭔지 모름. 즉 connected yet 상태인 것임. 즉 migrate model을 database에해야하는 것임!
  - 이 역할을 하는 것이 `python manage.py migrate`!
  - 근데 이 이전에 !!!! migration file 먼저 생성해야함
- migration file 생성 방법: `python manage.py makemigrations`
  - migration file은 내가 model에 주는 change를 track함
  - 그래서 내가 이미 생성한 MyBoard class에 대한 정보들이 쫙 적혀있음

- 그래서 migration file만들고 python manage.py migrate하면 dbtest.0001_initial 읽어가지고 실행 완료했다는 뜻. 즉 DB에 들어갔다는 것임



#### ORM

> 데이터베이스와 model을 활용해서 communication하는 법

- `python manage.py shell` -> interactive with database in shell로

- class명.objects.all() => query set을 return함
  - 만약 이 query set이 비어있다면 아무것도 삽입안한  것임
  - 그러면 이제 instance를 삽입해보자!
  - 그 전에 instance를 생성해야겠지?
- => shell에서 `test = MyBoard(myname='testuser', mytitle='test title', mycontent='test 1234', mydate=timezone.now()) ` 이런식으로 인스턴스 생성

- 여기까지는 instance 생성만 한거고, 이제 database에 save해야하니 
  - 인스턴스명.save() 즉, `test.save()` 해주자!



- 참고로 obejcts.all() 하면 class뭐시기로만 나왔는데, 

- ```python
  def__str__(self) :
  
  return self.title => string version
  ```

- 이걸 설정하고 나면 string으로 보여줌!







## 기타

- chrome web browser는 automatically appedns a trailing slash to URLs ! 
- urls파일의 path 경로에는 꼭 마지막에 trailing slash 붙여주자, 반대로 views 파일의 a태그 안에는 trailing slash없이 작성될 수도, slash있어도 관계없이 작동됨.

- 앱의 view파일은 직접 생성하기 (그냥 시스템이 그렇게 되어있음)

- 경로 작성할 때 만약에 앞에 슬래쉬 없이 시작하면, 그거는 내 위치에서 시작이고, 슬래쉬 있이 시작하면 root 경로에서 시작

- include는 바로 그냥 앱명 쓰기 

- html에서 POST방식으로 보낼 수 있는 건 form태그밖에 없고 나머지는 다 get방식이다

- 403 error -> 서버가 클라이언트의 접근을 거부할 때 발생









