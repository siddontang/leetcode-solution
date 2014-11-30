# Merge Two Sorted Lists

> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

这题要求合并两个已经排好序的链表，很简单的题目，直接上代码：

```c++
class Solution {
public:
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNode dummy(0);
        ListNode* p = &dummy;

        while(l1 && l2) {
            int val1 = l1->val;
            int val2 = l2->val;
            //哪个节点小，就挂载，同时移动到下一个节点
            if(val1 < val2) {
                p->next = l1;
                p = l1;
                l1 = l1->next;
            } else {
                p->next = l2;
                p = l2;
                l2 = l2->next;
            }
        }

        //这里处理还未挂载的节点
        if(l1) {
            p->next = l1;
        } else if(l2) {
            p->next = l2;
        }

        return dummy.next;
    }
};
```

# Merge k Sorted Lists

> Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

这题需要合并k个排好序的链表，我们采用`divide and conquer`的方法，首先两两合并，然后再将先前合并的继续两两合并。时间复杂度为O(NlgN)。

代码如下：

```c++
class Solution {
public:
     ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.empty()) {
            return NULL;
        }

        int n = lists.size();
        while(n > 1) {
            int k = (n + 1) / 2;
            for(int i = 0; i < n / 2; i++) {
                //合并i和i + k的链表，并放到i位置
                lists[i] = merge2List(lists[i], lists[i + k]);
            }
            //下个循环只需要处理前k个链表了
            n = k;
        }
        return lists[0];
    }

    ListNode* merge2List(ListNode* n1, ListNode* n2) {
        ListNode dummy(0);
        ListNode* p = &dummy;
        while(n1 && n2) {
            if(n1->val < n2->val) {
                p->next = n1;
                n1 = n1->next;
            } else {
                p->next = n2;
                n2 = n2->next;
            }
            p = p->next;
        }

        if(n1) {
            p->next = n1;
        } else if(n2) {
            p->next = n2;
        }

        return dummy.next;
    }
};
```
