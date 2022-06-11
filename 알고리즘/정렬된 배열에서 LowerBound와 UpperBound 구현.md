# 정렬된 배열에서 LowerBound와 UpperBound 구현

### LowerBound

정렬된 배열에서 특정 숫자(이하, target)를 탐색하였을 때, target의 index 중 가장 낮은 값을 말합니다.

<br>

### UpperBound

정렬된 배열에서 특정 숫자(이하, target)를 탐색하였을 때, target의 index 중 가장 높은 값을 말합니다.

<br>

### 문제풀이 방법

1. 정렬된 배열의 범위 안에 target 값이 있는지 확인합니다.
   - target < 배열의 첫번째 요소의 값일 경우, LowerBound와 UpperBound 값은 찾을 수 없습니다.
   - target > 배열의 마지막 요소의 값일 경우, LowerBound와 UpperBound 값은 찾을 수 없습니다.
   - 정렬된 배열의 범위 안에 target 값이 있다면, 2번, 3번 과정을 수행합니다.
2. LowerBound 탐색
   - 이분탐색을 시행합니다.
   - start = (배열의 첫번째 index), end = (배열의 마지막 index), mid = (start + end) / 2로 설정합니다.
   - target == 배열의 첫번째 요소의 값(arr[start])일 경우, LowerBound는 start가 됩니다. 더 탐색하지 않고 종료합니다.
   - target == 배열의 마지막 요소의 값(arr[end])일 경우, LowerBound 가 될 수 있는 end 값을 저장하고, 아래 과정을 수행합니다.
   - start + 1 == end 가 될 때까지 아래 과정을 반복합니다.
     - arr[mid] < target 이라면, start = mid 대입합니다.
     - arr[mid] > target 이라면, end = mid 대입합니다.
     - arr[mid] == target 이라면, LowerBound 가 될 수 있는 mid 값을 저장하고, end = mid 대입하여 LowerBound가 될 수 있는 값을 탐색합니다.
3. UpperBound 탐색
   - 이분탐색을 시행합니다.
   - start = (배열의 첫번째 index), end = (배열의 마지막 index), mid = (start + end) / 2로 설정합니다.
   - target == 배열의 마지막 요소의 값(arr[end])일 경우, UpperBound 는 end가 됩니다. 더 탐색하지 않고 종료합니다.
   - target == 배열의 첫번째 요소의 값(arr[start])일 경우, UpperBound 가 될 수 있는 start 값을 저장하고, 아래 과정을 수행합니다.
   - start + 1 == end 가될 때까지 아래 과정을 반복합니다.
     - arr[mid] < target 이라면, start = mid 대입합니다.
     - arr[mid] > target 이라면, end = mid 대입합니다.
     - arr[mid] == target 이라면, UpperBound 가 될 수 있는 mid 값을 저장하고, start = mid 대입하여 LowerBound가 될 수 있는 값을 탐색합니다.

<br>

### Java 코드 구현

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        return new int[]{getLowerBound(nums, target), getUpperBound(nums, target)};
    }
    
    private static int getLowerBound(int[] nums, int target) {
        if (nums.length == 0) {
            return -1;
        }

        int start = 0;
        int end = nums.length - 1;
        if (target < nums[start] || target > nums[end]) {
            return -1;
        }

        if (target == nums[start]) {
            return start;
        }

        int lowerBound = -1;
        if (target == nums[end]) {
            lowerBound = end;
        }

        while (start + 1 < end) {
            int mid = (start + end) / 2;
            if (nums[mid] < target) {
                start = mid;
            } else if (nums[mid] > target) {
                end = mid;
            } else {
                lowerBound = mid;
                end = mid;
            }
        }

        return lowerBound;
    }

    private static int getUpperBound(int[] nums, int target) {
        if (nums.length == 0) {
            return -1;
        }

        int start = 0;
        int end = nums.length - 1;
        if (target < nums[start] || target > nums[end]) {
            return -1;
        }

        if (target == nums[end]) {
            return end;
        }

        int upperBound = -1;
        if (target == nums[start]) {
            upperBound = start;
        }

        while (start + 1 < end) {
            int mid = (start + end) / 2;
            if (nums[mid] < target) {
                start = mid;
            } else if (nums[mid] > target) {
                end = mid;
            } else {
                upperBound = mid;
                start = mid;
            }
        }

        return upperBound;
    }
}
```

