# Jupyter

- 아나콘다를 통해 주피터도 같이 다운
- Jupyter Notebook : 다양한 언어들 기반으로 문서 작성을 편리하게 가능케 하는 웹 응용 프로그램이다. 출력형식도 html, pdf 등 다양하게 가능하다

:heavy_check_mark:Jupyter Notebook화면(검정색)을 끄면 웹에서 작동을 안하니까 꼭 화면도 켜놓아야함.

- Jupyter Prompt :
  - cd\ -> 최상위 폴더 c:\로 이동
  - cd .. -> 바로 윗폴더로 이동
  - cd 폴더이름 -> 해당 폴더로 이동



## Jupyter Notebook

- 셀 :![셀](C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220106200227278.png)

- 위처럼 초록창은 입력모드, 파란창은 명령모드임
- Juptyer는 print()쓰지 않고 변수명만 작성해도 output이 정상적으로 출력



### 유용한 단축키:

- 명령모드에서 조작: (Ctrl + m/Esc)

  - Ctrl + Shift + '-' : 해당 포인터가 있는 곳 기준으로 셀 강제로 분리

  - a : 바로 위에 셀 생성

  - b : 바로 아래 셀 생성

  - Shift + m : 셀 합치기; merging

  - dd : 셀 삭제

  - o : 실행결과 열고 닫기

  - m : Markdown으로 변경
  - y : code로 변경

  

- 편집모드에서 조작: (Enter)

  - Ctrl + y (ctrl + z반대): 선택 셀 다시 실행

  - 실행키 Enter 조합:

    - Ctrl + Enter : 현재 셀만 실행
    - Shift + Enter : 현재 셀 실행 + 아래에 아무 셀도 없을 때는 새로운 셀 생성/ 존재한다면 아래 셀로 이동
    - Alt + Enter : 현재 셀 실행 + 아래 셀 존재 여부 관계없이 새로운 셀 생성