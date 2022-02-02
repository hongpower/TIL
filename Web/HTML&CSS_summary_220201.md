# HTML/CSS

# HTML

- input태그 중 radio type은 label을 명시하기

```html
<li>
    <li><input type="text"></li>
	<li><input type="password"></li>
</li>
<li>
    <label><input type="radio" name="fruit" id="orange">오렌지</label>
	<label><input type="radio" name="fruit" id="strawberry">딸기</label>
	<label><input type="radio" name="fruit" id="apple">사과</label>
</li>
```

- radio는 같은 선택지라는 것을 명시하기 위해 name은 같게, id는 다르게

- submit 버튼은 바로 전송한다는 단점때문에



- 속성 작성할 때 인라인 태그는 최대한 지양.(style=""속성) 왜냐하면 cssfile에서 따로 작업하더라도 인라인 태그가 우선순위가 되기 때문에 최대한 꾸미는 요소들은 따로 cssfile내에 작성하자
- 참고: JS는 body의 마지막 파트에 작성



## CSS

- padding: 내 자신의 크기 늘리기 (4방향 다)
  - padding-top: 위에만 패딩 
  - padding-left 등 패딩 상화자우 다르게 줄 수 도 있음
- margin: 상대방과의 간격
  - margin-top : padding과 마찬가지로 상하좌우 다르게 가능

- border:
  - border-bottom
  - border-collapse: collapse => border선들이 디폴트로 다 떨어져있을 때 선이 하나로 잡혀짐



초반 설정:

```css
*{
	border-collapse:collapse;
    margin:0px;
    padding:0px;
    text-decoration: none; /*a태그 쓰면 파란색깔 밑줄 들어가 있는데 이런 기본 설정 지우고 싶을 때 사용*/
    color: black;
    list-style: none; /*마찬가지로 li태그 썻을 때 점들 삭제 */ 
}
```



##### display

- none: 존재하지만 보이지 않게
- inline
- block
- flex (참고 링크 : https://studiomeal.com/archives/197)
  - flex의 아이템들(자식요소)는 자신이 가진 내용물의 width만큼만 공간을 차지하고, 가로로 배치된다. 반면에 높이는 컨테이너의 높이만큼!
  - float는 height마저도 자식이 가진 내용물만큼만 늘어났지만, flex는 자식들이 동일한 높이를 갖는다
  - flex를 가지고 있는 부모요소(container)의 자식들에 `flex:1;`이런식으로 주면 부모요소 안에서 1칸만큼 차지하라는 뜻. 만약 다른 자식이 `flex: 2;` 라면 이 자식은 2만큼 차지 



##### justify-content

- display 속성과 함께 사용

- 메인축 방향 정렬 
  - 메인축 = 오뎅의 꼬치
- flex-end : 메인축의 오른쪽부터 정렬



#### cascading :

- 여러 개의 source들을 cascade down 하는 것. 즉 CSS는 hierachy가 있고 높은 위치에 잇는 style이 먼저 적용이 되고 그 위에 다음 위치의 소스가 overwrite되는 것.
- 여러 stylesheet를 한 html에 적용한다면 : 
  - 첫번 째 stylesheet를 `<link>`를 활용해서 작성하고 그 밑에 또 stylesheet를 `<link>`를 활용해서 작성하면 덮어씌우기로 작성 가능 



## 기타 :

- 글짜색은 상속이 불가능 하니 정확히 지정해주기
- margin: auto => 중앙 정렬. 브라우저가 margin을 계산하며 여백이 균등하게 배분. auto는 width를 지정해 주어야 한다
- HTML은 대소문자 구분 x
- 단축키:
  - CTRL + 1 : 첫번째 창 이동
  - Alt + Shift + 아래방향키 : 복사가능
  - Ctrl + Alt : 위 아래 다중커서 가능