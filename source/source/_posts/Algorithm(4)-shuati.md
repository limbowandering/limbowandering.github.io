---
title: 题解-矩阵乘法-子串统计-数字分类-数素数
date: 2017-12-9
categories: Algorithm
tags: [PAT题解, 蓝桥杯题解]
---

刷题中.

## 矩阵乘法

题源: [蓝桥杯](http://lx.lanqiao.cn/problem.page?gpid=T218) 

> 问题描述
>
> 　　输入两个矩阵，分别是m*s，s*n大小。输出两个矩阵相乘的结果。
>
> 输入格式
>
> 　　第一行，空格隔开的三个正整数m,s,n（均不超过200）。
> 　　接下来m行，每行s个空格隔开的整数，表示矩阵A（i，j）。
> 　　接下来s行，每行n个空格隔开的整数，表示矩阵B（i，j）。
>
> 输出格式
>
> 　　m行，每行n个空格隔开的整数，输出相乘後的矩阵C（i，j）的值。
>
> 样例输入
>
> 2 3 2
> 1 0 -1
> 1 1 -3
> 0 3
> 1 2
> 3 1
>
> 样例输出
>
> -3 2
> -8 2
> **提示**
> 矩阵C应该是m行n列，其中C(i,j)等于矩阵A第i行行向量与矩阵B第j列列向量的内积。
> 例如样例中C(1,1)=(1,0,-1)*(0,1,3) = 1 * 0 +0*1+(-1)*3=-3

常规题. 记一下

``` C++
#include <iostream>
#include <vector>

using namespace std;

int main(){
  int m, s, n;
  cin >> m >> s >> n;
  vector< vector<int> > a(m,vector<int>(s));
  vector< vector<int> > b(s,vector<int>(n));
  vector< vector<int> > c(m,vector<int>(n));
  for(int i = 0; i < m; i++){
    for(int j = 0; j < s; j++){
      cin >> a[i][j];
    }
  }
  for(int i = 0; i < s; i++){
    for(int j = 0; j < n; j++){
      cin >> b[i][j];
    }
  }
  for(int i = 0; i < m; i++){
    for(int j = 0; j < n; j++){
      for(int k = 0; k < s; k++){
        c[i][j] += a[i][k] * b[k][j];
      }
    }
  }
  for(int i = 0; i < m; i++){
    for(int j = 0; j < n; j++){
      cout << c[i][j] << " ";
    }
    cout << endl;
  }
  return 0;
}
```

## 子串统计

题源: [蓝桥杯](http://lx.lanqiao.cn/problem.page?gpid=T219)

> 问题描述
>
> 　　给定一个长度为n的字符串S，还有一个数字L，统计长度大于等于L的出现次数最多的子串（不同的出现可以相交），如果有多个，输出最长的，如果仍然有多个，输出第一次出现最早的。
>
> 输入格式
>
> 　　第一行一个数字L。
> 　　第二行是字符串S。
> 　　L大于0，且不超过S的长度。
>
> 输出格式
>
> 　　一行，题目要求的字符串。
> 　　输入样例1：
> 　　4
> 　　bbaabbaaaaa
> 　　输出样例1：
> 　　bbaa
> 　　输入样例2：
> 　　2
> 　　bbaabbaaaaa
> 　　输出样例2：
> 　　aa
>
> 数据规模和约定
>
> 　　n<=60
> 　　S中所有字符都是小写英文字母。
>
> 提示
>
> 　　枚举所有可能的子串，统计出现次数，找出符合条件的那个

枚举所有可能的子串, 统计出现次数. 然后根据条件筛选.

``` C++
#include <iostream>
#include <vector>

using namespace std;

int main(){
  int m, s, n;
  cin >> m >> s >> n;
  vector< vector<int> > a(m,vector<int>(s));
  vector< vector<int> > b(s,vector<int>(n));
  vector< vector<int> > c(m,vector<int>(n));
  for(int i = 0; i < m; i++){
    for(int j = 0; j < s; j++){
      cin >> a[i][j];
    }
  }
  for(int i = 0; i < s; i++){
    for(int j = 0; j < n; j++){
      cin >> b[i][j];
    }
  }
  for(int i = 0; i < m; i++){
    for(int j = 0; j < n; j++){
      for(int k = 0; k < s; k++){
        c[i][j] += a[i][k] * b[k][j];
      }
    }
  }
  for(int i = 0; i < m; i++){
    for(int j = 0; j < n; j++){
      cout << c[i][j] << " ";
    }
    cout << endl;
  }
  return 0;
}
```

在此补一些常用的vector操作:

- empty() 
- size()
- clear()
- insert()
- erase()
- push_back()
- pop_back()
- swap()

## 数字分类

这题也不难… 只是一口气写完后 改啊改 挫败感好强

题源: [PAT](https://www.patest.cn/contests/pat-b-practise/1012)

> 给定一系列正整数，请按要求对数字进行分类，并输出以下5个数字：
>
> A1 = 能被5整除的数字中所有偶数的和；
>
> A2 = 将被5除后余1的数字按给出顺序进行交错求和，即计算n1-n2+n3-n4...；
>
> A3 = 被5除后余2的数字的个数；
>
> A4 = 被5除后余3的数字的平均数，精确到小数点后1位；
>
> **输入格式：**
>
> 每个输入包含1个测试用例。每个测试用例先给出一个不超过1000的正整数N，随后给出N个不超过1000的待分类的正整数。数字间以空格分隔。
>
> **输出格式：**
>
> 对给定的N个正整数，按题目要求计算A1~A5并在一行中顺序输出。数字间以空格分隔，但行末不得有多余空格。
>
> 若其中某一类数字不存在，则在相应位置输出“N”。
>
> 输入样例1：
>
> ```
> 13 1 2 3 4 5 6 7 8 9 10 20 16 18
>
> ```
>
> 输出样例1：
>
> ```
> 30 11 2 9.7 9
>
> ```
>
> 输入样例2：
>
> ```
> 8 1 2 4 5 6 7 9 16
>
> ```
>
> 输出样例2：
>
> ```
> N 11 2 N 9
> ```

题解:

``` C++
#include <iostream>
#include <vector>
#include <cstdio>

using namespace std;

#define MAX = 1000;

int main(){
  int N;
  cin >> N;
  vector<int> v;
  int t = 0, cnt = 0, tmp = 0;
  int res1 = 0, res2 = 0, res3 = 0, res5 = 0;
  float res4=0.0;
  int flag1 = 0, flag2 = 0, flag3 = 0, flag4 = 0, flag5 = 0;
  vector<int> a1,a2,a3,a4,a5;

  for(int i = 0; i < N; i++){
    cin >> tmp;
    v.push_back(tmp);
  }

  //就地处理 比上次写的5次单独处理好 就地处理
  for(int i = 0; i < v.size(); i++){
    if(v[i] % 10 == 0){
      res1 += v[i];
      flag1 = 1;
    }
    if(v[i] % 5 == 1){
      if(t % 2 == 0){
        res2 += v[i];
      }else{
        res2 -= v[i];
      }
      t++;
      flag2 = 1;
    }
    if(v[i] % 5 == 2){
      res3++;
      flag3 = 1;
    }
    if(v[i] % 5 == 3){
      res4+=v[i];
      cnt++;
      flag4 = 1;
    }
    if(v[i] % 5 == 4){
      if(res5 < v[i])
        res5 = v[i];
      flag5 = 1;
    }
  }
  res4 = res4/cnt;
  if(flag1 == 0){
    cout << "N ";
  }
  else{
    cout <<res1 << " ";
  }
  if(flag2 == 0){
    cout << "N ";
  }
  else{
    cout << res2 << " ";
  }
  if(flag3 == 0){
    cout << "N ";
  }else{
    cout << res3 << " ";
  }
  if(flag4 == 0){
    cout << "N ";
  }
  else{
    printf("%.1f ",res4);
  }
  if(flag5 == 0){
    cout <<"N";
  }
  else{
    cout << res5;
  }
  return 0;
}
```

## 数素数

很痛苦, 注意最后行末不要输出空格

题源: [PAT](https://www.patest.cn/contests/pat-b-practise/1013)

> 令Pi表示第i个素数。现任给两个正整数M <= N <= 104，请输出PM到PN的所有素数。
>
> **输入格式：**
>
> 输入在一行中给出M和N，其间以空格分隔。
>
> **输出格式：**
>
> 输出从PM到PN的所有素数，每10个数字占1行，其间以空格分隔，但行末不得有多余空格。
>
> 输入样例：
>
> ```
> 5 27
>
> ```
>
> 输出样例：
>
> ```
> 11 13 17 19 23 29 31 37 41 43
> 47 53 59 61 67 71 73 79 83 89
> 97 101 103
> ```

题解:

``` C++
#include <iostream>
#include <vector>

using namespace std;

bool isPrime(int x){
  for(int i = 2; i * i <= x; i++){
    if(x % i == 0){
      return false;
    }
  }
  return true;
}

int main(){
  int M, N, cnt = 0;
  cin >> M >> N;
  vector<int> res;
  int i = 2;
  while(cnt < N){
    if(isPrime(i)){
      cnt++;
      if(cnt >= M)
        res.push_back(i);
    }
    i++;
  }
  // int s = res.size();
  // int row = s / 10;
  // if(s % 10 == 0){
  //   row -= 1;
  // }
  // for(int j = 0; j < row; j++){
  //   for(int k = 0; k < 9; k++){
  //     cout << res[10*j + k] << " ";
  //   }
  //   cout << res[(j+1)*10 - 1] << endl;
  // }
  // for(int j = row*10; j < s - 1; j++){
  //   cout << res[j] << " ";
  // }
  // cout << res[s-1];

  for(int j = 0; j < res.size() - 1; j++){
    cout << res[j];
    if((j+1) % 10 == 0){
      cout << endl;
    }
    else cout << " ";
  }
  cout << res[res.size() - 1] << endl;
  return 0;
}


```

