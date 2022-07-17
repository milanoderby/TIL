# Rotate List

### 개념

Linked List와 k값이 주어질 때, Linked List를 k 값만큼 우측으로 회전한 Linked List를 반환합니다.

<br>

### 문제풀이

1. Linked List의 크기(Linked List 내부 Element의 개수)를 계산합니다.
2. k 값이 Linked List의 크기보다 크면, 여러번 회전하여 의미가 없으므로 k를 Linked List의 크기만큼 나눈 나머지만큼만 회전하기 위해 k값을 나머지 값으로 설정합니다.
3. k만큼 회전한다는 것은 앞의 (Linked List의 size - k)개의 Node가 뒷쪽으로 이동하는 것이고, 뒤의 k개의 Node를 앞쪽으로 이동한다는 것입니다.
4. 이를 위해 (Linked List의 size - k - 1)번째 Node의 next 노드를 null 값으로 만들고, 
5. 맨 마지막 노드의 next 노드를 처음으로 이동시키며,
6. (Linked List의 size - k)번째 노드를 결과노드로 반환합니다.

<br>

### 구현

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) {
            return null;
        }
        
        int sizeOfListNode = 0;
        ListNode temp = head;
        while (temp != null) {
            sizeOfListNode++;
            temp = temp.next;
        }

        k %= sizeOfListNode;
        temp = head;
        for (int i = 0; i < sizeOfListNode - k - 1; i++) {
            temp = temp.next;
        }
        ListNode newLastNode = temp;

        while (temp.next != null) {
            temp = temp.next;
        }
        temp.next = head;
        ListNode newFirstNode = newLastNode.next;
        newLastNode.next = null;

        return newFirstNode;
    }
}
```

