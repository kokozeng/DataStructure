
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

![image-20190725153153680](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073154.png)

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

![image-20190725153217467](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073218.png)

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

![image-20190725153340021](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073340.png)

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

![image-20190725153410676](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073535.jpg)

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

![image-20190725153437292](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073534.jpg)

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

![image-20190725153459495](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-73536.jpg)

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

![image-20190725153514235](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-25-073533.jpg)

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
      //边界条件
        if(m > n) return NULL;
        if(m == n) return head;
        
        auto dummy = new ListNode(-1);
        dummy->next = head;
        
      //定义a,b,c,d
        auto c = dummy, a = dummy;
        while(n --) c = c->next;
        auto d = c->next;
        m = m - 1;
        while(m --) a = a->next;
        auto b = a->next;
        
      //反转b到c
        auto i = b, j = i->next;
        while(i != c)
        {
            auto w = j->next;
            j->next = i;
            i = j, j = w;
        }
        
      //将反转的接回链表
        a->next = c;
        b->next = d;
        
        return dummy->next;
        
    }
};
```

### [LeetCode 160  相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/) 

####  题目描述

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表：

![image-20190728235258859](/Users/weijunzeng/Documents/Work/Code/image/image-20190728235258859.png)

在节点 c1 开始相交。

 

**示例 1：**

![image-20190728235310595](/Users/weijunzeng/Documents/Work/Code/image/image-20190728235310595.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

![image-20190728235352263](/Users/weijunzeng/Documents/Work/Code/image/image-20190728235352263.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

![image-20190728235416819](/Users/weijunzeng/Documents/Work/Code/image/image-20190728235416819.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

**注意：**

```
如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。
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

### [Leetcode142 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

#### 题目描述

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

**示例 1：**

![image-20190730112901597](/Users/weijunzeng/Documents/Work/Code/image/image-20190730112901597.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![image-20190730112916373](/Users/weijunzeng/Documents/Work/Code/image/image-20190730112916373.png)

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```



进阶：
你是否可以不用额外空间解决此题？

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
    ListNode *detectCycle(ListNode *head) {
        
        auto fast = head, slow = head;
        while(fast)
        {
            fast = fast->next;
            slow = slow->next;
            if (fast) fast = fast->next;
            else break;
            
            if (fast == slow)
            {
                slow = head;
                while (fast != slow)
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

