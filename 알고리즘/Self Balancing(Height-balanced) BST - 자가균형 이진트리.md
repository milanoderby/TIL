# Self Balancing (Height-balanced) BST

한국어로는 자가균형 이진트리라고 불립니다.

이 트리는 일반적인 BST (Binary Search Tree)에서 발생할 수 있는 단점인 한쪽으로 치우쳐진 형태의 이진트리형태를 만들지 않게하기 위해 **rebalancing** 이라는 연산을 수행합니다.

<br>

**rebalancing** 연산을 어떤 방식으로 수행하느냐에 따라서 여러가지 Self-balancing BST가 있습니다. 

다음은 그 예시입니다.

- AVL 트리
- Red Black 트리

<br>

### AVL 트리

#### 성질

##### 1. Height balance property (높이 균형 성질)

모든 노드 v에 대해서 **v의 자식노드들의 최대 높이 차이는 1**입니다.
$$
|height(left(v)) - height(right(v))| <= 1
$$

##### 2. 높이 균형 성질로 인해 N개의 원소를 가지는 AVL트리의 높이는 O(log N) 입니다.

<br>

#### 삽입연산

일반적인 BST의 삽입연산과 동일하게 삽입됩니다.

다만, **height- balance property** 를 유지하기 위해 추가 연산을 수행합니다.

AVL트리에서 삽입 후, 불균형한 Subtree를 **rebalancing** 하는 과정을 **trinode restructuring** 이라고 부릅니다.

<br>

##### 삽입 연산 시, 용어 정리

먼저, 삽입되는 노드를 **w** 라고 명합니다.

그리고, **w**에서 root 노드로 가는 경로에서 **높이 불균형**이 발생하는 첫 번째 노드를 **z** 라고 명합니다.

**y**는 **z**의 자식노드 중 더 큰 높이를 가지는 노드입니다. 

**x**는 **y**의 자식노드 중 더 큰 높이를 가지는 노드입니다.

<br>

삽입연산 시에는 다음과 같이 **w** 에서 **z** 로 향하는 경로에 **x**와 **y**가 존재합니다.

![image-20210827040400714](./images/AVL-tree-1)

<br>

#### 삭제연산

일반적인 BST의 삭제연산과 동일하게 삭제됩니다.

다만, **height- balance property** 를 유지하기 위해 추가 연산을 수행합니다.

AVL트리에서 삭제 후, 불균형한 Subtree를 **rebalancing** 하는 과정도 **trinode restructuring** 연산이 이루어집니다.

<br>

##### 삭제 연산 시, 용어 정리

먼저, 삭제되는 노드의 부모노드를 **w** 라고 명합니다.

그리고, **w**에서 root 노드로 가는 경로에서 **높이 불균형**이 발생하는 첫 번째 노드를 **z** 라고 명합니다.

**y**는 **z**의 자식노드 중 더 큰 높이를 가지는 노드입니다. 

**x**는 **y**의 자식노드 중 더 큰 높이를 가지는 노드입니다.

<br>

삭제연산 시에는 다음과 같이 **w** 에서 **z** 로 향하는 경로에 **x**와 **y**가 존재하지 않을 수도 있습니다.

![image-20210827041341491](./images/AVL-tree-2)

<br>

#### Trinode Restructuring Algorithm

##### 동작방식

노드 **x**, **y**, **z** 를 inorder(중위순회)로 나열한 것을 각각 **a**, **b**, **c** 라고 부르겠습니다.

이 때, **b** 노드를 세 노드 중에 가장 위에 위치하도록 **x**, **y**, **z** 세 노드를 **회전**시키는 것이 Trinode Restructuring Algorithm 의 핵심원리입니다. 다음은 동작순서입니다.

1. 노드 **x**, **y**, **z** 를 inorder(중위순회)로 나열한 것을 각각 **a**, **b**, **c** 라고 부릅니다.
2. **a**, **b**, **c** 의 subtree 중 **a**, **b**, **c** 를 제외한 것들을 inorder(중위순회)로 나열한 것을 T0, T1, T2, T3이라고 부릅니다.
3. **z** 를 root로 하는 subtree를 **b**를 root로 하는 새로운 subtree로 바꿉니다.
4. **a** 가 **b**의 왼쪽자식노드가 되고, T0와 T1은 **a**의 왼쪽, 오른쪽 자식노드가 됩니다.
5. **c** 가 **b**의 오른쪽자식노드가 되고, T2와 T3은 **c**의 왼쪽, 오른쪽 자식노드가 됩니다.

<br>

이 동작순서를 구체화하면, 

**b** = **y** 일 때, **y**와 **z**를 한번만 회전하는 모양입니다. 그래서 **single rotation** 이라고 부릅니다.

![img](./images/AVL-tree-3)

**b** = **x** 일 때는 **x**와 **y**를 회전시키고, **x**를 **z**와 한번 더 회전하는 모양입니다. 그래서 **double rotation** 이라고 부릅니다.

![img](./images/AVL-tree-4)

<br>

#### 시간복잡도

위의 작업은 부모-자식 관계만 바꾸는 것이므로 O(1) 시간복잡도를 가지게 됩니다.

즉, N개의 원소를 가지는 AVL트리의 삽입 / 삭제연산 시간복잡도는 O(log N) 입니다.

<br>

### 출처

https://ko.wikipedia.org/wiki/AVL_%ED%8A%B8%EB%A6%AC