[TOC]

# 二分算法

## 二分查找算法模版

**这个模版我还没有研究的十分透彻，还不知道它的实际使用场景，就是关于答案在左半区间还是右半区间。**

二分模板一共有两个，分别适用于不同情况。

算法思路：假设目标值在闭区间`[l, r]`中， 每次将区间长度缩小一半，当`l = r`时，我们就找到了目标值。

### 版本1

当我们将区间`[l, r]`划分成`[l, mid]`和`[mid + 1, r]`时，其更新操作是`r = mid`或者`l = mid + 1;`，计算`mid`时不需要加1。



https://www.acwing.com/blog/content/346/ 用这里的图更新下下面



**边界**的问题

70% 跟单调性有关

95% 存在两段性的性质

![image-20190714203110250](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-19-085137.jpg)

二分的流程：

1、确定二分的边界

2、编写二分的代码框架

3、设定一个check（性质）

4、判断一下区间如何更新

5、如果是模版二，算mid的时候要+1，向上取整



个人理解：只要左边界没有去掉中位数，那么选取右中位数；只要右边界没有去掉中位数，那么选取左中位数



## 题型

### Leetcode 33 搜索旋转排序数组

#### 题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组` [0,1,2,4,5,6,7]` 可能变为` [4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

示例 2:

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```



#### 代码

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty()) return -1;
        
        // 找到最小值
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] <= nums.back()) r = mid; //注意边界条件要为<=或者>=，否则就会忽略一些值
            else l = mid + 1;
        }
        
        if (target <= nums.back()) r = nums.size() - 1;
        else l = 0, r -- ;
        
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        
        if (nums[l] == target) return l;
        return -1;
    }
};
```

### [Leetcode 287寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number)

#### 题目描述

给定一个包含` n + 1 `个整数的数组 `nums`，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

示例 1:

```
输入: [1,3,4,2,2]
输出: 2
```

示例 2:

```
输入: [3,1,3,4,2]
输出: 3
```

说明：
1. 不能更改原数组（假设数组是只读的）。
2. 只能使用额外的 O(1) 的空间。
3. 时间复杂度小于 O(n2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

#### 代码

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        
       if (nums.empty()) return false;
       int l = 1, r = nums.size() - 1;
       while(l < r)
       {
           int mid = l + r >> 1;
           int count = 0;
           for(auto x:nums)
           {
               if(x >= l && x <= mid) count ++;
           }
            
           if(mid - l + 1 < count) r = mid;
           else l = mid + 1;        
       }        
       return r;
        
    }
};
```

注意l、mid、r的关系，注意判决条件是小于，不是小于等于。

### [Leetcode x的平方根](https://leetcode-cn.com/problems/sqrtx)

#### 题目描述

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

示例 1:

输入: 4
输出: 2

示例 2:

输入: 8
输出: 2

说明: 8 的平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。

#### 代码

```c++
class Solution {
public:
    int mySqrt(int x) {
       long long int l = 0, r = x;
       while( l < r)
       {
           long long int mid = l + r + 1>> 1;
           if (mid <= x/mid) l = mid;
           else r = mid - 1;
       }
        return r;
    }
};
```



### [Leetcode 35 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

#### 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例 1:

```
输入: [1,3,5,6], 5
输出: 2
```


示例 2:

```
输入: [1,3,5,6], 2
输出: 1
```


示例 3:

```
输入: [1,3,5,6], 7
输出: 4
```


示例 4:

```
输入: [1,3,5,6], 0
输出: 0
```

#### 代码

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if (nums.empty() || target > nums.back()) return nums.size();//注意边界条件
        int l = 0, r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        
        return r;

    }
};
```

### [LeetCode 74 Search a 2D Matrix](https://leetcode-cn.com/problems/search-a-2d-matrix)

#### 题目描述

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。
示例 1:

```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true
```


示例 2:

```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false
```

#### 代码

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if( matrix.empty() || matrix[0].empty()) return false;
        int n = matrix.size(), m = matrix[0].size();
        int l = 0, r = n * m - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if (matrix[mid/m][mid%m] >= target) r = mid;
            else l = mid + 1;
        }
        
        if (matrix[r/m][r%m] == target) return true;
        else return false;
```

###  [Leetcode34 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

#### 题目描述

给定一个按照升序排列的整数数组` nums`，和一个目标值` target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

示例 1:

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```


示例 2:

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

#### 代码

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
       if(nums.empty() || nums.back() < target) return {-1,-1};
       int l = 0, r = nums.size() - 1;
       int start, end;
       while(l < r)
       {
           int mid = l + r >> 1;
           if (nums[mid] >= target) r = mid;
           else l = mid + 1;
       }
       if (nums[r] == target) start = r;
       else return {-1,-1};
       l = 0, r = nums.size() - 1;
       while(l < r)
       {
           int mid = l + r + 1 >> 1;
           if (nums[mid] <= target) l = mid;
           else r = mid - 1;
       }
       if (nums[r] == target) end = r;
       else return {-1,-1};
       return vector<int>({start,end});
    }
};
```

