# Plus One Linked List

> Given a non-negative integer represented as non-empty a singly linked list of digits, plus one to the integer.

> You may assume the integer do not contain any leading zero, except the number 0 itself.

> The digits are stored such that the most significant digit is at the head of the list.

> Given 1->2->3, 

> return 1->2->4.

此题计算方法与数组分类中的[Plus One](../array/plus_one.md)完全一样。需要注意两点：

 + 单向链表没有办法像数组一样轻易从低位往高位操作，因此需要借助递归完成；
 + 可使用指针传递进位。

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
    ListNode* plusOne(ListNode* head) {
        bool* carry = new bool(false);
        recur(head, carry);
        
        if(!(*carry)) {
            return head;
        }
        else{
            ListNode* newHead = new ListNode(1);
            newHead->next = head;
            return newHead;
        }
        

    }
    
    ListNode* recur(ListNode* head, bool* carry) {
        if(head->next != nullptr) {recur(head->next, carry);}
        
        if(head->next == nullptr || *carry){
            head->val++;
            if(head->val >= 10) {
                head->val = 0;
                *carry=true;
            }
            else{
                 *carry = false;
            }
        }       
        
        return head;
    }
};
```