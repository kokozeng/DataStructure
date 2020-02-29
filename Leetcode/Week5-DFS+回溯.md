[TOC]

宽搜：求最短步数、最小距离

深搜：状态数量非常庞大，求数独的某一组解

搜索 != 递归

# DFS+回溯

##  [Leetcode17 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

### 题目描述

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![image-20190822201941750](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045352.jpg)

示例:

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

### 题解

循环dfs

state{""}

for 每个数字

 	 for c = 当前数字的所有备选字母

​			for s = state 中的所有字符串

​				s += c

​				将s加入到新的集合中去

![image-20190822202657591](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045349.jpg)

 ### 代码

```c++
class Solution {
public:

    vector<string> letterCombinations(string digits) {

        if (digits.empty()) return vector<string>();

        string chars[8] = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

        vector<string> res(1, "");//创建一个包含一个值为“”的vector
        for (auto u : digits)
        {
            vector<string> now;
            for (char c : chars[u - '2'])
                for (auto path : res)
                    now.push_back(path + c);
            res = now;
        }
        return res;
    }
};
//循环如何定义，出了一点点问题
```

## [Leetcode79 单词搜索](https://leetcode-cn.com/problems/word-search/)

### 题目描述

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

```
示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true.
给定 word = "SEE", 返回 true.
给定 word = "ABCB", 返回 false.
```

### 题解

顺序！

1、枚举起点

2、从起点开始，依次搜索下一个点的位置

3、在枚举的过程中，要保证和目标单词匹配



nm (起点) * 3 ^ k // 每次只有三种选择，k是字符串的长度

矩阵或者棋盘的题目：数据范围比较小用搜索，数据范围比较大用动态规划。



### 代码

```c++
class Solution {
public:

    int n, m;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty() || board[0].empty()) return false;

        n = board.size(), m = board[0].size();
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (dfs(board, i, j, word, 0))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>>& board, int x, int y, string &word, int u)
    {
        if (board[x][y] != word[u]) return false;
        if (u == word.size() - 1) return true;

        board[x][y] = '.';  // 因为在算下一个的时候，不能算当前的值
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m)
            {  
                if (dfs(board, a, b, word, u + 1)) return true;
            }
        }
        board[x][y] = word[u]; // 回溯，恢复初始状态,x,y,u的初始值不能更改
        return false;
    }
};
```

## [Leetcode46 全排列](https://leetcode-cn.com/problems/permutations/)

### 题目描述

给定一个没有重复数字的序列，返回其所有可能的全排列。

```
示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

### 题解

1、搜索的顺序问题

2.1、枚举每一个位置上该放哪个数

![image-20190823103142590](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045350.jpg)

2.2、枚举每个数放到哪个位置上

![image-20190823103435305](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-45351.jpg)

### 代码

```c++
class Solution {
public:
		int n;
  	vector<bool> st; //当前分支可以用的数字是哪一个 
    vector<vector<int>> ans; //存所有方案
    vector<int> path; //存当前方案

    vector<vector<int>> permute(vector<int>& nums) {
        n = nums.size();// 数字的个数
        st = vector<bool>(n); //状态
       // for (int i = 0; i < nums.size(); i ++ ) st.push_back(false);
        dfs (nums, 0);
        return ans;
    }

    void dfs(vector<int>& nums, int u)
    {
        if (u == n)
        {
            ans.push_back(path);
            return;
        }

        for (int i = 0; i < n; i ++ ) // 枚举当前状态可以填哪个数
            if (!st[i])
            {
                st[i] = true;//填了数
                path.push_back(nums[i]);
                dfs(nums, u + 1 );//枚举到哪一个位置 u
                path.pop_back();//删除最后一个元素
                st[i] = false;
            }
    }
};

```

##  [Leetcode47 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

### 题目描述

给定一个可包含重复数字的序列，返回所有不重复的全排列。

```
示例:

输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

### 题解

1、枚举每个数放到哪个位置上

2、将所有相同数放在一起，sort

3、人为规定相同数字的相对顺序：不变

4、dfs(u, start)

### 代码

![image-20190823113639647](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045351.jpg)

