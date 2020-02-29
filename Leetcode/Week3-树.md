[TOC]

# 树

## 二叉搜索树

- 左子树的所有节点的值都小于当前节点的值；
- 右子树的所有节点的值都大于当前节点的值；
- 左子树和右子树都必须是合法的二叉搜索树；

中序遍历



## 例题

### [LeetCode 98. Validate Binary Search Tree](https://leetcode-cn.com/problems/validate-binary-search-tree)

#### 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

#### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return dfs(root, INT_MIN, INT_MAX);
    }
    
    bool dfs(TreeNode *root, long long minv, long long maxv)
    {
        if (!root) return true;
        if (root->val < minv || root->val > maxv) return false;
        
        return dfs(root->left, minv, root->val - 1ll) && dfs(root->right, root->val + 1ll, maxv);// 1LL是 long long 的 1,注意判断条件
    }
};
```

### [LeetCode 94. Binary Tree Inorder Traversal](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

#### 题解

![image-20190802115453816](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045309.jpg)

#### 代码

```c++
/**递归
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
        
    }
    
    void traversal(TreeNode * root, vector<int> &res)
    {
        if(!root) return;
        traversal(root->left, res);
        res.push_back(root->val);
        traversal(root->right, res);
    }
};
```



```c++
/**迭代
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        
        auto p = root;
        while (p || stk.size())
        {
            while (p)
            {
                stk.push(p);
                p = p->left;
            }
            
            p = stk.top();
            stk.pop();
            res.push_back(p->val);
            p = p->right;
        }
        
        return res;
    }
};
```

### [LeetCode 101. Symmetric Tree](https://leetcode-cn.com/problems/symmetric-tree)

#### 题目描述

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```

说明:
如果你可以运用递归和迭代两种方法解决这个问题，会很加分。

#### 题解



![image-20190728204323183](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045307.jpg)

#### 代码

```c++
/**递归
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return dfs(root->left, root->right);
    }
    
    bool dfs(TreeNode* p, TreeNode* q)
    {
        if (!p || !q) return !p && !q;
        
        return p->val == q->val && dfs(p->left, q->right) && dfs(p->right, q->left);
    }
};
```

```c++
/**迭代
左边：左中右的遍历
右边：右中左的遍历
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        stack<TreeNode*>left, right;
        auto l = root->left, r = root->right;
        while (l || r || left.size() || right.size())
        {
            while (l && r)
            {
                left.push(l), l = l->left;
                right.push(r), r = r->right;
            }
            
            if (l || r) return false;
            
            l = left.top(), left.pop();
            r = right.top(), right.pop();
            if (l->val != r->val) return false;
            
            l = l->right, r = r->left;
        }
        
        return true;
    }
};
```


### [Leetcode105 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

#### 题目描述

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

#### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    unordered_map<int,int> pos;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        for (int i = 0; i < n; i ++ )
            pos[inorder[i]] = i;
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    TreeNode* dfs(vector<int>&pre, vector<int>&in, int pl, int pr, int il, int ir)
    {
        if (pl > pr) return NULL;// don't forget it.
        int k = pos[pre[pl]] - il;
        TreeNode* root = new TreeNode(pre[pl]);
        root->left = dfs(pre, in, pl + 1, pl + k, il, il + k - 1);
        root->right = dfs(pre, in, pl + k + 1, pr, il + k + 1, ir);
        return root;
    }
};
```

### [Leetcode102 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

#### 题目描述

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

例如:
给定二叉树: [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```


#### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        
        vector<vector<int>> res;
        if(!root) return res;
        
        queue<TreeNode*> q;        
        q.push(root);
        
        while(q.size())
        {
            int len = q.size();
            vector<int> level;
            for(int i = 0; i < len; i++)
            {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }
            res.push_back(level);
        }
        
        return res;
        
    }
};
```

###  [Leetcode236 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045310.jpg)

 

示例 1:

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```


示例 2:

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```

#### 题解

![image-20190802153032987](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045308.jpg) 

#### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;
        
        auto left = lowestCommonAncestor(root->left, p, q);
        auto right = lowestCommonAncestor(root->right, p, q);
        
        if (!left) return right;
        if (!right) return left;
        return root;
        
    }
};
```

### LeetCode 543. Diameter of Binary Tree

![image-20190813193218039](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045311.jpg)

```c++
class Solution {
public:

    int ans = 0;

    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root)
    {
        if (!root) return 0;
        auto l = dfs(root->left);
        auto r = dfs(root->right);
        ans = max(ans, l + r);
        return max(l, r) + 1;
    }
};
```

