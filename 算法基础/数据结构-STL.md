# vector

变长数组，倍增的思想

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <vector>

using namespace std;

int main()
{
    //vector<int> a; //定义了一个vector
    //vector<int> a(n); //定义一个长度为n的vector
    vector<int> a(10, 3); //定义了一个长度为10的vector，每个值是3
    
    
    for (int i = 0; i < 10; i ++) a.push_back(i);
    
    for (int  i = 0; i < a.size(); i ++) cout << a[i] << ' ';
    cout << endl;
    
    for (vector<int>::iterator i = a.begin(); i != a.end(); i ++) cout << *i << ' ';
    cout << endl;
    // 迭代器可以看成指针
    
    for (auto x : a) cout << x << endl;
    // 范围遍历
    
    clear() // 清空，队列没有清空，不是所有容器都有清空
    front()/back() // 第一个和最后一个数
    push_back()/pop_back() // 插入和删除最后一个数
    begin()/end() // 迭代器 begin 第0个数 end 最后一个数的后面一个数
    [] //支持随机寻址
    
    // size和empty，所有容器都有 时间复杂度o(1)
    
    a.size() // 返回元素的个数
    a.empty() // 返回是否为空
    
    // 系统为某一个程序分配空间时，所需的时间基本上与空间大小无关。与申请次数有关
    // 所以变长数组要尽量减少申请空间的次数
    // 每一次数组长度不够的时候，就把数组长度乘以2，把之前的元素copy过来
    // 额外copy是o(1),申请空间是o(n)
    
    // 支持比较运算
    
    vector<int> a(4, 3), b(3, 4);
    
    if (a < b) puts("a < b");
}

```

