# Python

## file

- os 모듈:

  - os.mkdir('디렉토리명')
  - os.path.isdir('디렉토리명') : 해당이름의 디렉토리가 존재하는지 bool값으로 반환
  - os.walk(디렉토리주소): 디렉토리 목록보기
    - 활용: for root, dirs, files in os.walk('python/Day7')
  - os.path.exists(디렉터리주소): 해당 이름의 파일 또는 디렉토리가 존재하는지 bool 값으로 반환. isdir은 디렉토리만 가능하지만 exits는 파일도 가능
  - os.path.isfile or isdir(주소): 디렉토리인지 파일인지 구분
  - os.remove('주소'): 파일 삭제
  - os.path.getsize('주소'): 파일 크기 확인. byte단위

  

- shutil 모듈: 디렉토리에 대한 복사, 삭제 등을 수행할 수 있는 모듈

  - shutil.rmtree('디렉토리명')

  

- zipfile 모듈:

  - zipfile.Zipfile('파일경로/파일이름', 'w') : 해당 파일을 zip한다

    - 활용: new = zipfile.Zipfile('c:/pythonStudy/Day8/Day8.zip', 'w')

    - new.write('파일이름명',compress_type=zipfile.ZIP_DELATED)
    - new.close()

  - zipfile.ZipfFile('파일경로/파일이름','r')

    - ext = zipfile.ZipfFile('c:/pythonStudy/Day8/Day8.zip', 'r')
    - ext.extractall('파일이름')
    - ext.close()

    

- 이진파일(binary file): 글자가 아닌 비트 단위로 의미가 있는 파일 : 그림파일, 음악파일, 동영상파일, 엑셀파일, 한글파일, 실행(exe)파일

  - binary file 읽기 or 쓰기: open('이진파일이름','rb'/'wb')

  - 이진파일 복사 (읽고 쓰기):

    ```python
    f1 = open('c:/windows/notepad.exe','rb')
    f2 = open('c:/Temp/notepad.exe','wb')
    
    while True:
        inStr = f1.read(1) #1바이트 읽어라
        if not inStr: #빈값이라면
            break
        f2.write(inStr)
        #f2에 글을 씀
        
    f1.close()
    f2.close()
    ```

- pickle모듈 : 메모리에 로딩된 객체나 정의된 클래스를 파일로 저장하여 사용

  - pickle.dump(객체,f) : 객체를 binary file에 저장

    - 활용: 

    ```python
    f = open('list.pickle','wb')
    result = [1,2,3,4,5]
    pickle.dump(result,f)
    f.close()
    ```

    

  - pickle.load() : 객체 로딩  

    ```python
    f = open('list.pickle','rb')
    result2 = pickle.load(f)
    print(result2)
    result2.append('데이터')
    f.close()
    ```

