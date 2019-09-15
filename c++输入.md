```c++
#include<bits/stdc++.h>
using namespace std;

int main() {
 
 vector<int> vec;
 int tmp;
 char ch;
 while (cin>>tmp) {
  vec.push_back(tmp);
  if (getchar()=='\n') {
   break;
  }
 }
 vector<int> vec2;
 while (cin >> tmp) {
  vec2.push_back(tmp);
  if (getchar() == '\n') {
   break;
  }
 }
 for (auto x : vec) {
  cout << x << " ";
 }
 cout << endl;
 for (auto x : vec2) {
  cout << x << " ";
 }
 cout << endl;
 system("pause");
 return 0;
}
```

```c++
#include<iostream> 
#include<set> 
using namespace std; 
int main() 
{ int m,n;
while(cin>>m>>n) 
{ set<int> s; 
int t; 
for(int i=0;i<m;++i) 
{ cin>>t; s.insert(t); } 
for(int i=0;i<n;++i) 
{ cin>>t; s.insert(t); } 
set<int>::iterator it = s.begin(); while(it!=s.end()) { cout<<*it<<" "; ++it; } cout<<endl; } return 0; }
```

```c++
// 6. 
 // 按行输入数组vector<int> num 不执行中断 
 string s; 
 while (getline(cin, s)) //屈一整行 
 { 
 vector<int> num; 
 int start_index = 0; 
 for (int i = 0; i != s.size(); ++i)  
 { 
 if (s[i] == ' ') //空格区分 
 { 
 num.push_back(stoi(s.substr(start_index, i - start_index)));//子串转换为整数 
 start_index = i + 1; 
 } 
 } 
 num.push_back(stoi(s.substr(start_index, s.size() - start_index))); 
 }
```

