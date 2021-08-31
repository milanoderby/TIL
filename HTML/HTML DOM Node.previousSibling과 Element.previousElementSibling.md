# HTML DOM Node.previousSibling과 Element.previousElementSibling

### 공통

두 property 모두 공통 부모를 가지는 형제를 반환합니다.

<br>

### 차이

`Node.previousSibling`은 이전의 형제 중 Element 노드, Text 노드 Comment 노드를 반환합니다.

`Element.previousElementSibling`은 이전의 형제 중 Element 노드만 찾아서 반환합니다.

<br>

### 주의

다음 [예시](https://developer.mozilla.org/en-US/docs/Web/API/Node/previousSibling#second_example)문서를 살펴보면 알 수 있듯이, `previousSibling`은 Element를 반환하는 것을 보장하지 않기 때문에 한 줄 공백으로 나뉘어진 2개의 Node에서 사용 시, Text노드를 반환할 수 도 있습니다. 일반적으로, 이런 Text 노드는 개발자가 조작을 원하는 노드가 아니기 때문에 HTMLElement를 반환받고 싶다면, `previousElementSibling` 을 사용해야 됩니다.

<br>

### 참고자료

https://developer.mozilla.org/ko/docs/Web/API/Node/previousSibling

https://developer.mozilla.org/en-US/docs/Web/API/Element/previousElementSibling

https://stackoverflow.com/questions/52467728/difference-between-previoussibling-and-previouselementsibling-javascript

https://teamtreehouse.com/community/previoussibling-vs-previouselementsibling



