# Partition List

> Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

> You should preserve the original relative order of the nodes in each of the two partitions.

> For example,

> Given 1->4->3->2->5->2 and x = 3,

> return 1->2->2->4->3->5.

这题要求我们対链表进行切分，使得左半部分所有节点的值小于x，而右半部分大于等于x。

我们可以使用两个链表，p1和p2，以此遍历原链表，如果节点的值小于x，就挂载到p1下面，反之则放到p2下面，最后将p2挂载到p1下面就成了。

代码如下：

```c++
class Solution {
public:
    ListNode *partition(ListNode *head, int x) {
        ListNode dummy1(0), dummy2(0);
        ListNode* p1 = &dummy1;
        ListNode* p2 = &dummy2;

        ListNode* p = head;
        while(p) {
            if(p->val < x) {
                p1->next = p;
                p1 = p1->next;
            } else {
                p2->next = p;
                p2 = p2->next;
            }
            p = p->next;
        }

        p2->next = NULL;
        p1->next = dummy2.next;
        return dummy1.next;
    }
};
```
