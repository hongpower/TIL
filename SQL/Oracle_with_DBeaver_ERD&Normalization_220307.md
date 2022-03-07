# Oracle

## ERD

> Entity Relationship Diagram

- 논리 ERD : 설게도와 같은 느낌이기 때문에 실제 코드가 영어더라도 한국어로 작성 가능. 이해를 위한 것이니
- 물리 ERD : 실제로 테이블 만든/생긴 모습.
- 논리와 물리 ERD는 다를 수 있음




1. 바커 표기법 (Barker Notation)

![image](https://user-images.githubusercontent.com/96896873/157051098-786257c6-4745-466c-a045-cc74d2657121.png)

- 사원은 빨간색, 부서는 초록색
  - 사원은 부서를 하나만 가질 수 있고,
  - 부서는 사원을 여러개 가질 수 있는 관계

- A : B 관계
  - 1 : 1 
  - 1 : N
  - N : N => N : N은 1:N, N:1로 중간에 테이블 하나더 만드는게 좋음

![image](https://user-images.githubusercontent.com/96896873/157051465-52209094-1f88-4e00-826f-38fb75f8ea35.png)

- 실선은 1, 점선은 0 포함
  - 첫번째는 1
  - 두번째는 0 또는 1
  - 세번째는 0 또는 그 이상
  - 네번째는 1 또는 그 이상




2. I/E 표기법 (Information Engineering Notation)/ 까마귀 발 모델

- A - B관계

  - 종속 관계 (1번째 ,실선) : A가 없으면 B가 존재 불가능 
  - 독립 관계 (2번째, 점선) 
  - 종속/독립 관계는 애매한 부분이 많아서 정의하기 나름임
    - 그래도 좀 명료한거는 ''학교-학생'' -> 종속 관계

  ![image](https://user-images.githubusercontent.com/96896873/157052053-1609e8b2-918f-452b-bfd9-867e92b57d7f.png)



![image](https://user-images.githubusercontent.com/96896873/157052282-754d4f08-ed5b-4194-aece-76417e306da1.png)

- 동그라미 : 0



모든 사진의 출저는 이동헌강사님의 PDF 파일로부터 .. 자유롭게 사용 가능하다고 하셨음!



## Normalization

- 역정규화 /반정규화 : 논리적으로는 정규화를 통해 나눠야한다고 판단했으나, 물리적으로 굳이 나눌 필요가 없다고 판단해서 다시 합치는걸 역정규화, 반정규화라고 부름


