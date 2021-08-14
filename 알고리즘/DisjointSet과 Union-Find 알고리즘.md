# DisjointSet과 Union-Find 알고리즘

### 구현소스코드

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

