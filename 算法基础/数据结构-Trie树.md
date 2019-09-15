# Trie树

## Trie字符串统计

```c++
#include <iostream>
#include <string>

using namespace std;

const int N = 1e5 + 10;

int son[N][26], cnt[N], idx;

void insert (char str[])
{
    int p = 0;
    for (int i = 0; str[i]; i ++)
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++;
}

int quer(char str[])
{
    int p = 0;
    for (int i = 0; str[i]; i ++)
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main()
{
    int n;
    cin >> n;
    while (n --)
    {
        char op[2];
        char str[N];
        scanf("%s%s", op, str);
        if (op[0] == 'I' ) insert(str);
        else cout << quer(str) << endl;;
    }
}
```

