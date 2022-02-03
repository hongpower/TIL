# Django

## 가상환경

> Virtual environments(venv)

- 가상환경이 필요한 이유 : 다른 버전의 모듈을 사용하는 파이썬, django 등이 필요할 때 가상환경이라는 것을 만들어서 이 환경 내에 파이썬이든 django든 원하는 버전의 프로그램을 설치해서 이용할 수 있다
- 만드는 방법:

1. Virtual environments(venv 모듈 활용)

- python -m venv 가상환경이름
- .\가상환경이름\Scripts\activate.bat => venv 실행

2. Conda 가상환경

- 콘다에서 가상환경 생성 :
  - `conda create -n myweb python=3.9` => 3.9버전의 python을 가지고 있는 __myweb이라는 가상환경__을 생성한다; 형식은 conda create -n <가상환경이름> <설치할 파이썬 버전>


- `y` => yes 치면 설치 시작
- `conda activate myweb` => myweb이라는 가상 환경에 들어가기
- `conda info --envs` => 정보 출력
- `pip install django` => 가상환경 내에 django 설치
- `cd /workspaces/workspace_django` => 경로 이동



### 가상환경 만든 곳에서 프로젝트 생성

=> 아마 여기부터 파이참 아래 터미널 사용

- `django-admin startporject 프로젝트이름` => 해당 프로젝트이름의 프로젝트를 생성
- `cd mysite`
- `python manage.py runserver` => manage.py라는 해당 프로젝트 전체를 장고가 관리하는 파일을 통해 runserver를하면 접속 url을 뱉어줌. 이 코드는 python manage.py 파일 아래에서 실행



- 앱 생성:

  - 하나의 프로젝트는 여러 앱으로 구성되어있음
  - `python manage.py startapp hello01` => hello01이라는 app을 생성. 이거는 manage.py라는 파일이 있는 위치에서 실행

  - 새로운 앱에는 url.py파일이 없으니 생성해주자

  

  - `ctrl+c` + `conda deactivate`를 통해서 나가주면 된다.

  



## Basic

#### django 프로젝트 내 기본 파일들 :

- manage.py -> django자체에 대한 설정

- wsgi -> web server gateway interface

- asgi -> asynnchronous server gateway interface : WAS의 동기/비동기 통신 지원(django 3.0이상부터)

- settings.py -> 해당 프로젝트의 환경 설정을 맡음

- urls.py -> 요청 url을 어떤 처리하는 거랑 연결할 건지 경로 설정.

순서 : wsgi.py -> urls.py -> views.py -> models.py 



server에는

1. web server : 정적인 파일들 저장(static) => 예를 들어 html, css, image, mp3 등 이미 만들어져있는 파일들을 연결. 빠르다는 장점

2. web application server(WAS): 동적인 파일들 저장 => django와 같이 유저가 새로운 버튼을 누르면 새로운 사이트가 자동으로 생성되는 등 동적인 기능 구현 가능. 



#### django 기본 개념:

- 장고는 MVT패턴

  - Model : 데이터베이스와 관련된 것
  - View : business logic, 요청한 데이터 처리를 담당하는 곳. 함수가 정의 됨.
  - Template : 실제로 보여지는 곳. 사용자에게 present 하는 곳

  

#### urls.py

> function mapping을 도와주는 파일

- `from django.urls import path` => path를 활용해서 function을 link와 연결할 것임

- path할 때 콤마 잊지말기 !!

- `from . import views` => 나랑 같은 위치에 있는 views라는 파일 import함

- `path('', views.index),` => 아무런 명령이 없거나 or 기본 주소 일 때 views.py의 index함수를 실행
  - 중요한 건 함수를 지금 실행시키는게 아니고 어떤 유저가 행동을 했을 때 views.index를 실행하기 때문에 () 없어야함!
  
- 각 앱마다 urls.py를 따로 만들고 어떤 요청이 들어왔을 때 그 때마다 urls.py로 연결하는 것이 url의 역할이다 생각하자

  - ```python
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
    path('', include('app.urls')),
    ]
    ```

  - 제일 큰 url파일에 앱들의 url연결

  - include는 새로 직접 추가한 app이라서 작성.

  

#### views.py

- index라는 함수는 요청받아서 response하는 함수.

- ```python
  def index(request):
  	return HttpResponse("HTTP문법 내용 작성")
  ```

- HttpResponse => 인자로 받은 문자열을 HTTP로 화면에 보여주는 함수



## 기타 :

- 단축키 

  - alt + 1 왼쪽 사이드바 토글

  - alt + insert 하면 새파일 만들기

- ssr => server side ; django처럼 요청이 들어오면 처리한 내용을 __서버가 만들어서(template)__ 응답
- csr => client side ; ajax와 같이 일부만 해주는 경우?

hello->hello->views.py파일 만듬 ???????/



