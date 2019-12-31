# 环形链表

### 题目

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

说明：不允许修改给定的链表。



### 解法

快慢指针，快指针的速度是慢指针的两倍。

**有环判断**：如果快指针在运动中指向了NULL，表明链表无环；若快慢指针相遇了，则表明存在环。可以类比运动跑圈，当慢指针在环中运动了一圈后，快指针必定运动了两圈（多跑了一圈），那么肯定在过程中会相遇的。

**环入口判断**：当快慢指针相遇时，将快指针重新指向链表头节点，慢指针位置不变，然后使得两个指针速度相同，都每次走1个节点，他们第一次相遇的节点就是入口节点。

我们假设慢指针在之前的过程中一共走了n个节点才与快指针相遇，由于快指针的速度是慢指针的两倍，所以快指针走了2n个节点达到同样的节点。这也就意味着此时慢指针再走n个节点，肯定会再次来到这个节点。那么，当我们将快指针重新指向头节点，并使他们的速度相同，最多经过n个节点，快慢指针会在这个地方相遇，若在此之前，由于环的存在，它们第一次相遇肯定是环的入口（倒推易知）。

```c++
// 执行用时 :8 ms, 在所有 cpp 提交中击败了97.52%的用户
// 内存消耗 :9.8 MB, 在所有 cpp 提交中击败了30.64%的用户

class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == NULL || head->next == NULL){
            return NULL;
        }

        ListNode *slow, *fast;
        slow = head;
        fast = head;

        slow = slow->next;
        fast = fast->next->next;

        while(fast != NULL && slow != fast){
            slow = slow->next;
            fast = fast->next;
            if(fast == NULL)
                break;
            fast = fast->next;
        }

        if(fast == NULL)
            return NULL;
        
        fast = head;
        while(fast != slow){
            slow = slow->next;
            fast = fast->next;
        }
        return fast;

    }
};
```

