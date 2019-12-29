# 排序列表

### 题目

在 O(nlogn) 时间复杂度和常数级空间复杂度下，对链表进行排序。

##### 示例 1

```/
输入: 4->2->1->3
输出: 1->2->3->4
```

##### 示例 2

```/
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```



### 解法

#### 解法一：自底向上归并排序

归并排序的时间复杂度为 O(nlogn)，符合题目要求。对于归并排序：

+ 数组：空间复杂度为 O(n)，由归并过程中新开辟的数组O(n)和递归调用O(logn)组成；
+ 链表：由于可以修改节点之间的指针，无需开辟额外空间，但还是存在递归调用函数带来的 O(logn) 的空间复杂度

因此，根据题目要求，不能使用递归，而采用自底向上的迭代方式

```c++
// 执行用时 :52 ms, 在所有 cpp 提交中击败了92.49%的用户
// 内存消耗 :11.5 MB, 在所有 cpp 提交中击败了95.90%的用户

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
    ListNode* sortList(ListNode* head) {
        // 求出链表长度
        ListNode* p = head;
        int length = 0;
        while(p){
            length++;
            p = p->next;
        }

        // 自底向上归并排序
        ListNode fakeHead(0);
        fakeHead.next = head;
        for(int size = 1; size < length; size <<= 1){
            // tail记录了当前一轮归并完的尾节点, cur记录了未归并的头节点
            ListNode* tail = &fakeHead;
            ListNode* cur = fakeHead.next;

            while(cur){
                ListNode* left = cur;
                ListNode* right = cut(cur, size);
                cur = cut(right, size);

                tail->next = merge(left, right);

                while(tail->next){
                    tail = tail->next;
                }
            }
        }
        return fakeHead.next;
    }

    // cut 函数将当前链表切割成两段，前一段长度为size，并返回切割后后一段的头节点
    // 注意分割后尾部要置空
    ListNode* cut(ListNode* head, int size){
        ListNode* p = head;
        while(p && --size){
            p = p->next;
        }

        if(!p)
            return NULL;
        else{
            ListNode* temp = p->next;
            p->next = NULL;
            return temp;
        }
    }

    // 归并
    ListNode* merge(ListNode* left, ListNode* right){
        ListNode fakeHead(0);
        ListNode* p = &fakeHead;
        while(left && right){
            if(left->val <= right->val){
                p->next = left;
                p = p->next;
                left = left->next;
            }else{
                p->next = right;
                p = p->next;
                right = right->next;
            }
        }
        p->next = left ? left : right;
        return fakeHead.next;
    }

};
```

