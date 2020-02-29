[TOC]

# 字符串

## [Leetcode38 报数](https://leetcode-cn.com/problems/count-and-say/)

### 题目描述

报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
1 被读作  "one 1"  ("一个一") , 即 11。
11 被读作 "two 1s" ("两个一"）, 即 21。
21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。

给定一个正整数 n（1 ≤ n ≤ 30），输出报数序列的第 n 项。

注意：整数顺序将表示为一个字符串。



### 代码

```c++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for(int i = 1; i < n ; i ++) //注意这个循环
        {
            string ns;
            for (int j = 0, k = 0; j < s.size(); j ++)
            {
                while(k < s.size() && s[k] == s[j]) k ++;
                ns += to_string(k - j) + s[j];
                j = k - 1; //为什么j要更新成k - 1，因为此循环结束后j ++；
            }
            s = ns;
        }
        return s;
        
    }
};
```

## [Leetcode49 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

### 题目描述

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

### 题解

思路：排序+哈希

时间复杂度：nmlog(m) + nm

1、找到乱序字符串的本质

2、通过哈希表分到一组

### 代码

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> res;
        unordered_map<string, vector<string>> hash;
        for(auto s : strs)
        {
            string key = s;
            sort(key.begin(), key.end());//排序是直接对本身进行排序
            hash[key].push_back(s);//在hash表里对某个key加元素的写法
        }
        for(auto item : hash)
        {
            res.push_back(item.second);//每个hash表的单元，second就表示value，first表示key
        }
        return res;
    }
};
```

把哈希表变成map效率会变低，哈希表是o(1)；

map是一个平衡树，时间复杂度logn；

## [Leetcode151 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

### 题目描述

给定一个字符串，逐个翻转字符串中的每个单词。

**示例 1：**

```
输入: "the sky is blue"
输出: "blue is sky the"
```

**示例 2：**

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

**说明：**

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

### 代码

第一步：反转每个单词

第二步：整体反转

```c++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for(int i = 0; i < s.size(); i ++)
        {
           // 删除前面的空格
            while(i < s.size() && s[i] == ' ') i ++; //从某点找到连续的一段都用这种方法，很经典
            if(i == s.size()) break;
            int j = i;
            while(j < s.size() && s[j] != ' ') j ++;
            reverse(s.begin() + i, s.begin() + j);//左闭右开 [i,j)
            if(k) s[k ++] = ' ';
            while(i < j) s[k ++] = s[i ++]; //在原字符操作，把反转后的赋值过去       
        }
        s.erase(s.begin() + k, s.end());//删除字符
        reverse(s.begin(), s.end());
        return s;
    }
};
```

[string.erase的使用](https://www.cnblogs.com/liyazhou/archive/2010/02/07/1665421.html)

## [Leetcode165 比较版本号](https://leetcode-cn.com/problems/compare-version-numbers/)

### 题目描述

比较两个版本号 version1 和 version2。
如果 `version1 > version2` 返回 1，如果 `version1 < version2` 返回 -1， 除此之外返回 0。

你可以假设版本字符串非空，并且只包含数字和 . 字符。

 `.` 字符不代表小数点，而是用于分隔数字序列。

例如，`2.5` 不是“两个半”，也不是“差一半到三”，而是第二版中的第五个小版本。

你可以假设版本号的每一级的默认修订版号为 `0`。例如，版本号 `3.4` 的第一级（大版本）和第二级（小版本）修订号分别为` 3 `和 `4`。其第三级和第四级修订号均为 0。

**示例 1:**

```
输入: version1 = "0.1", version2 = "1.1"
输出: -1
```

**示例 2:**

```
输入: version1 = "1.0.1", version2 = "1"
输出: 1
```

**示例 3:**

```
输入: version1 = "7.5.2.4", version2 = "7.5.3"
输出: -1
```

**示例 4：**

```
输入：version1 = "1.01", version2 = "1.001"
输出：0
解释：忽略前导零，“01” 和 “001” 表示相同的数字 “1”。
```

**示例 5：**

```
输入：version1 = "1.0", version2 = "1.0.0"
输出：0
解释：version1 没有第三级修订号，这意味着它的第三级修订号默认为 “0”。
```

**提示：**

- 版本字符串由以点 （.） 分隔的数字字符串组成。这个数字字符串可能有前导零。
- 版本字符串不以点开始或结束，并且其中不会有两个连续的点。

### 代码

'a'表示是一个字符,"a"表示一个字符串相当于'a'+'\0';

首先长度就不一样，而且用双引号括起来的可以直接付给指针；

一个是常量，一个是指针常量；

```c++
class Solution {
public:
    int compareVersion(string s1, string s2) {
        int i = 0, j = 0;
        while (i < s1.size() || j < s2.size())
        {
            int x = i, y = j;
            while (x < s1.size() && s1[x] != '.') x ++ ;//还是那个经典套路
            while (y < s2.size() && s2[y] != '.') y ++ ;
            int a = x == i ? 0 : atoi(s1.substr(i, x - i).c_str());//将字符转成整型数字
            int b = y == j ? 0 : atoi(s2.substr(j, y - j).c_str());
            if (a > b) return 1;
            if (a < b) return -1;
            i = x + 1, j = y + 1;
        }
        return 0;
    }
};

```

https://blog.csdn.net/zhanglu5227/article/details/7895744

## [Leetcode929 独特的电子邮件地址](https://leetcode-cn.com/problems/unique-email-addresses/)

### 题目描述

每封电子邮件都由一个本地名称和一个域名组成，以 @ 符号分隔。

例如，在`alice@leetcode.com`中， `alice`是本地名称，而`leetcode.com`是域名。

除了小写字母，这些电子邮件还可能包含`'.'`或`'+'`。

如果在电子邮件地址的本地名称部分中的某些字符之间添加句点`（'.'）`，则发往那里的邮件将会转发到本地名称中没有点的同一地址。例如，`"alice.z@leetcode.com”` 和 `“alicez@leetcode.com”`会转发到同一电子邮件地址。 （请注意，此规则不适用于域名。）

如果在本地名称中添加加号`（'+'）`，则会忽略第一个加号后面的所有内容。这允许过滤某些电子邮件，例如 `m.y+name@email.com` 将转发到 `my@email.com`。 （同样，此规则不适用于域名。）

可以同时使用这两个规则。

给定电子邮件列表 `emails`，我们会向列表中的每个地址发送一封电子邮件。实际收到邮件的不同地址有多少？

 

**示例：**

```
输入：["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
输出：2
解释：实际收到邮件的是 "testemail@leetcode.com" 和 "testemail@lee.tcode.com"。
```

**提示：**

- `1 <= emails[i].length <= 100`
- `1 <= emails.length <= 100`
- 每封 `emails[i]` 都包含有且仅有一个 `'@'` 字符。

### 代码


![image-20190809222913020](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045340.jpg)

```c++
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> hash;
        for (auto email : emails)
        {
            int at = email.find('@');//find @ 的地址
            string name = email.substr(0, at);//string类型：用双引号，例如：”我是陈希章”
            string domain = email.substr(at + 1);
            string new_name;
            for (char s : name //char类型：用单引号，例如：‘陈’,’A’
            {
                if (s == '+') break;
                else if (s != '.') new_name += s;
            }
            hash.insert(new_name + '@' + domain);  //在set中加入一个值      
        }
        
        return hash.size();
    }
};
```

## [Leetcode5 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

### 题目描述

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```

### 题解 ![image-20190809231528123](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045339.jpg)

### 代码



```c++
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for (int i = 0; i < s.size(); i ++ )
        {
            int j, k;
            for (j = i, k = i; j >= 0 && k < s.size() && s[j] == s[k]; j --, k ++ )
                if (res.size() < k - j + 1)
                    res = s.substr(j, k - j + 1);
            for (j = i - 1, k = i; j >= 0 && k < s.size() && s[j] == s[k]; j --, k ++ )
                if (res.size() < k - j + 1)
                    res = s.substr(j, k - j + 1);
        }
        return res;
    }
};
```

# 07 

![image-20190813183332328](http://blogpicturekoko.oss-cn-beijing.aliyuncs.com/blog/2020-02-29-045341.jpg)

```c++
class Solution {
public:
    string convert(string s, int n) {
        if (n == 1) return s;
        string res;
        for (int i = 0; i < n; i ++ )
        {
            if (!i || i == n - 1)
            {
                for (int j = i; j < s.size(); j += 2 * (n - 1)) res += s[j];
            }
            else
            {
                for (int j = i, k = 2 * (n - 1) - i; j < s.size() || k < s.size(); j += 2 * (n - 1), k += 2 * (n - 1))
                {
                    if (j < s.size()) res += s[j];
                    if (k < s.size()) res += s[k];
                }
            }
        }
        return res;
    }
};
```



# 08

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash;
        int res = 0;
        for (int i = 0, j = 0; j < s.size(); j ++ )
        {
            hash[s[j]] ++ ;
            while (hash[s[j]] > 1) hash[s[i ++ ]] -- ;
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```

# 09

Trie 树

```c++
class Trie {
public:

    struct Node
    {
        bool is_end;
        Node *son[26];

        Node()
        {
            is_end = false;
            for (int i = 0; i < 26; i ++ ) son[i] = NULL;
        }
    } *root;

    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        auto *p = root;
        for (auto c : word)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto *p = root;
        for (auto c : word)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return p->is_end;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        auto *p = root;
        for (auto c : prefix)
        {
            int u = c - 'a';
            if (p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/80601/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



# 10

```c++
class Solution {
public:

    string bit[20] = {"Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine",
                       "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    string decade[10] = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

    string numberToWords(int num) {
        if (!num) return bit[0];
        string res;

        string big[4] = {"Billion", "Million", "Thousand", ""};

        for (int i = 1000000000, j = 0; i > 0; i /= 1000, j ++ )
        {
            if (num >= i)
            {
                res += get_part(num / i) + big[j] + ' ';
                num %= i;
            }
        }

        while (res.back() == ' ') res.pop_back();
        return res;
    }

    string get_part(int num)
    {
        string res;
        if (num >= 100)
        {
            res += bit[num / 100] + " Hundred ";
            num %= 100;
        }
        if (!num) return res;
        if (num >= 20)
        {
            res += decade[num / 10] + ' ';
            num %= 10;
            if (num) res += bit[num] + ' ';
            return res;
        }
        return res + bit[num] + ' ';
    }
};

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/80628/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

