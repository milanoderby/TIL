# (2, 4) Tree

(2, 4) Tree는 Multi Way Search Tree의 일종입니다.

모든 internal node가 자식노드로 2개, 3개, 4개의 노드를 가지는 트리를 의미합니다.

<br>

### 특징

1. Size property

   모든 내부 노드(internal node)들의 자식 노드의 개수는 최대 4개입니다.

   v번째 자식노드를 찾는 연산의 시간 복잡도는 O(1)입니다.

2. Depth property

   모든 외부 노드(external node)들의 깊이는 같습니다.

   높이를 최대한 낮게 유지하도록 강제합니다. 

   높이가 h이고, 모든 내부노드의 개수가 n일 때, h = O(log n) 입니다.
   $$
   n >= 1 + 2 + 4 + ... + 2^{h-1} = 2^h - 1\\
   n + 1 >= 2^h\\
   log(n + 1) >= h
   $$

 <br>

### 삽입 연산 (Insertion)

새로운 노드 `v`를 추가할 때, 추가될 노드의 위치를 key를  통해 Tree내부에서 탐색합니다.

`v`의 key를 탐색 시, 최종적으로 도달하는 외부노드(external node) `z`라고 할 때, `z`의 좌측에 새로운 노드를 추가합니다.

<br>

#### Overflow

새로운 노드 추가 시, 위의 삽입연산은 Depth property는 지키지만, Size property는 위반할 수 있습니다.

새로운 노드가 추가될 때, 노드의 개수가 5개가 될 때, 이를 **overflow**라고 부릅니다.

<br>

#### Split

Size property를 지키도록 하려면 **overflow**를 처리하기 위해 **split** 연산을 해야합니다.

**split** 연산은 다음 과정을 거칩니다.

1. 새로운 자식노드가 추가된 노드 `v`의 key는 4개가 되고, 자식노드는 5개가 될 것입니다.
2. `v`의 key들을 각각 오름차순으로 k1, k2, k3, k4라고 하고, 자식노드는 v1, v2, v3, v4, v5 라고가정합니다.
3. 노드 `v`를 `v'`, `v''`로 교체합니다.
4. 노드 `v'` 은 k1, k2 및 자식노드 v1, v2, v3을 가지고, 노드 `v''` 은 k4 및 자식노드 v4, v5로 구성합니다.
5. k3는 노드 v의 부모노드 `u`에 추가합니다.
6. 부모노드 `u`에서 **overflow**가 발생하면, 부모노드에서도 **split**연산을 합니다.

<br>

#### 시간복잡도

1. 추가할 위치를 찾는 연산: O(log n)
2. overflow 발생 시, split 연산: O(log n)

<br>

### 삭제 연산 (Deletion)

`leaf 노드`를 자식노드로 가지는 노드 삭제 시, 자식노드와 Entry를 삭제합니다.

`internal node`만 자식으로 가지는 노드 삭제 시, **inorder 순서** 상 다음 노드와 삭제할 노드의 위치를 바꾼 후, 삭제합니다.

#### Underflow

노드 v 삭제 시, 위의 삭제연산을 수행 시, 노드 v가 1개의 자식노드만 가지게 되는 경우가 발생합니다.

이렇게  Size property는 위반되는 경우를 **underflow**라고 부릅니다.

underflow는 2가지 경우를 고려해야 합니다.

1. 삭제된 노드 `v`의 인접형제노드가 3개, 4개 자식을 가지는 노드일 때: **transfer** 연산으로 해결합니다.
2. 삭제된 노드 `v`의 인접형제노드가 2개 자식을 가지는 노드일 때: **fusion** 연산으로 해결합니다.

<br>

#### Transfer

삭제된 노드를 `v`, 삭제된 노드의 부모노드 `u`, `v`의 인접 형제노드 `w`(이 때,  v의 모든 인접형제노드 중 하나인 w는 3개, 4개 자식을 가지는 노드입니다.) 가 있을 때, **underflow** 를 해결하는 방법입니다.

1. `u`의 entry 를 `v`로 이동시킵니다.
2. `w`의 entry 를 `u`로 이동시킵니다.
3. transfer 연산은 underflow가 다른 노드로 전파되지 않습니다.

<br>

#### Fusion

삭제된 노드를 `v`, 삭제된 노드의 부모노드 `u`, `v`의 인접 형제노드 `w`(이 때, v의 모든 인접형제노드는 2개 자식을 가지는 노드입니다.) 가 있을 때, **underflow** 를 해결하는 방법입니다.

1. `v`를 `w`와 병합합니다.
2. `u`의 entry 를 병합된 노드 `v'`로 이동시켜 3개의 자식노드를 가지도록 합니다.
3. fusion 연산은 underflow가 부모노드 `u`로 전파될 수 있습니다.
4. underflow가 전파된 부모노드 `u`가 root노드일 경우, u를 삭제합니다.
5. underflow가 전파된 부모노드 `u`가 root노드가 아닐 경우, fusion이나 transfer연산을 취해줍니다.

<br>

#### 시간복잡도

1. 삭제할 위치를 찾는 연산: O(log n)
2. underflow 발생 시, 연속적인 fusion 연산과 transfer연산: O(log n)

### 참고자료

https://en.wikipedia.org/wiki/2%E2%80%933%E2%80%934_tree