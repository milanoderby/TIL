# Add Binary

### 개념

String 으로 주어진 이진수 2개를 더한 결과 값을 구하는 문제입니다.

<br>

### 문제풀이

1. 이진수 A, B 덧셈의 로직은 다음과 같습니다.
   - 2^0의 자리부터 2^1의 자리 2^2의 자리를 순회하며 다음 작업을 수행하며,
   - A, B 현재 자릿수에 있는 숫자를 더합니다. 이를 각각 a, b라고 부르겠습니다.
   - 이전 자리의 덧셈에서 받아올림 수(여기선, carry 라고 부르겠습니다.)가 발생할 때, 해당 숫자도 함께 더해야합니다.
   - 현재 자릿수에서 덧셈 결과는 (a + b + carry) % 2가 됩니다.
   - 새로운 받아올림 수는 (a + b + carry) / 2가 됩니다.
   - 이를 반복하여 연산하면 됩니다.
2. 다만, String으로 주어진 입력 값에서 2^0 자리는 문자열의 제일 뒤에 있으므로 이를 위해 처음 입력 값을 reverse 연산을 수행하고, 결과 값도 reverse 연산을 수행합니다.

<br>

### 구현

```java
class Solution {
    public String addBinary(String a, String b) {
        int carry = 0;
        String reverseStringA = new StringBuilder(a).reverse().toString();
        String reverseStringB = new StringBuilder(b).reverse().toString();

        StringBuilder answer = new StringBuilder();
        int i = 0, j = 0;
        for ( ; i < reverseStringA.length() && j < reverseStringB.length(); i++, j++) {
            int tempA = reverseStringA.charAt(i) - '0';
            int tempB = reverseStringB.charAt(j) - '0';

            int sum = tempA + tempB + carry;
            answer.append(sum % 2);
            carry = sum / 2;
        }

        int indexOfRemainedString = 0;
        String remainedString = null;
        if (i >= reverseStringA.length() && j >= reverseStringB.length()) {
            if (carry == 1) {
                answer.append(carry);
            }
            return answer.reverse().toString();
        }

        if (i >= reverseStringA.length()) {
            indexOfRemainedString = j;
            remainedString = reverseStringB;
        } else {
            indexOfRemainedString = i;
            remainedString = reverseStringA;
        }

        for (int k = indexOfRemainedString; k < remainedString.length(); k++) {
            int temp = remainedString.charAt(k) - '0';

            int sum = temp + carry;
            answer.append(sum % 2);
            carry = sum / 2;
        }

        if (carry == 1) {
            answer.append(carry);
        }
        return answer.reverse().toString();
    }
}
```

