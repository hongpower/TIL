# Javascript

## Popup

- window.open(경로, [이름], [옵션])
  - 경로만 작성하면 이름도 옵션도 없는 새창 팝업
  - 이름 : 지정한 "이름"을 name 속성으로 가지고 있는 곳 __안으로 팝업창이 열린다__
    - 예) window.open("http://naver.com", "myframe", ...) => myframe이라는 이름을 가진 태그를 찾아봄
  - 옵션 : width, height 등 설정 가능



## Window

- display : none  => 공간을 차지하지 않는 상태(즉 만들어지지 않은 상태)로 display를 감춘다
  - visibility : hidden => 공간은 차지하지만 보이지는 않는 상태. 즉 이미 만들어져 있음
- display : block => 인라인 요소를 block요소로 바꿀 때 사용하지만, none으로 만들어진 display를 다시 나타나게 할 때도 사용 가능

- for ( x in nodelist){} : 

  - 객체에 있는 non-symbol이고 enumerable한 속성을 순회할 때 사용

  - 객체 자신과 상속받고 있는 모든 constructor의 열거 속성들을 전부 순회하게 됨.

    - enumerable :  열거 가능한

  - 배열, Object와 같이 내장 constructor로 생성된 객체들은 모두 비열거 속성들을 상속받음. 이것들은 순회에 제외.

  - 만약 nodelist에 객체가 들어간다면 key값과 속성 다 가져올 수 있음

  - array에는 for in을 추천하지 않는게 undefined되어있는 것들은 불러오지 않기 X

  - ```javascript
    var btns = document.getElementByName("btn")
    for ( var i in btns ){
    	btns[i].disabled = "disabled"
    }
    // i라는 변수는 값뿐만 아니라 button들의 기본 속성까지 가져오기 때문에 모든 button속성들을 disabled하게 만들 수 있는 것임
    ```

- disabled

  - disabled = "disabled" or true
  - disabled = "" or false or "문자열 아무거나"

- opener.document.getElement.. : opener=부모창

- val = document.getElement__s__ByName("이름")[0].value => 이름이라는 속성 name을 가진 태그가 __하나뿐이더라도 elements기 때문에 요소가 하나인 노드리스트로 가져온다. 그래서 value를 가지고 오고 싶다면 []를 꼭 써줘야한다__



## Location

- location.href="" : 내가 가고 싶은 경로 지정, href는 property
- location.assign("") : 내가 가고 싶은 경로 지정, assign은 메소드
- location.replace("") : 덮어씌우기, 즉 내가 이 메소드를 실행한 창을 해당 주소로 replace해버린다. 그래서 뒤로가기해도 원래 창으로 get back 불가능
- location.reload() : 페이지 새로고침





## CheckBox

```html
<input type="checkbox" name="chk" value="red">빨강<br>
<input type="checkbox" name="chk" value="green">초록<br>
<input type="checkbox" name="chk" value="blue">파랑<br>
<input type="checkbox" name="chk" value="magenta">진홍<br>

<script>
    var chks = document.getElementsByName("chk")
    for (var 9=0; i<chks.length; i++){
        console.log(chks[i]) // 결과는 : <input type="checkbox" name="chk" value="red"> 이렇게 가져옴
    }
</script>
```

- input의 속성 "checked" : true 또는 false 값을 가진다. 사용자가 체크박스에 체크하면 checked속성은 true로 바뀐다

```javascript
if (chk.checked){
    document.getElementById(chk.value).style.backgroundColor = chk.value
}
// 만약 chk가 checked되어있으면 chk의 value인 "red"라는 id를 가진 태그의 backgroundColor를 "red"로 설정하라
```

- 배경색 지정 x하고 싶을 때 `style.backgroundColor = ''` 라는 empty string으로 변환 

  - :heavy_check_mark: 지정한 CSS property 를 제거 하고 싶을 때 :

    - removeProperty method 사용: `document.querySelector('div').style.removeProperty('background-color')`

    - Empty String 만들기:

      `style.backgroundColor = ''`

      여기서 null을 빈스트링 대신 써도 되지만, 나중에 split을 하면 null이 포함되는 경우가 있을 수 있음

- `<input type="checkbox" name="all" onclick="allCheck(this.checked)">` : 여기서 this는 onclick을 발생 시킨 이 태그를 지칭하며, 이 태그의 "checked"속성이 true인지 false인지가 allCheck함수의 인자로 들어간다.



## DOM

- `<a href="http://www.naver.com" onclick="return false">` : 클릭을 해도 naver.com으로 이동을 하지 않게 된다. (이벤트 전파 막기의 방법 중 하나이다)

  - 이벤트 전파란 : 여러 태그의 영역이 겹쳐서 있을 때 (예를 들어서 DIV영역 안에 P, P안에 SPAN 영역) SPAN영역에 onclick과 같은 event를 했을 때 div영역의 클릭이벤트까지 작동하는 경우
  - 이벤트 전파 중단 방법 :
    - event.preventDefault() : 현재 이벤트의 기본 동작 중단
    - event.stopPropagation() : 현재 이벤트가 상위로 전파되지 않도록 중단 (그러므로 하위 이벤트는 동작함)
    - return false

- 노드.parentNode : [object HTMLDivElement] => HTML에 속해있는 div 요소이다

- 노드.childNodes: 줄바꿈도 문자라고 생각하기 때문에 

  - #text라는 공백문자까지 가져온다

  - 만약 한줄로 태그를 작성했다면, #text가져오지 않음

  - 한줄로 작성되어있지 않는 자식 노드들을 가져와서

    - tagName 사용 시 : 줄바꿈은 undefined로 뜬다
    - nodeName 사용시 : #text로 나온다

    

## 기타

- eval 사용은 지양. 해킹 위험
- checked와 alt는 input type이 체크박스나 radio일 때만 가능
- indexOf 가 -1이면 미존재
- 태그.focus() : 깜빡거리는 포커스가 생김
- close()나 self.close()는 동일 
