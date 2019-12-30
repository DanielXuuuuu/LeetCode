# 删除链表的倒数第N个节点

### 题目

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

使用一趟扫描实现。



### 解法

由于链表的单向性，我们只能向前不能向后，那么要找到倒数第n个节点，直观的方法是遍历一遍得到总长度后再进行第二次遍历。但题目要求使用一次遍历实现，那么就需要使用到类似快慢指针的实现方式。

使用快慢两个指针，其中块指针始终比慢指针领先n个节点。由于我们要删除第n个节点，那么我们实际需要得到的是倒数第n+1个节点，因此在循环中判断的是`while(front->next != NULL)	`，若是要得到倒数第n个节点，那么循环就要修改为`while(front != NULL)`。

同时在循环结束后判断`n == 1`的情况，这张情况表明要删除的是头节点。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head == NULL)
            return head;

        ListNode *front, *back;
        front = back = head;
        
        while(front->next != NULL){
            if(n != 0){
                front = front->next;
                n--;
            }else{
                front = front->next;
                back = back->next;
            }
        }   
        
        if(n == 1)
            return back->next;

        back->next = back->next->next;
        return head;
    }
};
```

