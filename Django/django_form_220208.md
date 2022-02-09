# Django

## admin 접속

```python
python manage.py createsuperuser

Username (leave blank to use 'jisuh'): admin # username인 admin만 내가 직접 입력한 것임

Email address: admin@admin.com: # emailaddress인 admin@admin.com만 내가 입력한 것임

Password: 입력!
```



- admin.py 파일 생성

- ```python
  # admin.py
  
  from django.contrib import admin
  from .models import MyBoard, MyMember # 아직까지는 MyBoard이라는 객체만 models.py에 생성한 상태
  
  admin.site.register(MyBoard)
  admin.site.register(MyMember)
  ```

- ```python
  # urls.py
  
  # admin 추가
  urlpatterns = [
      path('admin/', admin.site.urls),]
  ```






## 직접 회원가입, 로그인, 로그아웃 생성하기

### Register 생성 (회원가입)

```python
## models.py
# 새로운 클래스 객체 MyMember 생성

class MyMember(models.Model):
    myname = models.CharField(max_length=100)
    mypassword = models.CharField(max_length=100)
    myemail = models.CharField(max_length=100)

    def __str__(self):
        return str({'myname': self.myname, 'mypassword': self.mypassword, 'myemail': self.myemail})
```

- 이렇게 생성후 수정했으니,` makemigrations mymember` & `migrate` 해주자



```python
## views.py
# 암호화 시키는 make_password 함수 import
from django.contrib.auth.hashers import make_password
```

=> settings.py의 installed_apps에 두번 째에 보면 `'django.contrib.auth'` 가 있는데 이미 장고가 만들어논 UI로서 이것을 import한 것임, 마찬가지로 `'django.contrib.admin'` 이것도 이전에 사용한 admin인 것이고



```python
## views.py
def register(request):
    if request.method == 'GET':
        return render(request, 'register.html')
    elif request.method == 'POST':
        myname = request.POST['myname']
        mypassword = request.POST['mypassword']
        myemail = request.POST['myemail']

        mymember = MyMember(myname=myname, mypassword=make_password(mypassword), myemail=myemail)
        mymember.save()

        return redirect('/')

    return redirect('/')
```

```html
<!--register.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <h1>Register</h1>

    <form action="/register/" method="post">{% csrf_token %}
        NAME : <input type="text" name="myname">
        <br>
        PASSWORD : <input type="text" name="mypassword">
        <br>
        EMAIL : <input type="text" name="myemail">
        <br>
        <input type="submit" value="회원가입">
    </form>

</body>
</html>
```

- 일단 index.html에서 회원가입이라는 anchor 태그를 누르면 `/register/`로 이동하도록 프로그래밍 되어있음
  - index.html모습 : `<a href="/register">회원가입</a>`

- register/로 이동하면 urls.py의  `path('register/',views.register)` 를 통해 views.py의 register함수를 실행



- __anchor 태그를 활용한 이동은 url주소 뒤에 쿼리스트링이 추가되니 GET방식을 통해서 접근한 것임__
- 그러면 if 조건문의 조건에 해당이 되어서, 이 때 register.html을 사용자의 화면에 띄우는 것임

- 사용자가 register.html의 form을 다 작성하고 submit버튼을 누르면 __이번에는 POST 방식으로 같은 /register/라는 곳으로 해당 데이터를 전송__



- 결국 또 다시 views.py의 register함수를 실행하게 되는데, 사용자가 입력했던 값들을 변수에 저장하고 이 변수들을 활용해서 MyMember 객체인 mymember 변수를 생성!
- 이 mymember변수를 DB에 save하고 원래 페이지로 get back.



### Login 생성

```python
## views.py
from django.contrib.auth.hashers import make_password, check_password

def login(request):
    if request.method == 'GET':
        return render(request, 'login.html')
    else:
        myname = request.POST['myname']
        mypassword = request.POST['mypassword']

        mymember = MyMember.objects.get(myname=myname)
        # check_password는 두 인자가 같은 값인지 확인
        if check_password(mypassword, mymember.mypassword):
            # 여기서 mymember라는 row를 가져와서, myname을 session에 저장되어있는 myname이라는 키의 값으로 바꾼다
            request.session['myname'] = mymember.myname
            return redirect('/')
        else:
            return redirect('/login')
```

```python
## login.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

  <h1>login</h1>

  <form action="/login/" method="post">{% csrf_token %}
    NAME : <input type="text" name="myname">
    <br>
    PASSWORD : <input type="text" name="mypassword">
    <br>
    <input type="submit" value="로그인">
  </form>

</body>
</html>
```

- 아까와 같은 절차로, index.html에서 login 버튼을 누르면 `/login/`으로 이동하고 urls.py를 확인해보면 `path('login/',views.login)` 이렇게 적혀있으므로 login함수를 실행



- 마찬가지로 anchor tag로 이동했으니 get방식에 해당되어서 login.html을 화면에 띄움

- 사용자가 폼 입력해서 post 방식으로 제출하면 login 함수의 else 구문실행

  - 사용자가 입력한 데이터 변수에 할당하고

  -  사용자가 입력한 username에 해당되는 데이터를 DB에서 가져와서 mymember라는 변수에 삽입

  - check_password(mypassword, mymember.mypassword)

    - 만약 mymember.mypassword (사용자가 입력한 __username에 해당되는 password__)와
    - mypassword(사용자가 __직접입력한 password__)가 같다면
    - `request.session['myname']=mymember.myname` => session 안의 myname이라는 변수를 사용자가 입력한 username으로 바꾸어라
    - :white_check_mark: checkpassword는 복호화(암호화된 값을 다시 원상복귀)기능을 포함함
    - 비밀번호가 틀리면 login 페이지로 get back.
  

##### Session

> 참고 : https://docs.djangoproject.com/en/4.0/topics/http/sessions/
>
> https://docs.djangoproject.com/en/4.0/ref/request-response/#django.http.HttpRequest 

- view function의 첫번째 argument들은 모두 session attribute를 가지고 있다

- session은 dictionary형태의 object이다
- 하나의 server는 여러 클라이언트가 접근 가능하고, 서버 입장에서는 클라이언트를 구분해야 한다. 각 클라이언트에 대한 정보를 session은 보유하고 있고, 이 session으로 서버는 클라이언트 구부니이 가능하다
- 시크릿 모드 단축키인 Ctrl + Shift + N 을 사용해서 실험해 보면 로그인 된 상태의 링크를 시크릿 모드에 복붙해도 server가 다른 session을 읽기 때문에 로그아웃된 상태로 화면이 띄워진다



### Logout 구현

```python
def logout(request):
    del request.session['myname']
    return redirect('/')
```



- 이미 session에 사용자의 정보가 저장되어 로그인이 되어있는 상태에서
- index.html의 로그아웃 링크를 누르면 urls.py를 거쳐가서 views.py의 로그아웃 함수 실행
- 세션에서만 해당 myname 정보를 삭제하고 기본 화면으로 돌아가는 것이기 때문에 __del이 있다고 해서 DB에서 사용자 정보가 삭제되는 것은 아니다__



## Django auth.forms에서 생성

- app 추가하고 settings.py에서
  - `'DIRS': [BASE_DIR/'templates']` 작성
- 아까까지와는 다르게 models.py는 직접 생성하지 않는다 => 장고가 대신 모델 객체를 만들어주기 때문에 ! 대신 forms.py 작성한다!

```python
## forms.py

from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User

class MyMemberForm(UserCreationForm):
    """
    UserCreationForm이 가진 기본적인 필드 (자동으로 생성) : username, password1, password2
    password1 : 비밀번호
    password2 : 비밀번호확인
    """
    email = forms.EmailField()
    first_name = forms.CharField()
    last_name = forms.CharField()

    class Meta:
        # 모델과 필드를 지정하면, 모델 폼 같은 경우는 자동으로 폼 필드를 생성한다
        model = User
        fields = ('username', 'password1', 'password2', 'first_name', 'last_name', 'email')
```

- django에 기본적으로 내장되어있는 UserCreationForm을 사용해서 MyMemberForm이라는 객체를 생성한 것임 
  - UserCreationForm클래스를 상속해서 만들지 않고 그대로 사용해도 되지만 부가 속성으 ㄹ추가할 때는 이렇게 상속해서 만든다
  
- 만약 username, password1, password2 이외에도 추가로 속성 추가하고 싶다면 안에 작성해줄 것.

- class Meta:
  - 직접 필드를 정의하는 일반 폼과 달리 지금 이 경우는 model form을 사용하고 있는데, 이런 경우는 model과 필드를 지정해야한다. 지정하면 모델폼이 자동으로 폼 필드를 생성한다
  - 즉 mymemberform은 그냥 기존에 있는 username, password1, password2에다가 email, first_name, last_name 추가한 정도에 그치는 거고, django가 생성하기를 바란다면 class Meta작성
  
  

### Reigster

- 이전과 굉장히 유사

```python
## views.py
from django.shortcuts import render, redirect
from .forms import MyMemberForm

def register(request):
    print(request)
    if request.method == "POST":
        form = MyMemberForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('index')

    else:
        return render(request, 'register.html', {'form': MyMemberForm()})
```

```html
<!--register.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <h1>Register</h1>

    <form action="." method="post">{% csrf_token %}
        <table>
            {{ form.as_table }}
        </table>
        <input type="submit" value="회원가입">
    </form>

</body>
</html>
```



- index.html에서 버튼을 누르면 urls.py를 통해 views.py내의 register 함수 실행

- 현재는 anchor tag의 href를 통해서 실행된 것이라 method가 get이므로 else 구문 실행.
- `{'form': MyMemberForm()}` 를 register.html로 이동하면서 같이 전달
  - 우리가 이전에 forms.py에 작성했던 MyMemberForm 객체를 생성한 것임.
- register.html로 돌아와서 
  - `{{ form.as_table }}` 은 UserCreationForm에 있는 기본적인 기능으로서 tabel형태로 form을 그릴 수 있는 형태 종류이다.
  - as_table말고도 다양한 형태 존재
- submit버튼을 누르면 __"."__ 즉 내 위치로 post방식으로 데이터 전달 (전달되는 데이터는 form형태로 전달되지는 않는듯)
- 내 위치는 /register/고 그러면 다시 register 함수 실행
- if 조건에 해당되기 때문에 request.POST내에 저장되어있는 데이터를 활용해서 다시 mymemberform의 객체인 form이라는 변수 만든다.
- form이 유효하다면 DB에 저장하고 get back



### Login & Logout

```python
## settings.py
LOGIN_REDIRECT_URL = '/result'
LOGOUT_REDIRECT_URL = '/'
```

```python
## urls.py
from django.contrib import admin
from django.urls import path
from . import views
#  views랑 겹치지 않도록 별칭지어줌 :
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('register/', views.register, name='register'),
    path('login/', auth_views.LoginView.as_view(template_name='login.html'), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
    path('result/', views.result, name='result'),
]
```

```html
<!--login.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <form action="{% url 'login' %}" method="post">{% csrf_token %}
        ID : <input type="text" name="username">
        <br>
        PW : <input type="text" name="password">
        <br>
        <input type="submit" value="로그인">
    </form>

</body>
</html>
```



- login 링크를 누르면 로그인 뷰를 여는데 login.html이라는 우리가 만든 template을 열기 위해서 `auth_views.LoginView.as_view(template_name='login.html')`처럼 작성한다.
  - 이 코드가 어렵다면, 그냥 form으로 로그인 폼을 만든다면 이렇게 작성한다고 생각하자
- login.html에 내용 작성하고 submit하면 login이라는 이름을 가진 (즉 같은 경로인 login/)로 데이터를 post방식으로 전송



```html
## result.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <h1>Hello, {{ user.username }}</h1>

    <a href="/logout/">로그아웃</a>

</body>
</html>
```

```python
## views.py
def result(request):
    return render(request, 'result.html')
```



- 앞서 settings.py에서 login redirect url이 /result로 설정되어있으므로 /result로 이동하고, 결국 result함수 실행하게 된다
- `user.username`은 우리가 직접 user를 정의해줄 필요없고 자동으로 전달된다! (즉 이미 장고내에서 만들어져있는 user 객체)





