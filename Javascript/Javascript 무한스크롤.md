# Javascript 무한스크롤 구현방법

### 무한스크롤이란?

특정 페이지에서 화면에 컨텐츠 목록들이 나열되어있다고 할 때, 

사용자는 현재 보고 있는 컨텐츠를 다 보게되면, 다른 컨텐츠들도 더 찾아보고 싶어할 것입니다.

이 때, 다른 컨텐츠들을 불러오는 방법에는 크게 2가지가 있습니다.

1. 페이지번호를 직접 클릭
2. 무한스크롤

1번은 컨텐츠 목록 하단에서 다른 페이지번호를 눌러, 해당 페이지의 컨텐츠를 노출시키는 방법입니다.

2번 무한스크롤은 사용자가 컨텐츠목록의 최하단까지 모두 봤을 때, 즉 스크롤이 하단에 놓여지는 것을 감지하여 자동으로 다음 페이지의 컨텐츠를 가져와서 노출시키는 방식입니다.

<br>

### 스크롤 위치가 최하단임을 감지하기 위한 HTMLElement 속성

위에서 말했듯, 원리는 **현재 보고 있는 화면**에서 **스크롤의 위치 값**이 **전체 스크롤 중 하단** 에 위치할 때, 이벤트를 감지하는 것입니다.

이 이벤트를 감지하기 위해서, 다음 3가지 속성이 필요합니다.

#### 1. scrollHeight - HTMLElement 의 전체 높이 값

scrollHeight 속성은 `HTMLElement`의 읽기전용속성으로 해당 `HTMLElement`가 내포하는 컨텐츠의 높이 값입니다. 이 때, 단순히 보여지는 부분의 높이 값이 아닌, 스크롤 바를 통해 압축되어 있는 컨텐츠를 모두 펼쳤을 때, 컨텐츠의 높이 값입니다.

그림으로 살펴보면 아래와 같습니다.

![image-20210807215346163](./images/infinitescroll-1)

<br>

#### 2. scrollTop - HTMLElement 에서 스크롤된 높이 값

scrollTop 속성은 `HTMLElement`의 속성으로 현재까지 스크롤된 높이 값을 의미합니다.

스크롤이 최상단에 있으면, scrollTop의 값은 0입니다.

그림으로 살펴보면 아래와 같습니다.

![image-20210807215346163](./images/infinitescroll-2)

<br>

#### 3. clientHeight - HTMLElement 에서 보여지는 화면의 높이 값

clientHeight 속성은 `HTMLElement`의 읽기전용속성으로 해당 `HTMLElement`가 내포하는 컨텐츠의 높이 값입니다. 이 때, 스크롤 바를 통해 압축되어 있는 컨텐츠 전체의 높이 값이 아닌, 단순히 눈에 보여지는 컨텐츠의 높이 값을 말합니다.

그림으로 살펴보면 아래와 같습니다.

![image-20210807215346163](./images/infinitescroll-3)

<br>

### 스크롤 위치가 최하단임을 감지하는 공식

일반적으로 생각할 때, 무한스크롤을 발생시키는 스크롤의 위치가 최하단일 때를 감지하기 위한 공식은 아래처럼 생각할 것 입니다.

###### 현재 스크롤된 높이 값 >= 전체 화면의 높이 값

 하지만, `scrollTop`의 정의에 의하면, 스크롤이 최하단에 있을 때, `scrollTop`의 값은 `scrollHeight`의 값보다 작습니다. 그 이유는 scrollTop은 현재까지 스크롤된 즉, 현재 보고있는 화면의 최상단 위치까지의 컨텐츠의 높이를 계산한 값이기 때문입니다. 

 그래서, HTMLElement의 `스크롤`이 최하단에 위치할 때를 감지하는 이벤트의 공식은 아래와 같게 됩니다.

###### (현재 스크롤된 높이 값 + 현재 화면의 높이 값) >= 전체 화면의 높이 값

<br>

스크롤을 내릴 때마다 `scrollTop` 값이 변하는 것을 알 수 있습니다.

| 스크롤: 최상단                                        | 스크롤: 중간높이                                      | 스크롤: 최하단                                        |
| ----------------------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------- |
| ![image-20210808162220061](./images/infinitescroll-4) | ![image-20210808162453757](./images/infinitescroll-5) | ![image-20210808162538869](./images/infinitescroll-6) |

<br>

위의 예시를 확인하기 위한 코드는 다음과 같습니다.

```html
<!DOCTYPE html>
<html>
<head>
<style>
#myDIV {
  height: 250px;
  width: 250px;
  border: 100px;
  overflow: auto;
}

#content {
  height: 800px;
  background-color: coral;
}
</style>
</head>
<body>

<p>스크롤이 하단에 닿을 때, 감지하는 이벤트</p>

<div id="myDIV" onscroll="myFunction()">
  <div id="content">Scroll inside me!</div>
</div>

<p id="demo"></p>

<script>
function myFunction() {
  var elmnt = document.getElementById("myDIV");
  var clientHeight = elmnt.clientHeight;
  var scrollTop = elmnt.scrollTop;
  var scrollHeight = elmnt.scrollHeight;
  document.getElementById ("demo").innerHTML = "clientHeight: " + clientHeight + "px<br>" + "scrollTop: " + scrollTop + "px<br>" + "scrollHeight: " + scrollHeight + "px<br>";
}
</script>

</body>
</html>
```

<br>

### 최적화

#### 1. 스크롤이 최하단까지 내려가기 전에 감지

위의 공식은 좋지만, 최하단까지 완벽하게 내려가야 컨텐츠 목록을 불러오기 시작합니다. 

일반적인 페이스북 무한스크롤을 겪어본 사용자라면, 위와 같이 동작하지 않는다는 것을 알 것 입니다.

사용자 입장에서 스크롤을 내릴 때, 최하단까지 내릴 때가 아닌 스크롤이 어느정도 하단에 위치할 때 쯤, 컨텐츠를 읽어들여오는 것이 좋은 방식입니다.

그래서, 하단의 `offset` 이라는 개념을 추가하여 스크롤이 최하단에서 어느정도 위에 위치했을 때, 컨텐츠를 가져오는 개념으로 수정하는 것이 좋습니다. 위의 스크롤 감지 공식에서 `offset` 변수만 추가합니다.

###### (현재 스크롤된 높이 값 + 현재 화면의 높이 값 + 하단에서의 offset) >= 전체 화면의 높이 값

<br>

#### 수정된 코드

```html
<script>
function myFunction() {
  var elmnt = document.getElementById("myDIV");
  var clientHeight = elmnt.clientHeight;
  var scrollTop = elmnt.scrollTop;
  var scrollHeight = elmnt.scrollHeight;
  var offset = 100;
  document.getElementById ("demo").innerHTML = "clientHeight: " + clientHeight + "px<br>" + "scrollTop: " + scrollTop + "px<br>" + "scrollHeight: " + scrollHeight + "px<br>";
  if(scrollTop + clientHeight + offset >= scrollHeight) {
  	document.getElementById ("demo").innerHTML += 'Scroll is on bottom';
  }
}
</script>
```

<br>

#### 결과

![image-20210808174753968](./images/infinitescroll-7)

<br>

#### 2. 스크롤 감지 이벤트 발생지연

스크롤을 올리거나 내리는 행동은 일반적인 버튼 클릭하는 것과 다르게 연속적인 행동이므로, `onScroll` 이벤트는 엄청나게 자주 발생하는 이벤트입니다. 한번 스크롤을 내리는 와중에 함수 호출은 스크롤을 내리는 시간에 따라서, 최소 10 ~ 100번 정도 호출될 것입니다.

 그래서, `onScroll` 이벤트를 그대로 사용하는 것은 매우 불필요한 리소스 낭비를 야기합니다. 이러한 리소스를 낭비하기 위한 방법이 `lodash의 debounce` 를 이용하여 이벤트를 지연하는 것입니다.

이런 debounce는 여러번 발생하는 이벤트에서, 가장 마지막 이벤트만을 실행되도록 만드는 개념입니다.

대략적인 사용법은 다음과 같습니다.

`myFunction` 함수가 구현되어있을 때, 100ms 마다 스크롤 이벤트가 발생되도로고 하는 `handleDebouncedScroll` 함수는 아래와 같이 정의하며, 이벤트를 등록합니다.

```html
<script>
	this.handleDebouncedScroll = debounce(this.myFunction, 100);
    window.addEventListener('scroll', this.handleDebouncedScroll);
</script>
```

<br>

### 참고자료

- https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollHeight
- https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollTop
- https://developer.mozilla.org/en-US/docs/Web/API/Element/clientHeight
- https://stackoverflow.com/questions/45822150/how-to-listen-to-the-window-scroll-event-in-a-vuejs-component
- https://lodash.com/docs/4.17.15#debounce

