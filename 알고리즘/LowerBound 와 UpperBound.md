# LowerBound 와 UpperBound

## 개념

### Lower Bound

`단조증가수열 arr` 에서 `찾고자 하는 값(target)` 이상인 첫번째 원소의 인덱스

<br>

### Upper Bound

`단조증가수열 arr`에서 `찾고자 하는 값(target)` 보다 큰 첫번째 원소의 인덱스

<br>

## 구현방법

공통적으로 이분탐색을 이용하여 구현합니다.

### Lower Bound

1. 이분탐색이 시작하면, 탐색의 범위를 제한해주는 변수인 `start`와 `end`가 되는 값은 다시는 탐색이 되지 않는 값입니다. 그래서, 2가지 방법으로 이런 엣지케이스 조건을 확인합니다.

   - `start`, `end` 일 때를 미리 확인해줍니다.

   - 이분탐색 수행 시, `start`, `end` 를 확인하도록 범위를 늘려줍니다.

   - ###### start, end 변수의 초기 값이 Upper Bound 구현코드와 다릅니다. Lower Bound 에서는 start일 때, 탐색을 미리하여 답을 찾아낼 수 있고, Upper Bound에서는 end일 때, 탐색을 미리하여 답을 찾아낼 수 있기 때문입니다.

2. `arr[0]`이 `target` 이라면, `0` 이 Lower Bound입니다.

3. arr은 단조증가배열이므로, 다음 성질이 성립합니다.

   - `arr[k - 1]` < `target` 이면서, `arr[k]` >= `target` 라면 k가 Lower Bound입니다.
   - 즉, 가장 처음으로 `arr[k]` >= `target` 성립하는 인덱스 k를 찾은 것이기 때문입니다.

4. 이제 2번의 조건을 만족하는 `k 값`을 찾기위해 이분탐색을 수행합니다.

   - 2번의 조건을 만족하는 k를 찾으면 바로 반환합니다.

   - 2번의 조건은 만족하지 않지만, `target` < `arr[k]`이라면, 탐색범위를 좌측으로 옮겨줍니다.

   - ###### 2번의 조건은 만족하지 않지만, `target` == `arr[k]`이라면, 탐색범위를 좌측으로 옮겨줍니다. - 이 조건은 Upper Bound와 다릅니다. Lower Bound는 target == arr[k] 를 만족하는 k값 중 가장 작은 값이기 때문입니다.

   - 2번의 조건은 만족하지 않지만, `target` > `arr[k]`이라면, 탐색범위를 우측으로 옮겨줍니다.

<br>

### Upper Bound

1. 이분탐색이 시작하면, 탐색의 범위를 제한해주는 변수인 `start`와 `end`가 되는 값은 다시는 탐색이 되지 않는 값입니다. 그래서, 2가지 방법으로 이런 엣지케이스 조건을 확인합니다.

   - `start`, `end` 일 때를 미리 확인해줍니다.

   - 이분탐색 수행 시, `start`, `end` 를 확인하도록 범위를 늘려줍니다.

   - ###### start, end 변수의 초기 값이 Lower Bound 구현코드와 다릅니다. Lower Bound 에서는 start일 때, 탐색을 미리하여 답을 찾아낼 수 있고, Upper Bound에서는 end일 때, 탐색을 미리하여 답을 찾아낼 수 있기 때문입니다.

2. `arr[arr.length - 1]`이 `target` 이라면, `arr.length - 1` 이 Upper Bound입니다.

3. arr은 단조증가배열이므로, 다음 성질이 성립합니다.

   - `arr[k]` <= `target` 이면서, `arr[k + 1]` > `target` 라면 k가 Upper Bound입니다.
   - 즉, 가장 마지막으로 `arr[k]` <= `target` 성립하는 인덱스 k를 찾은 것이기 때문입니다.

4. 이제 2번의 조건을 만족하는 `k 값`을 찾기위해 이분탐색을 수행합니다.

   - 2번의 조건을 만족하는 k를 찾으면 바로 반환합니다.

   - 2번의 조건은 만족하지 않지만, `target` < `arr[k]`이라면, 탐색범위를 좌측으로 옮겨줍니다.

   - ###### 2번의 조건은 만족하지 않지만, `target` == `arr[k]`이라면, 탐색범위를 우측으로 옮겨줍니다. - 이 조건은 Lower Bound와 다릅니다. Upper Bound는 target == arr[k] 를 만족하는 k값 중 가장 큰 값이기 때문입니다.

   - 2번의 조건은 만족하지 않지만, `target` > `arr[k]`이라면, 탐색범위를 우측으로 옮겨줍니다.

<br>

## 구현 소스코드

구현소스코드는 LowerBound UpperBound 의 개념을 사용하는 Leetcode 문제를 구현한 것입니다.

https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

다만, 일반적인 LowerBound UpperBound과는 다릅니다. 문제의 조건 중 배열 arr 에 target 값을 찾을 수 없을 때, -1을 반환하는 조건이 있기 때문입니다. 그 점만 제외하면 위에서 설명한 구현방법과 동일합니다.

### Lower Bound

```java
private static int lowerBound(int nums[], int target) {
    if (nums.length == 0) {
        return -1;
    }

	int start = 0;
	int end = nums.length;
	if (target == nums[start]) {
		return start;
	}

	while (start + 1 < end) {
    	int mid = (start + end) / 2;
        if (nums[mid - 1] < target && nums[mid] >= target) {
            return nums[mid] == target? mid : -1;
        } else if (target <= nums[mid]) {
            end = mid;
        } else {
            start = mid;
        }
    }
    return -1;
}
```

<br>

### Upper Bound

```java
private static int upperBound(int nums[], int target) {
	if (nums.length == 0) {
		return -1;
	}

	int start = -1;
	int end = nums.length - 1;
	if (target == nums[end]) {
		return end;
	}

	while (start + 1 < end) {
		int mid = (start + end) / 2;
		if (nums[mid] <= target && nums[mid + 1] > target) {
			return nums[mid] == target? mid : -1;
		} else if (target >= nums[mid]) {
			start = mid;
		} else {
			end = mid;
		}
	}
	return -1;
}
```

