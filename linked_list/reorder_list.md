# Reorder List

> Given a singly linked list L: L0→L1→…→Ln-1→Ln,

> reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

> You must do this in-place without altering the nodes' values.

> For example,

> Given {1,2,3,4}, reorder it to {1,4,2,3}.

这题比较简单，其实就是将链表的左右两边合并，只是合并的时候右半部分需要翻转一下。

主要有三步：

+ 快慢指针找到切分链表
+ 翻转右半部分
+ 依次合并

代码如下：

```c++
class Solution {
public:
    void reorderList(ListNode *head) {
        if(head == NULL || head->next == NULL) {
            return;
        }

        ListNode* fast = head;
        ListNode* slow = head;


        //快慢指针切分链表
        while(fast->next != NULL && fast->next->next != NULL){
            fast = fast->next->next;
            slow = slow->next;
        }

        fast = slow->next;
        slow->next = NULL;

        //翻转右半部分
        ListNode dummy(0);
        while(fast) {
            ListNode* n = dummy.next;
            dummy.next = fast;
            ListNode* nn = fast->next;
            fast->next = n;
            fast = nn;
        }

        slow = head;
        fast = dummy.next;

        //依次合并
        while(slow) {
            if(fast != NULL) {
                ListNode* n = slow->next;
                slow->next = fast;
                ListNode* nn = fast->next;
                fast->next = n;
                fast = nn;
                slow = n;
            } else {
                break;
            }
        }
    }
};
```
