# Linked List Cycle

> Given a linked list, determine if it has a cycle in it.

> Follow up:
> Can you solve it without using extra space?

这道题就是判断一个链表是否存在环，非常简单的一道题目，我们使用两个指针，一个每次走两步，一个每次走一步，如果一段时间之后这两个指针能重合，那么铁定存在环了。

代码如下：

```c++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == NULL || head->next == NULL) {
            return false;
        }

        ListNode* fast = head;
        ListNode* slow = head;

        while(fast->next != NULL && fast->next->next != NULL) {
            fast = fast->next->next;
            slow = slow->next;
            if(slow == fast) {
                return true;
            }
        }

        return false;
    }
};
```

# Linked List Cycle II

> Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

> Follow up:
> Can you solve it without using extra space?

紧跟着第一题，这题不光要求出是否有环，而且还需要得到这个环开始的节点。譬如下面这个，起点就是n2。

```
        n6-----------n5
        |            |
  n1--- n2---n3--- n4|

```

我们仍然可以使用两个指针fast和slow，fast走两步，slow走一步，判断是否有环，当有环重合之后，譬如上面在n5重合了，那么如何得到n2呢？

首先我们知道，fast每次比slow多走一步，所以重合的时候，fast移动的距离是slot的两倍，我们假设n1到n2距离为a，n2到n5距离为b，n5到n2距离为c，fast走动距离为`a + b + c + b`，而slow为`a + b`，有方程`a + b + c + b = 2 x (a + b)`，可以知道`a = c`，所以我们只需要在重合之后，一个指针从n1，而另一个指针从n5，都每次走一步，那么就可以在n2重合了。

代码如下：

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
         if(head == NULL || head->next == NULL) {
            return NULL;
        }

        ListNode* fast = head;
        ListNode* slow = head;

        while(fast->next != NULL && fast->next->next != NULL) {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow) {
                slow = head;
                while(slow != fast) {
                    fast = fast->next;
                    slow = slow->next;
                }
                return slow;
            }
        }

        return NULL;
    }
};
```

# Intersection of Two Linked Lists

> Write a program to find the node at which the intersection of two singly linked lists begins.


> For example, the following two linked lists:

>```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗
B:     b1 → b2 → b3
```
begin to intersect at node c1.


> Notes:

> + If the two linked lists have no intersection at all, return null.
> + The linked lists must retain their original structure after the function returns.
> + You may assume there are no cycles anywhere in the entire linked structure.
> + Your code should preferably run in O(n) time and use only O(1) memory.

这题需要得到两个链表的交接点，也就是c1，这一题也很简单。

+ 遍历A，到结尾之后，将A最后的节点连接到B的开头，也就是`c3 -> b1`
+ 使用两个指针fast和slow，从a1开始，判断是否有环
+ 如果没环，在返回之前记得将`c3 -> b1`给断开
+ 如果有环，则按照第二题的解法得到c1，然后断开`c3 -> b1`

代码如下：

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
               if(!headA) {
            return NULL;
        } else if (!headB) {
            return NULL;
        }

        ListNode* p = headA;
        while(p->next) {
            p = p->next;
        }

        //将两个链表串起来
        p->next = headB;

        ListNode* tail = p;
        p = headA;

        //fast和slow，判断是否有环
        headB = headA;
        while(headB->next && headB->next->next) {
            headA = headA->next;
            headB = headB->next->next;
            if(headA == headB) {
                break;
            }
        }

        if(!headA->next || !headB->next || !headB->next->next) {
            //没环，断开两个链表
            tail->next = NULL;
            return NULL;
        }

        //有环，得到环的起点
        headA = p;
        while(headA != headB) {
            headA = headA->next;
            headB = headB->next;
        }

        //断开两个链表
        tail->next = NULL;
        return headA;
    }
};
```
