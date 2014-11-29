# Remove Duplicates from Sorted List

> Given a sorted linked list, delete all duplicates such that each element appear only once.

> For example,

> Given 1->1->2, return 1->2.

> Given 1->1->2->3->3, return 1->2->3.

这题要求在一个有序的链表里面删除重复的元素，只保留一个，也是比较简单的一个题目，我们只需要判断当前指针以及下一个指针时候重复，如果是，则删除下一个指针就可以了。

代码如下:

```c++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
                if(!head) {
            return head;
        }

        int val = head->val;
        ListNode* p = head;
        while(p && p->next) {
            if(p->next->val != val) {
                val = p->next->val;
                p = p->next;
            } else {
                //删除next
                ListNode* n = p->next->next;
                p->next = n;
            }
        }

        return head;
    }
};
```

# Remove Duplicates from Sorted List II

> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

> For example,

> Given 1->2->3->3->4->4->5, return 1->2->5.

> Given 1->1->1->2->3, return 2->3.

这题需要在一个有序的链表里面删除所有的重复元素的节点。因为不同于上题可以保留一个，这次需要全部删除，所以我们遍历的时候需要记录一个prev节点，用来处理删除节点之后节点重新连接的问题。

代码如下：

```c++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
                if(!head) {
            return head;
        }

        //用一个dummy节点来当做head的prev
        ListNode dummy(0);
        dummy.next = head;
        ListNode* prev = &dummy;
        ListNode* p = head;
        while(p && p->next) {
            //如果没有重复，则prev为p，next为p->next
            if(p->val != p->next->val) {
                prev = p;
                p = p->next;
            } else {
                //如果有重复，则继续遍历，直到不重复的节点
                int val = p->val;
                ListNode* n = p->next->next;
                while(n) {
                    if(n->val != val) {
                        break;
                    }
                    n = n->next;
                }

                //删除重复节点
                prev->next = n;
                p = n;
            }
        }
        return dummy.next;
    }
};
```
