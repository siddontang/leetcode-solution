# Copy List with Random Pointer

> A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

> Return a deep copy of the list.

这题要求深拷贝一个带有random指针的链表random可能指向空，也可能指向链表中的任意一个节点。

对于通常的链表，我们递归依次拷贝就可以了，同时用一个hash表记录新旧节点的映射关系用以处理random问题。

代码如下：

```c++
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if(head == NULL) {
            return NULL;
        }

        RandomListNode dummy(0);
        RandomListNode* n = &dummy;
        RandomListNode* h = head;
        map<RandomListNode*, RandomListNode*> m;
        while(h) {
            RandomListNode* node = new RandomListNode(h->label);
            n->next = node;
            n = node;

            node->random = h->random;

            m[h] = node;

            h = h->next;
        }

        h = dummy.next;
        while(h) {
            if(h->random != NULL) {
                h->random = m[h->random];
            }
            h = h->next;
        }

        return dummy.next;
    }
};
```

但这题其实还有更巧妙的作法。假设有如下链表：

```
|------------|
|            v
1  --> 2 --> 3 --> 4

```

节点1的random指向了3。首先我们可以通过next遍历链表，依次拷贝节点，并将其添加到原节点后面，如下：

```
|--------------------------|
|                          v
1  --> 1' --> 2 --> 2' --> 3 --> 3' --> 4 --> 4'
       |                   ^
       |-------------------|
```

因为我们只是简单的复制了random指针，所以新的节点的random指向的仍然是老的节点，譬如上面的1和1'都是指向的3。

调整新的节点的random指针，对于上面例子来说，我们需要将1'的random指向3'，其实也就是原先random指针的next节点。

```
|--------------------------|
|                          v
1  --> 1' --> 2 --> 2' --> 3 --> 3' --> 4 --> 4'
       |                         ^
       |-------------------------|
```

最后，拆分链表，就可以得到深拷贝的链表了。

代码如下：

```c++
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if(head == NULL) {
            return NULL;
        }

        //遍历并插入新的节点
        RandomListNode* n = NULL;
        RandomListNode* h = head;
        while(h) {
            RandomListNode* node = new RandomListNode(h->label);
            node->random = h->random;

            n = h->next;

            h->next = node;
            node->next = n;
            h = n;
        }

        //调整random
        h = head->next;
        while(h) {
            if(h->random != NULL) {
                h->random = h->random->next;
            }
            if(!h->next) {
                break;
            }
            h = h->next->next;
        }

        //断开链表
        h = head;
        RandomListNode dummy(0);
        RandomListNode* p = &dummy;
        while(h) {
            n = h->next;
            p->next = n;
            p = n;
            RandomListNode* nn = n->next;
            h->next = n->next;
            h = n->next;
        }

        return dummy.next;
    }
};
```
