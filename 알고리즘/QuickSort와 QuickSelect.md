# QuickSort와 QuickSelect

### 정의

QuickSort와 QuickSelect 모두 **랜덤으로 선택한 Pivot** 을 이용하여 각각 배열을 정렬, 요소를 추출하는 알고리즘입니다.

**Divide And Conquer(분할정복기법)**을 이용합니다.

<br>

### QuickSort 동작방식

#### 1. Divide

배열에서 랜덤으로 요소하나를 선택합니다. 이를 **Pivot**이라고 부릅니다.

Pivot을 기준으로 작은 요소들은 Pivot의 왼쪽에, 큰 요소들은 Pivot의 오른쪽에 위치시킵니다.

**Pivot보다 작은 요소들만 모인 배열 L**, **Pivot보다 큰 요소들만 모인 배열 R** 그리고 **Pivot**으로 총 3개의 그룹으로 나뉘어집니다.

<br>

#### 2. Recursive

**L배열과 R배열**에 대해서 **1번 Divide 과정을 반복하며, 새로운 Pivot에 의해 분할(Partition)**됩니다.

**1번 Divide 과정의 종료 및 반환시점**은 배열의 크기가 1이 될 때입니다.

<br>

#### 3. Conquer

배열 L, Pivot, 배열 R을 합칩니다.

위의 Recursive Divde 과정을 거치기 때문에 정렬된 순서대로 합쳐지게 됩니다.

<br>

### QuickSort Partition 구현방법

#### Naive하게 Partition

Naive하게 Partition 방법을 생각하면, Pivot의 값을 기준으로 작은 배열과 큰 배열로 나눌 때, 배열을 새로 생성하는 방법을 생각할 수 있습니다. 하지만, 이 방법은 추가 메모리를 사용하기 때문에 공간효율이 그다지 좋지 못한 방법입니다.

 <br>

#### Lomuto Partition Scheme

이 방법은 위의 방법과 다르게 **추가 메모리를 사용하지 않는 방법**입니다.

이와 같이 추가 공간을 사용하지 않고, input으로 주어진 메모리 공간만 이용하여 결과를 도출해내는 알고리즘을 ***In-place 알고리즘*** 이라고 부릅니다. 물론, 추가적으로 상수 공간을 사용하는 것도 허용됩니다.

<br>

##### 동작방식

1. 배열의 가장 마지막 요소를 Pivot으로 선택합니다.

2. Pivot과 비교하는 수의 위치(이하, number_index), 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(이하, replace_index)를 가리키는 인덱스를 배열의 시작위치로 정의합니다.
3. Pivot과 number_index를 1씩 증가하면서, 아래 작업을 number_index가 Pivot-1이 될 때 까지 반복수행합니다.
   1. 비교하는 숫자가 Pivot보다 작다면, 비교하는 숫자를 replace_index에 위치한 숫자와 교체합니다. replace_index를 1증가시킵니다.
   2. number_index를 1증가시킵니다.
4. 마지막으로 Pivot을 현재 replace_index에 위치한 숫자와 교체합니다.

<br>

##### 예시

아래 배열에서 Pivot은 3이됩니다. 

| Pivot을 기준으로 분할하는 배열                               | 5    | 1    | 4    | 2    | 3    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             | 0    |      |      |      |      |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) | 0    |      |      |      |      |

비교하는 숫자인 5가 Pivot인 3보다 크거나 같으므로, number_index만 1증가됩니다.

| Pivot을 기준으로 분할하는 배열                               | 5    | 1    | 4    | 2    | 3    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             |      | 1    |      |      |      |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) | 0    |      |      |      |      |

비교하는 숫자인 1이 Pivot인 3보다 작으므로, 두 인덱스가 가리키는 숫자의 위치를 바뀝니다.

number_index와 replace_index를 1증가시킵니다.

| Pivot을 기준으로 분할하는 배열                               | 1    | 5    | 4    | 2    | 3    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             |      |      | 2    |      |      |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) |      | 1    |      |      |      |

비교하는 숫자인 4가 Pivot인 3보다 크거나 같으므로, number_index만 1증가됩니다.

| Pivot을 기준으로 분할하는 배열                               | 1    | 5    | 4    | 2    | 3    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             |      |      |      | 3    |      |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) |      | 1    |      |      |      |

비교하는 숫자인 2가 Pivot인 3보다 작으므로, 두 인덱스가 가리키는 숫자의 위치를 바뀝니다.

number_index와 replace_index를 1증가시킵니다.

| Pivot을 기준으로 분할하는 배열                               | 1    | 2    | 4    | 5    | 3    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             |      |      |      |      | 4    |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) |      |      | 2    |      |      |

마지막으로 Pivot을 현재 replace_index에 위치한 숫자와 교체합니다.

| Pivot을 기준으로 분할하는 배열                               | 1    | 2    | 3    | 5    | 4    |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- |
| 비교할 숫자의 위치(number_index)                             |      |      |      |      | 4    |
| 비교하는 수가 Pivot보다 작은 숫자일 때, 들어갈 위치(replace_index) |      |      | 2    |      |      |

<br>

### 시간복잡도

QuickSort의 시간복잡도는 다음 3가지로 생각할 수 있습니다.

#### Worst Case

만약, Lomuto Partition Scheme을 사용하는 QuickSort에서 이미 정렬이 완료된 데이터를 넣는다면, 어떻게 될까요? 배열의 가장 마지막 요소를 Pivot으로 설정하면, R배열의 크기는 0이 되고, Pivot을 제외한 배열의 모든 요소들은 L배열에 담기게될 것입니다. 중요한 것은 이러한 Divide 과정이 Recursive하게 계속되기 때문에 전혀 Pivot을 통해서 2개의 배열로 적절하게 나누어지는 효과를 볼 수가 없다는 것입니다. 이 때의 시간복잡도는 O(N^2)이며, 이게 QuickSort의 WorstCase입니다.

<br>

#### Best Case

QuickSort의 Best Case는 Pivot을 통해서 2개의 배열로 적절하게 나누어질 때이며, 이 때의 시간복잡도는 O(N logN) 입니다.

<br>

#### Average Case

QuickSort의 Average Case는 평균적으로 예상되는 시간복잡도를 의미하며, 이는 O(N logN)입니다.

컴퓨터과학자들이 다양한 방법을 이용하여 증명한 자료들이 있습니다.

참고: https://en.wikipedia.org/wiki/Quicksort#Average-case_analysis

<br>

### Pivot 선택 최적화

#### 여러가지 방법

Pivot을 적절하게 선택하지 않으면, 위와 같이 O(N^2)의 시간복잡도를 가지게됩니다.

적당히 좋은 Pivot을 선택하기 위한 방법으로 배열의 맨앞, 맨뒤, 중간에 위치한 값, 또는 완전 랜덤 위치의 값을 선택하는 방법 등이 있지만, 이 방법 모두 완벽하게 Worst Case를 방지할 수는 없습니다.

<br>

#### Median of medians

위의 단점을 보완한 Pivot 선택방법이 5명의 컴퓨터과학자들에 의해 발표되었는데, 이 방법이 Median of medians입니다. 쉽게 말하자면, 배열을 여러개의 데이터 셋으로 나누어서 각 데이터 셋에서 중위값을 구한 다음, 해당 중위값들을 모아놓은 값들 중에서 또 다시 중위값을 구하는 방식입니다. 이 방식을 이용하여 Pivot을 구하면, 이 Pivot에 의해서 나뉘어진 배열들은 최악의 경우에도 7:3 비율로 나뉘어지기 때문에 항상 O(N logN) 시간복잡도를 보장합니다.

하지만, 이는 중복 값이 들어있는 배열에 대해서 제대로 동작하지 않는 단점이 있으며, 배열의 크기인 N이 적절하게 클 때, 효율적인 방법입니다.

자세한 설명은 아래 문서에 잘 설명되어있습니다.

참고: https://modoocode.com/287

<br>

### QuickSelect 사용시기

QuickSelect는 **K번째 ~한 값을 찾을 때**, 사용할 수 있는 방법입니다.

QuickSort와 비슷한 접근방법을 이용하여 답을 찾습니다.

<br>

### QuickSelect 동작방식

#### 1. Divide

배열에서 랜덤으로 요소하나를 선택합니다. 이를 **Pivot**이라고 부릅니다.

Pivot을 기준으로 작은 요소들은 Pivot의 왼쪽에, 큰 요소들은 Pivot의 오른쪽에 위치시킵니다.

**Pivot보다 작은 요소들만 모인 배열 L**, **Pivot보다 큰 요소들만 모인 배열 R** 그리고 **Pivot**으로 총 3개의 그룹으로 나뉘어집니다.

<br>

#### 2. Recursive

1. Divide 과정 후, Pivot 요소 위치를 가리키는 인덱스가 K 보다 작다면, K번째 작은 값은 R배열에 있다고 할 수 있으므로 R배열에 대해 1번 Divide 과정을 수행합니다.

2. Divide 과정 후, Pivot 요소 위치를 가리키는 인덱스가 K 보다 크다면, K번째 작은 값은 L배열에 있다고 할 수 있으므로 L배열에 대해 1번 Divide 과정을 수행합니다.
3. Divide 과정 후, Pivot 요소 위치를 가리키는 인덱스가 K 라면, K번째 작은 값은 Pivot이라고 할 수 있으며, 이를 반환하고, 이 때가 재귀함수가 종료하는 시점입니다.

<br>

### 참고자료

https://en.wikipedia.org/wiki/Quicksort

https://en.wikipedia.org/wiki/In-place_algorithm

https://en.wikipedia.org/wiki/Quickselect

https://en.wikipedia.org/wiki/Median_of_medians

https://leetcode.com/problems/top-k-frequent-elements/solution/

https://modoocode.com/248

https://modoocode.com/287