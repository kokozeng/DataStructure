[TOC]



# 动态规划-线性DP-区间DP

## 线性DP

有一个明显的顺序，它在各个维度符合线性增长的特点。

### 数字三角形

#### 题目描述

给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。

```
        7
      3   8
    8   1   0
  2   7   4   4
4   5   2   6   5
```

**输入格式**

第一行包含整数n，表示数字三角形的层数。

接下来n行，每行包含若干整数，其中第 i 行表示数字三角形第 i 层包含的整数。

**输出格式**

输出一个整数，表示最大的路径数字和。

**数据范围**
$$
1≤n≤500,
−10000≤三角形中的整数≤10000
$$



**输入样例：**

```
5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5
```

**输出样例：**

```
30
```

#### 状态转移思路

![image-20190719233254999](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-81157.jpg)

#### 代码	f

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, INF = 1e9;

int n;
int a[N][N], f[N][N];

int main()
{
    scanf("%d", &n);
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= i; j++)
            scanf("%d", &a[i][j]);
    
  //初始化的过程要注意
    for(int i = 0;  i <= n; i++ )
        for(int j = 0; j <= i + 1; j++)
            f[i][j] = -INF;
   //还要定义f[1][1]
    f[1][1] = a[1][1];
    for(int i = 2; i <= n; i++)
        for(int j = 1; j <= i; j++)
                f[i][j] = max(f[i - 1][j - 1], f[i - 1][j]) + a[i][j];
            
    int res = -INF;
    for(int i = 1; i <= n; i++) res = max(res, f[n][i]);
    
    printf("%d\n", res);
    return 0;
}
```



### 最长上升子序列问题

#### 题目描述

给定一个长度为N的数列，求数值严格单调递增的子序列的长度最长是多少。

**输入格式**

第一行包含整数N。

第二行包含N个整数，表示完整序列。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

$$1≤N≤1000$$，
$$−10^9≤$$数列中的数$$≤10^9$$

**输入样例：**

```
7
3 1 2 1 8 5 6
```

**输出样例：**

```
4
```

#### 状态转移

![image-20190719225034740](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081156.jpg)
$$
f_i = (max(f[j] + 1)|f_j < f_i, j = 0, ..., i-1)
$$


#### 代码

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010, INF = 1e9;

int n;
int a[N], f[N];

int main()
{
    scanf("%d", &n);
    for(int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    
    for(int i = 1; i <= n; i++)
    {
        f[i] = 1; //(只有a[i]一个数的情况)
        for(int j = 1; j < i; j++)
        {
            //f[i] = f[i - 1]; //在这里定义就会出问题，如果a[i]为最小值，它不可能能等于前一个数的公共子序列
            if(a[i] > a[j]) f[i] = max (f[i], f[j] + 1); //有很多个f[j], 我们要取一个最大的
                                                        //是在前i-1个f[j]中找出他们的最大的然后+1
        }
    }
    int res = 0;
    for (int i = 1; i <= n; i++) res = max(res, f[i]);
    printf("%d\n", res);
    
    return 0;
}
```



#### 把最长子序列输出

把转移记录下来

![image-20190719231102032](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-81151.jpg)

### 最长公共子序列问题

#### 题目描述

给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少。

**输入格式**

第一行包含两个整数N和M。

第二行包含一个长度为N的字符串，表示字符串A。

第三行包含一个长度为M的字符串，表示字符串B。

字符串均由小写字母构成。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

$$1≤N≤1000，$$

**输入样例：**

```
4 5
acbd
abedc
```

**输出样例：**

```
3
```

#### 状态转移

两个序列所以用两维来表示

![image-20190719235237758](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081151.jpg)

#### 代码

```c++
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 1010;

int n, m;
int f[N][N];
char a[N], b[N];

int main()
{
    scanf("%d%d", &n, &m);
    scanf("%s%s", a + 1, b + 1);
    
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++)
        {
            if(a[i] == b[j]) f[i][j] = max(f[i][j], f[i-1][j-1] + 1);
            else f[i][j] = max(f[i-1][j], f[i][j-1]);
        }
        
    printf("%d", f[n][m]);
}
```

为什么int a, b报错的解释：
int 是四个字节，后面的全为0。
![image-20190720150110237](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081153.jpg)

## 区间DP

### 石子合并

#### 题目描述

设有N堆石子排成一排，其编号为1，2，3，…，N。

每堆石子有一定的质量，可以用一个整数来描述，现在要将这N堆石子合并成为一堆。

每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。

例如有4堆石子分别为 1 3 5 2， 我们可以先合并1、2堆，代价为4，得到4 5 2， 又合并 1，2堆，代价为9，得到9 2 ，再合并得到11，总代价为4+9+11=24；

如果第二步是先合并2，3堆，则代价为7，得到4 7，最后一次合并代价为11，总代价为4+7+11=22。

问题是：找出一种合理的方法，使总的代价最小，输出最小代价。

**输入格式**

第一行一个数N表示石子的堆数N。

第二行N个数，表示每堆石子的质量(均不超过1000)。

**输出格式**

输出一个整数，表示最小代价。

**数据范围**
$$
1≤N≤300
$$



**输入样例：**

```
4
1 3 5 2
```

**输出样例：**

```
22
```

#### 状态转移思路

以最后一次分界线的位置来分类：

![image-20190720151554836](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081150.jpg)

![image-20190720151947463](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081155.jpg)

![image-20190720152012674](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081147.jpg)

#### 代码

![image-20190720152933473](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2019-07-20-081148.jpg)

先循环区间长度，然后循环区间的左端点，再枚举决策。