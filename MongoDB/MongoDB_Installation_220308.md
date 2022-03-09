# MongoDB

- 설치

  - mongodb.com 들어가서 products -> community -> 오른쪽 available downloads에 버전 확인해서 다운로드

  - 설치할 때 **콤파스** 설치는 하지말기. (오류뜨는 경우 잦음) 나중에 필요하면 설치하기

- path 이어주기

  - 내컴퓨터 속성-> 고급 시스템 설정 -> 환경변수 -> 시스템변수(path클릭) -> 편집 -> 새로 만들기 -> 

C:\Program Files\MongoDB\Server\5.0\bin -> cmd에서 mongo 입력 -> exit

- 환경변수 path	

  - OS가 특정 프로세스를 실행시킬 때 그 경로를 찾는 데 사용

  - 환경 변수라는 메모리에 저장하기 때문에 일일이 파일 경로를 지정해서 파일을 실행할 필요가 없어짐

    - 원하는 파일의 경로를 PATH에 추가하면 cmd 창에서 해당 파일의 경로 지정없이 이름만 입력해도 실행됨

    

### Intro

- rdbs : 엔터티들의 **관계**를 지정해서 테이블 형태로 데이터를 관리할 수 있는 데이터베이스

- NoSQL

  - 스키마가 고정 X(즉 비정형 데이터) -> 필요할 때마다 필드 추가 / 제거

  -  데이터 간의 관계가 없기 때문에 DB -> COLLECTION -> Document

  -  분산형 구조 -> 대용량 데이터 저장 용이 (Sharding을 지원)
    - sharding : master가 slave 노드한테 데이터를 분산해서 (데이터 크기가 커서 잘라서 복사복사) 저장

