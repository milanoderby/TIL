# HTML DOM Event.target과 Event.currentTarget

### 공통

Event 인터페이스의 속성입니다.

<br>

### 차이

`Event.currentTarget`은 이벤트가 붙여진 HTML Element를 가리킵니다.

`Event.target`은 이벤트가 최종적으로 발동하는 HTML Element를 가리킵니다. 보통, 이벤트 전달의 방향은 DOM 트리의 하향이기 때문에 currentTarget의 자손노드가 될 수 있습니다.

<br>

### 참고자료

https://stackoverflow.com/questions/10086427/what-is-the-exact-difference-between-currenttarget-property-and-target-property

https://developer.mozilla.org/en-US/docs/Web/API/Event/target

https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget