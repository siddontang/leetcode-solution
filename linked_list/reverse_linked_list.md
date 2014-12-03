# Reverse Linked List II

> Reverse a linked list from position m to n. Do it in-place and in one-pass.

> For example:
> Given 1->2->3->4->5->NULL, m = 2 and n = 4,

> return 1->4->3->2->5->NULL.

> Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

这题要求我们翻转[m, n]区间之间的链表。对于链表翻转来说，几乎都是通用的做法，譬如`p1 -> p2 -> p3 -> p4`，如果我们要翻转p2和p3，其实就是将p3挂载到p1的后面，所以我们需要知道p2的前驱节点p1。伪代码如下：

```c++
    //保存p3
    n = p2->next;
    //将p3的next挂载到p2后面
    p2->next = p3->next;
    //将p3挂载到p1的后面
    p1->next = p3;
    //将p2挂载到p3得后面
    p3->next = p2;
```

对于上题，我们首先遍历得到第m - 1个node，也就是pm的前驱节点。然后依次遍历，处理挂载问题就可以了。

代码如下：

```c++
class Solution {
public:
    ListNode *reverseBetween(ListNode *head, int m, int n) {
        if(!head) {
            return head;
        }

        ListNode dummy(0);
        dummy.next = head;

        ListNode* p = &dummy;
        for(int i = 1; i < m; i++) {
            p = p->next;
        }

        //p此时就是pm的前驱节点
        ListNode* pm = p->next;

        for(int i = m; i < n; i++) {
            ListNode* n = pm->next;
            pm->next = n->next;
            n->next = p->next;
            p->next = n;
        }

        return dummy.next;
    }
};
```

# Reverse Nodes in k-Group

> Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

> If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

> You may not alter the values in the nodes, only nodes itself may be changed.

> Only constant memory is allowed.

> For example,
> Given this linked list: 1->2->3->4->5

> For k = 2, you should return: 2->1->4->3->5

> For k = 3, you should return: 3->2->1->4->5

这题要求我们按照每k个节点对其进行翻转，理解了链表如何翻转之后很容易处理，唯一需要注意的就是每次k个翻转之后，一定要知道最后一个节点，因为这个节点就是下组的前驱节点了。

```c++
ListNode *reverseKGroup(ListNode *head, int k) {
        if(k <= 1 || !head) {
            return head;
        }

        ListNode dummy(0);
        dummy.next = head;
        ListNode* p = &dummy;
        ListNode* prev = &dummy;

        while(p) {
            prev = p;
            for(int i = 0; i < k; i++){
                p = p->next;
                if(!p) {
                    //到这里已经不够k个没法翻转了
                    return dummy.next;
                }
            }

            p = reverse(prev, p->next);
        }

        return dummy.next;
    }

    ListNode* reverse(ListNode* prev, ListNode* end) {
        ListNode* p = prev->next;

        while(p->next != end) {
            ListNode* n = p->next;
            p->next = n->next;
            n->next = prev->next;
            prev->next = n;
        }

        //这里我们会返回最后一个节点，作为下一组的前驱节点
        return p;
    }
```
