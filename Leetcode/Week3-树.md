[TOC]

# 树

## 二叉搜索树

- 左子树的所有节点的值都小于当前节点的值；
- 右子树的所有节点的值都大于当前节点的值；
- 左子树和右子树都必须是合法的二叉搜索树；

中序遍历



## 例题

### LeetCode 98. Validate Binary Search Tree

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
        
        return dfs(root->left, minv, root->val - 1ll) && dfs(root->right, root->val + 1ll, maxv);// 1LL是 long long 的 1
    }
};
```

### LeetCode 94. Binary Tree Inorder Traversal

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

### LeetCode 101. Symmetric Tree

右边：右中左遍历



![image-20190728204323183](/Users/weijunzeng/Documents/Work/Code/image/image-20190728204323183.png)

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

