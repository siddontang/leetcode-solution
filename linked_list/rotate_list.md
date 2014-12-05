# Rotate List

> Given a list, rotate the list to the right by k places, where k is non-negative.

> For example:

> Given 1->2->3->4->5->NULL and k = 2,

> return 4->5->1->2->3->NULL.

这题要求把链表后面k个节点轮转到链表前面。

对于这题我们首先需要遍历链表，得到链表长度n，因为k可能大于n，所以我们需要取余处理，然后将链表串起来形成一个换，在遍历`n - k % n`个节点，断开，就成了。譬如上面这个例子，k等于2，我们遍历到链表结尾之后，连接1，然后遍历 `5 - 2 % 5`个字节，断开环，下一个节点就是新的链表头了。

代码如下：

```c++
class Solution {
public:
    ListNode *rotateRight(ListNode *head, int k) {
        if(!head || k == 0) {
            return head;
        }

        int n = 1;
        ListNode* p = head;
        //得到链表长度
        while(p->next) {
            p = p->next;
            n++;
        }

        k = n - k % n;

        //连接成环
        p->next = head;

        for(int i = 0; i < k; i++) {
            p = p->next;
        }

        //得到新的链表头并断开环
        head = p->next;
        p->next = NULL;
        return head;
    }
};
```
