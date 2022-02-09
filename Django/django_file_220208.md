# Django

## File

- updown이라는 이름의 project 생성하고

- settings.py에 base dir 작성, media 작업한다

  ```python
  ## settings.py
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [BASE_DIR/'templates'],
          .
          .
          .
          .
          
          
  MEDIA_URL = '/media/' # 내가 주소창에 media/filename 치면 이동가능 하도록
  MEDIA_ROOT = BASE_DIR/'media' # 이미지 업로드할 공간 지정
  ```

  

- 장고에서 일반적인 file들을 media file이라고 사용

### Upload

```html
## index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <h1>File Upload</h1>

    <form action="/upload/" method="post" enctype="multipart/form-data">{% csrf_token %}
        <input type="file" name="uploadfile">
        <br>
        <input type="submit" value="업로드">
    </form>

</body>
</html>
```

- index함수를 실행하게 되면 해당 index.html이 화면에 출력된다
- __파일을 서버에 전송하려면 enctype="multipart/form-data" 무조건 작성__
- file type으로 input 설정해야 파일 업로드 가능

##### enctype

- request.POST 요청 시에만 사용. 
- 패키징 방식으로 만약 작성하지 않으면 웹 서버로 데이터 넘길 때 파일의 경로명만 전송되고 파일 내용은 전송되지 않게됨
- application/x-www-form-urlencoded: 디폴트임!

- multipart/form-data : 파일 업로드할 때 사용 (많은 양의 바이너리 데이터 전송)
- text/plain : 실제로 쓰이지 않고 debugging에만 사용된다.



- upload를 하면 views.py의 upload process 함수 실행

```python
## views.py
from django.core.files.base import ContentFile

def upload_process(request):
    # index.html에 업로드 된 파일 받아줌
    upload_file = request.FILES['uploadfile']

    # settings에서 찾아놨던 media root찾아서 어디다 저장할지 정해줌. 
    
    uploaded = default_storage.save(upload_file.name,ContentFile(upload_file.read()))
    return render(request, 'download.html', {'filename': uploaded})

```

- POST방식으로 받아왔지만 file을 받아오려면 __request.FILES__에서 받아와야한다.
- `uploaded = default_storage.save(upload_file.name, ContentFile(upload_file.read()))` 여기서 save할 때
  - 첫번째 arguement는 filename이여야하고
  - 두번째 argument는 file 그 자체여야하는데
  - 지금 현재 두번 째 argument `ContentFile(upload_file.read())`는, upload_file 을 read했는데 __binary file__로 읽지 않음. 하지만 ContentFile을 쓰면 binary file로 전환되는듯
  - 그래서 두번 째 argument를 그냥 바로 upload_file로 작성해도 오류없이 잘 진행된다

```python
print(upload_file) # 내가 업로드한 파일 명
print(type(upload_file)) # 클래스 객체가 나옴
print(ContentFile(upload_file.read())) # rawcontent
print(type(ContentFile(upload_file.read()))) #contentfile 객체로
print(uploaded)
print(type(uploaded))
```



### Download

```python
## views.py
from django.core.files.storage import default_storage

def download_process(request, filename):
    print(filename)
    response = HttpResponse(default_storage.open(filename).read())
    # content-disposition은 브라우저한테 response를 file attachment로 treat하라는 뜻임
    response['Content-Disposition'] = f"attachment; filename={filename}"

    return response
```

```html
<!--download.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <h1>File Download</h1>

    <input type="button" value="다운로드" onclick="location.href='/download/{{ filename }}'">

</body>
</html>
```

```python
## urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
    path('upload/', views.upload_process),
    path('download/<str:filename>', views.download_process),
]
```

- download 버튼을 누르면 __filename이라는 변수로 해당 파일 이름__을 가지고 download_process 함수 실행
- default storage에 있는 해당 파일이름을 읽어와서 httpresponse를 할건데
- content-disposition은 브라우저한테 response를 file attachment로 treat하라는 뜻으로 이걸 하지 않으면 이전까지는 그냥 textfile?staticfile로 읽게 되겠지
- `response['Content-Disposition'] = f"attachment; filename={filename}"` 에서 ContentFile은 class중 하나인데 file로부터 inherit하는 것이고 그냥 File class와 다르게 string content도 제공한다
- default_storage는 우리가 settings에서 지정한 storage를 말하고, default_storage에서 해당 파일 이름을 읽어온다는 뜻이다.