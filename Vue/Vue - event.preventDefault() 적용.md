## Vue - event.preventDefault() 적용

태그에 걸려있는 기본 이벤트가 실행되지 않도록 하는 명령어

## 예시

### 이벤트 취소 함수

``` javascript
function stopDefAction(evt) {
  evt.preventDefault();
}
```

위와 같이 이벤트 취소 함수가 있는 상황에서, 아래의 엘리먼트를 예시로 보겠습니다.

<br>

### \<input type="checkbox">

``` html
<input type="checkbox" onclick="stopDefAction(event);"/>
```

위와 같이 html 엘리먼트가 있다면, 체크박스 클릭이 정상적으로 동작하지 않습니다.

<br>

### \<input type="text">

``` html
<input type="text" onkeypress="stopDefAction(event);"/>
```

위와 같이 html 엘리먼트가 있다면, 키보드를 눌러도 문자가 입력되지 않습니다.

<br>

### \<a href="#">

``` html
<a href="#gototag" onclick="stopDefAction(event);"/>
```

위와 같이 html 엘리먼트가 있다면, 클릭해도 설정한 태그로 이동되지 않습니다.

<br>

## Vue 에서 적용

``` javascript
<a href="#gototag" @click.prevent="stopDefAction"/>
```

## 참고자료

* [https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault](https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault)