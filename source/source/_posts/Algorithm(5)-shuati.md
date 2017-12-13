---
title: 题解-德才论-石头剪刀布-数字黑洞-D进制
date: 2017-12-13 12:00:00
categories: Algorithm
tags: [PAT题解]
---

刷题中; 题源: [PAT乙级](https://www.patest.cn/contests/pat-b-practise)

## 1015-德才论

> 宋代史学家司马光在《资治通鉴》中有一段著名的“德才论”：“是故才德全尽谓之圣人，才德兼亡谓之愚人，德胜才谓之君子，才胜德谓之小人。凡取人之术，苟不得圣人，君子而与之，与其得小人，不若得愚人。”
>
> 现给出一批考生的德才分数，请根据司马光的理论给出录取排名。
>
> **输入格式：**
>
> 输入第1行给出3个正整数，分别为：N（<=105），即考生总数；L（>=60），为录取最低分数线，即德分和才分均不低于L的考生才有资格被考虑录取；H（<100），为优先录取线——德分和才分均不低于此线的被定义为“才德全尽”，此类考生按德才总分从高到低排序；才分不到但德分到线的一类考生属于“德胜才”，也按总分排序，但排在第一类考生之后；德才分均低于H，但是德分不低于才分的考生属于“才德兼亡”但尚有“德胜才”者，按总分排序，但排在第二类考生之后；其他达到最低线L的考生也按总分排序，但排在第三类考生之后。
>
> 随后N行，每行给出一位考生的信息，包括：准考证号、德分、才分，其中准考证号为8位整数，德才分为区间[0, 100]内的整数。数字间以空格分隔。
>
> **输出格式：**
>
> 输出第1行首先给出达到最低分数线的考生人数M，随后M行，每行按照输入格式输出一位考生的信息，考生按输入中说明的规则从高到低排序。当某类考生中有多人总分相同时，按其德分降序排列；若德分也并列，则按准考证号的升序输出。
>
> 输入样例：
>
> ```
> 14 60 80
> 10000001 64 90
> 10000002 90 60
> 10000011 85 80
> 10000003 85 80
> 10000004 80 85
> 10000005 82 77
> 10000006 83 76
> 10000007 90 78
> 10000008 75 79
> 10000009 59 90
> 10000010 88 45
> 10000012 80 100
> 10000013 90 99
> 10000014 66 60
>
> ```
>
> 输出样例：
>
> ```
> 12
> 10000013 90 99
> 10000012 80 100
> 10000003 85 80
> 10000011 85 80
> 10000004 80 85
> 10000007 90 78
> 10000006 83 76
> 10000005 82 77
> 10000002 90 60
> 10000014 66 60
> 10000008 75 79
> 10000001 64 90
> ```

题解:

``` C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

struct node{
  int num;
  int de;
  int cai;
};

int cmp(struct node a, struct node b){
  if((a.de + a.cai) != (b.de + b.cai))
    return (a.de + a.cai) > (b.de + b.cai);
  else if(a.de != b.de)
    return a.de > b.de;
  else
    return a.num < b.num;
}

int main(){
  int n, low, high;
  scanf("%d %d %d", &n, &low, &high);
  vector<node> v[4];
  node tmp;
  int total = n;
  for(int i = 0; i < n; i++){
    scanf("%d %d %d", &tmp.num, &tmp.de, &tmp.cai);
    if(tmp.de < low || tmp.cai < low){
      total--;
    }else if(tmp.de >= high && tmp.cai >= high){
      v[0].push_back(tmp);
    }else if(tmp.de >= high && tmp.cai < high){
      v[1].push_back(tmp);
    }else if(tmp.de < high && tmp.cai < high && tmp.de >= tmp.cai){
      v[2].push_back(tmp);
    }else{
      v[3].push_back(tmp);
    }
  }
  printf("%d\n", total);
  for(int i = 0; i < 4; i++){
    sort(v[i].begin(), v[i].end(), cmp);
    for(int j = 0; j < v[i].size(); j++){
      printf("%d %d %d\n", v[i][j].num, v[i][j].de, v[i][j].cai);
    }
  }
  return 0;
}
```

## 1018-石头剪刀布

> 现给出两人的交锋记录，请统计双方的胜、平、负次数，并且给出双方分别出什么手势的胜算最大。
>
> **输入格式：**
>
> 输入第1行给出正整数N（<=105），即双方交锋的次数。随后N行，每行给出一次交锋的信息，即甲、乙双方同时给出的的手势。C代表“锤子”、J代表“剪刀”、B代表“布”，第1个字母代表甲方，第2个代表乙方，中间有1个空格。
>
> **输出格式：**
>
> 输出第1、2行分别给出甲、乙的胜、平、负次数，数字间以1个空格分隔。第3行给出两个字母，分别代表甲、乙获胜次数最多的手势，中间有1个空格。如果解不唯一，则输出按字母序最小的解。
>
> 输入样例：
>
> ```
> 10
> C J
> J B
> C B
> B B
> B C
> C C
> C B
> J B
> B C
> J J
>
> ```
>
> 输出样例：
>
> ```
> 5 3 2
> 2 3 5
> B B
>
> ```

``` C++
#include <iostream>
using namespace std;

int main(){
  int n;
  cin >> n;
  int jiawin = 0, yiwin = 0;
  int jia[3] = {0}, yi[3] = {0};
  for(int i = 0; i < n; i++){
    char s,t ;
    cin >> s >> t;
    if(s == 'B' && t == 'C'){
      jiawin++;
      jia[0]++;
    }else if (s == 'B' && t == 'J') {
      yiwin++;
      yi[2]++;
    } else if (s == 'C' && t == 'B') {
      yiwin++;
      yi[0]++;
    } else if (s == 'C' && t == 'J') {
      jiawin++;
      jia[1]++;
    } else if (s == 'J' && t == 'B') {
      jiawin++;
      jia[2]++;
    } else if (s == 'J' && t == 'C') {
      yiwin++;
      yi[1]++;
    }
  }
  cout << jiawin << " " << n - jiawin - yiwin << " " << yiwin << endl << yiwin <<" " << n -jiawin - yiwin << " " << jiawin<<endl;
  int maxjia = jia[0] >= jia[1] ? 0 : 1;
  maxjia = jia[maxjia] >= jia[2] ? maxjia : 2;
  int maxyi = yi[0] >= yi[1] ? 0 : 1;
  maxyi = yi[maxyi] >= yi[2] ? maxyi : 2;
  char str[4] = {"BCJ"};
  cout << str[maxjia] << " " << str[maxyi];
  return 0;
}
```

## 1019-数字黑洞

> 给定任一个各位数字不完全相同的4位正整数，如果我们先把4个数字按非递增排序，再按非递减排序，然后用第1个数字减第2个数字，将得到一个新的数字。一直重复这样做，我们很快会停在有“数字黑洞”之称的6174，这个神奇的数字也叫Kaprekar常数。
>
> 例如，我们从6767开始，将得到
>
> 7766 - 6677 = 1089
> 9810 - 0189 = 9621
> 9621 - 1269 = 8352
> 8532 - 2358 = 6174
> 7641 - 1467 = 6174
> ... ...
>
> 现给定任意4位正整数，请编写程序演示到达黑洞的过程。
>
> **输入格式：**
>
> 输入给出一个(0, 10000)区间内的正整数N。
>
> **输出格式：**
>
> 如果N的4位数字全相等，则在一行内输出“N - N = 0000”；否则将计算的每一步在一行内输出，直到6174作为差出现，输出格式见样例。注意每个数字按4位数格式输出。
>
> 输入样例1：
>
> ```
> 6767
>
> ```
>
> 输出样例1：
>
> ```
> 7766 - 6677 = 1089
> 9810 - 0189 = 9621
> 9621 - 1269 = 8352
> 8532 - 2358 = 6174
>
> ```
>
> 输入样例2：
>
> ```
> 2222
>
> ```
>
> 输出样例2：
>
> ```
> 2222 - 2222 = 0000
> ```

这题要注意如果是0000, 6174 也要输出一次. 

``` C++
#include <iostream>
#include <algorithm>

using namespace std;

bool cmp(char a, char b){return a > b;}

int main(){
  string s;
  cin >> s;
  s.insert(0, 4 - s.length(), '0');
  do{
    string a = s, b = s;
    sort(a.begin(), a.end(), cmp);
    sort(b.begin(),b.end());
    int result = stoi(a) - stoi(b);
    s = to_string(result);
    s.insert(0, 4 - s.length(), '0');
    cout << a << " - " << b << " = " << s << endl;
  }while(s != "6174" && s != "0000");

  return 0;
}
```

## 1022 D进制

> 输入两个非负10进制整数A和B(<=230-1)，输出A+B的D (1 < D <= 10)进制数。
>
> **输入格式：**
>
> 输入在一行中依次给出3个整数A、B和D。
>
> **输出格式：**
>
> 输出A+B的D进制数。
>
> 输入样例：
>
> ```
> 123 456 8
>
> ```
>
> 输出样例：
>
> ```
> 1103
> ```

``` C++
#include <iostream>

using namespace std;

int main(){
  int a,b,d;
  cin >> a >> b >>d;
  int t = a + b;
  //注意边界情况
  if (t == 0){
    cout << 0;
    return 0;
  }
  //int s[100];
  int s[30];
  int i = 0;
  while(t != 0){
    s[i++] = t % d;
    t /= d;
  }
  for(int j = i - 1; j >= 0; j--){
    cout << s[j];
  }

  return 0;
}
```

