# Django

## MySQL 연결

1. 새로운 프로젝트 생성

   `django-admin startproject 프로젝트명 `

- 만약 mysqlclient가 설치되어 있지 않다면 `pip install mysqlclient` 하고 안되면 `conda install -c quantopian mysqlclient`

2. 새로운 프로젝트로 이동해서 settings.py에 정보 몇 개 추가

   `INSTALLED_APPS`에 `'프로젝트명'` 추가 (왜 apps에 프로젝트명 썼냐면, 이래야지 mysql하고 myboard가 연결이 되기 때문에)

   `DATABASE` 에 추가:

   ```python
   'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'mysql',
           'USER' : 'root',
           'PASSWORD' : 'MYSQL 비밀번호',
           'HOST' : 'localhost',
           'PORT' : '3306'
       }
   ```

    

3. models.py 생성하고 내부에 클래스 생성

```python
from django.db import models

class MyPlan(models.Model):
    myname = models.CharField(max_length=20)
    mytodo = models.CharField(max_length=500)
    mycontent = models.CharField(max_length=2000)
    mydate = models.DateTimeField()

    def __str__(self):
        return str({'myname':self.myname, 'mytodo':self.mytodo, 'mycontent':self.mycontent,'mydate':self.mydate})
```

4. `cd 프로젝트명`

5. 터미널창에 `python manage.py makemigrations 프로젝트명 `
6. 터미널창에 `python manage.py migrate`
7. 터미널창에 `python manage.py runserver`로 정상적으로 실행됐는지 확인



