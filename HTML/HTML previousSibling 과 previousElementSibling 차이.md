# HTML previousSibling 과 previousElementSibling 차이

### 공통

두 property 모두 부모가 같은 형제노드 중 가장 가까운 이전 노드를 반환합니다.

<br>

### 차이

`previousElementSibling`은 이전의 형제노드 중 element 노드만 찾아서 반환하지만,

`previousSibling`은 이전의 형제노드 중 element 노드, text 노드 comment 노드를 반환합니다.

<br>

### 참고자료

https://stackoverflow.com/questions/52467728/difference-between-previoussibling-and-previouselementsibling-javascript

https://teamtreehouse.com/community/previoussibling-vs-previouselementsibling