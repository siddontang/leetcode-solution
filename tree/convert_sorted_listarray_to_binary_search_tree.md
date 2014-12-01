# Convert Sorted List to Binary Search Tree

> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

这题需要将一个排好序的链表转成一个平衡二叉树，我们知道，对于一个二叉树来说，左子树一定小于根节点，而右子树大于根节点。所以我们需要找到链表的中间节点，这个就是根节点，链表的左半部分就是左子树，而右半部分则是右子树，我们继续递归处理相应的左右部分，就能够构造出对应的二叉树了。

这题的难点在于如何找到链表的中间节点，我们可以通过fast，slow指针来解决，fast每次走两步，slow每次走一步，fast走到结尾，那么slow就是中间节点了。

代码如下：

```c++
class Solution {
public:

    TreeNode *sortedListToBST(ListNode *head) {
        return build(head, NULL);
    }

    TreeNode* build(ListNode* start, ListNode* end) {
        if(start == end) {
            return NULL;
        }

        ListNode* fast = start;
        ListNode* slow = start;

        while(fast != end && fast->next != end) {
            slow = slow->next;
            fast = fast->next->next;
        }

        TreeNode* node = new TreeNode(slow->val);
        node->left = build(start, slow);
        node->right = build(slow->next, end);

        return node;
    }
};
```

# Convert Sorted Array to Binary Search Tree

> Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

这题类似上面那题，同样地解题方式，对于数组来说，能更方便的得到中间节点，代码如下：

```c++
class Solution {
public:
    TreeNode *sortedArrayToBST(vector<int> &num) {
        return build(num, 0, num.size());
    }

    TreeNode* build(vector<int>& num, int start, int end) {
        if(start >= end) {
            return NULL;
        }

        int mid = start + (end - start) / 2;

        TreeNode* node = new TreeNode(num[mid]);
        node->left = build(num, start, mid);
        node->right = build(num, mid + 1, end);

        return node;
    }
};
```
