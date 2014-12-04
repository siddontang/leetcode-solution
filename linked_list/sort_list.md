# Sort List

> Sort a linked list in O(n log n) time using constant space complexity.

这题要求我们对链表进行排序，我们可以使用divide and conquer的方式，依次递归的对链表左右两半进行排序就可以了。代码如下：

```c++
class Solution {
public:
   ListNode *sortList(ListNode *head) {
        if(head == NULL || head->next == NULL) {
            return head;
        }

        ListNode* fast = head;
        ListNode* slow = head;

        //快慢指针得到中间点
        while(fast->next && fast->next->next) {
            fast = fast->next->next;
            slow = slow->next;
        }

        //将链表拆成两半
        fast = slow->next;
        slow->next = NULL;

        //左右两半分别排序
        ListNode* p1 = sortList(head);
        ListNode* p2 = sortList(fast);

        //合并
        return merge(p1, p2);
    }

    ListNode *merge(ListNode* l1, ListNode* l2) {
        if(!l1) {
            return l2;
        } else if (!l2) {
            return l1;
        } else if (!l1 && !l2) {
            return NULL;
        }

        ListNode dummy(0);
        ListNode* p = &dummy;

        while(l1 && l2) {
            if(l1->val < l2->val) {
                p->next = l1;
                l1 = l1->next;
            } else {
                p->next = l2;
                l2 = l2->next;
            }

            p = p->next;
        }

        if(l1) {
            p->next = l1;
        } else if(l2){
            p->next = l2;
        }

        return dummy.next;
    }
};
```

# Insertion Sort List

> Sort a linked list using insertion sort.

这题要求我们使用插入排序的方式对链表进行排序，假设一个链表前n个节点是有序，第n + 1的节点需要遍历前n个，插入到合适位置就可以了。

代码如下：

```c++
class Solution {
public:
     ListNode *insertionSortList(ListNode *head) {
        if(head == NULL || head->next == NULL) {
            return head;
        }

        ListNode dummy(0);

        ListNode* p = &dummy;

        ListNode* cur = head;
        while(cur) {
            p = &dummy;

            while(p->next && p->next->val <= cur->val) {
                p = p->next;
            }

            ListNode* n = p->next;
            p->next = cur;

            cur = cur->next;
            p->next->next = n;
        }

        return dummy.next;
    }
};
```
