[TOC]

边界节点的处理

上一个节点和下一个节点

# 链表

## 题型

###  [Leetcode19  删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

#### 题目描述

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 n 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？



#### 题解

![image-20190721201606628](/Users/weijunzeng/Documents/Work/Code/image/image-20190721201606628.png)

#### 代码

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        auto first = dummy, second = dummy;
        while (n -- ) first = first->next;
        while (first->next)
        {
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        
        return dummy->next;
    }
};
```

### [Leetcode237  删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

#### 题目描述

编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。

现有一个链表 -- head = [4,5,1,9]，它可以表示为:

![image-20190722121216726](/Users/weijunzeng/Documents/Work/Code/image/image-20190722121216726.png)

 

**示例 1:**

```
输入: head = [4,5,1,9], node = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

**示例 2:**

```
输入: head = [4,5,1,9], node = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

**说明:**

```
链表至少包含两个节点。
链表中所有节点的值都是唯一的。
给定的节点为非末尾节点并且一定是链表中的一个有效节点。
不要从你的函数中返回任何结果。
```

#### 题解

把当前的点伪装成下一个点，然后把下一个点删掉。

![image-20190721202702123](/Users/weijunzeng/Documents/Work/Code/image/image-20190721202702123.png)

#### 代码

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
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
      //  *(node) = *(node->next);   //一行代码表示方法
    }
};
```

### [Leetcode83 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

#### 题目描述

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

示例 1:

```
输入: 1->1->2
输出: 1->2
```


示例 2:

```
输入: 1->1->2->3->3
输出: 1->2->3
```

#### 代码

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
    ListNode* deleteDuplicates(ListNode* head) {
        auto cur = head;
        while (cur)
        {
            if (cur->next && cur->next->val == cur->val) //注意边界节点
                cur->next = cur->next->next;
            else cur = cur->next;
        }
        return head;
    }
};
```

Leetcode 82也做一下

### [Leetcode61 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

#### 题目描述

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```


示例 2:

```
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```

#### 题解
![image-20190721204643443](/Users/weijunzeng/Documents/Work/Code/image/image-20190721204643443.png)

#### 大佬代码

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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return NULL;
        
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        
        k %= n;
        auto first = head, second = head;
        while (k -- ) first = first->next;
        while (first->next)
        {
            first = first->next;
            second = second->next;
        }
        
        first->next = head;
        head = second->next;
        second->next = NULL;
        
        return head;
    }
};
```

#### 我的代码

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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return NULL; //注意边界判断
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        auto cur = head;
        int n = 0;
        while(cur) 
        {
            n++;
            cur = cur->next;
        }
       
        k %= n;
        
        auto first = dummy, second = dummy;
        while(k--) first = first->next;
        
        while(first->next)
        {
            first = first->next;
            second = second->next;
        }
        
        first->next = head;
        dummy->next = second->next; //注意这一行代码
        second->next = NULL;
        
        return dummy->next;
    }
};
```

### [Leetcode24 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

#### 题目描述

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

#### 题解

![image-20190721205431827](/Users/weijunzeng/Documents/Work/Code/image/image-20190721205431827.png)

p->next = b;

a->next = b->next;

b->next = a;

p = a;

#### 大佬代码

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
    ListNode* swapPairs(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        for (auto p = dummy; p->next && p->next->next;)
        {
            auto a = p->next, b = a->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
            p = a;
        }
        
        return dummy->next;
    }
};
```
#### 我的代码

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
    ListNode* swapPairs(ListNode* head) {
        if(!head) return NULL;
        
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        auto p = dummy;
        
        while(p->next && p->next->next) //注意边界节点的判断
            
        {
            auto a = p->next, b = p->next->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
            p = a;
        
        }
        
        return dummy->next;
        
    }
};
```

###   [Leetcode206 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

#### 题目描述

反转一个单链表。

示例:

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

#### 题解



![image-20190721210510554](/Users/weijunzeng/Documents/Work/Code/image/image-20190721210510554.png)

#### 代码

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
    ListNode* reverseList(ListNode* head) {
        if (!head) return NULL;
        
        auto a = head, b = head->next;
        while (b)
        {
            auto c = b->next;
            b->next = a;
            a = b, b = c; //注意这里是有次序的
        }
        
        head->next = NULL; 
        
        return a;
    }
};
```



### [Leetcode92 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

#### 题目描述

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

示例:

```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

#### 题解

![image-20190725152312305](/Users/weijunzeng/Documents/Work/Code/image/image-20190725152312305.png)

#### 大佬代码

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (m == n) return head;
        
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        auto a = dummy, d = dummy;
        for (int i = 0; i < m - 1; i ++ ) a = a->next;
        for (int i = 0; i < n; i ++ ) d = d->next;
        
        auto b = a->next, c = d->next;
        
        for (auto p = b, q = b->next; q != c;)
        {
            auto o = q->next;
            q->next = p;
            p = q, q = o;
        }
        
        b->next = c;
        a->next = d;
        
        return dummy->next;
    }
};
```

#### 我的代码

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if(m > n) return NULL;
        if(m == n) return head;
        
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        auto c = dummy, a = dummy;
        while(n --) c = c->next;
        auto d = c->next;
        m = m - 1;
        while(m --) a = a->next;
        auto b = a->next;
        
        auto i = b, j = i->next;
        while(i != c)
        {
            auto w = j->next;
            j->next = i;
            i = j, j = w;
        }
        
        a->next = c;
        b->next = d;
        
        return dummy->next;
        
    }
};
```

8、

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        auto p = headA, q = headB;
        while (p != q)
        {
            if (p) p = p->next;
            else p = headB;
            if (q) q = q->next;
            else q = headA;
        }
        
        return p;
    }
};
```

9、

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
    ListNode *detectCycle(ListNode *head) {
        auto fast = head, slow = head;
        while (slow)
        {
            fast = fast->next;
            slow = slow->next;
            if (slow) slow = slow->next;
            else break;
            
            if (fast == slow)
            {
                slow = head;
                while (slow != fast)
                {
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

重看一遍

10、

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
    ListNode* sortList(ListNode* head) {
        
        int n = 0;
        for (auto p = head; p; p = p->next) n ++ ;
        
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
        for (int i = 1; i < n; i *= 2)
        {
            auto cur = dummy;
            for (int j = 0; j + i < n; j += i * 2)
            {
                auto left = cur->next, right = cur->next;
                for (int k = 0; k < i; k ++ ) right = right->next;
                int l = 0, r = 0;
                while (l < i && r < i && right)
                    if (left->val <= right->val)
                    {
                        cur->next = left;
                        cur = left;
                        left = left->next;
                        l ++ ;
                    }
                    else
                    {
                        cur->next = right;
                        cur = right;
                        right = right->next;
                        r ++ ;
                    }
                while (l < i)
                {
                    cur->next = left;
                    cur = left;
                    left = left->next;
                    l ++ ;
                }
                while (r < i && right)
                {
                    cur->next = right;
                    cur = right;
                    right = right->next;
                    r ++ ;
                }
                
                cur->next = right;
            }
        }
        
        return dummy->next;
    }
};
```

