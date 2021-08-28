# DisjointSet (서로소 집합 자료구조)와 Union, Find연산

### 정의

서로소란, 공통으로 포함하는 원소가 없는 두 집합의 관계를 말합니다.

서로소 집합 자료구조란, 모든 원소들이 서로소 부분집합들로 나뉘어진 것을 의미합니다.

2개의 연산을 제공합니다.

<br>

### 연산

#### 1. Find 연산

원소가 속한 집합을 반환합니다.

#### 2. Union 연산

두 개의 집합을 하나의 집합으로 합칩니다.

<br>

### 구현방법

#### 1. Disjoin Set Linked Lists

Find 연산은 원소의 Head까지 역추적하는 방식으로 O(n)의 시간복잡도를 가집니다.

Union 연산은 한 리스트의 헤드 부분을 다른 리스트의 꼬리를 가리켜서 O(1)의 시간복잡도를 가집니다.

이 글에서는 Linked Lists 방식 구현까지 다루지는 않겠습니다.

#### 2. Disjoint Set Forest

Forest 라고 표현한것은 여러 개의 Tree를 포함하기 때문입니다.

Find 연산은 원소가 속한 Tree의 Root를 찾는 연산이며, 이 역시 불균형적인 Tree를 생성될 수 있기 때문에 O(n)의 시간복잡도를 가지게 됩니다.

Union 연산은 한 Tree의 다른 Tree의 Root 노드를 가리키게하며, O(1)의 시간복잡도를 가지게 됩니다.

<br>

### 최적화

위의 2가지 구현방법 모두 최적화되지 않은 방법입니다. 아래 2가지 방법으로 위 방법의 성능을 향상시킬 수 있습니다.

#### 1. 유니온 바이 랭크

이 글에서는 다루지 않겠습니다.

#### 2. 경로 압축

Tree에서 Find 연산 시, 순회 중 방문한 노드들이 루트노드를 가리키도록 합니다.

효과로 Tree의 Node들이 Root의 자식노드가 되므로 높이가 줄어듭니다.

<br>

### 구현소스코드

다음은 `경로 압축` 방식으로 최적화한 Disjoint Set Forest의 구현입니다.

```java
private static class DisjointSet {
	private int[] group;

	public DisjointSet(int size) {
		group = new int[size];
		for (int i = 0; i < size; i++) {
			group[i] = i;
		}
	}

	public void unionGroup(int a, int b) {
		int groupOfA = findGroup(a);
		int groupOfB = findGroup(b);

		int min = Math.min(groupOfA, groupOfB);
		int max = Math.max(groupOfA, groupOfB);
		group[max] = min;
	}

	public int findGroup(int i) {
		if (group[i] == i) {
			return i;
		}
		return group[i] = findGroup(group[i]);
	}
}
```

<br>

### 참고자료

https://ko.wikipedia.org/wiki/%EC%84%9C%EB%A1%9C%EC%86%8C_%EC%A7%91%ED%95%A9_%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0
