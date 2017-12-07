---
title: 题解-数组循环右移-说反话
date: 2017-12-7
categories: Algorithm
tags: [PAT题解,C++,数据结构]
---

刷题中.

## 数组元素循环右移问题

题源: [PAT乙级](https://www.patest.cn/contests/pat-b-practise/1008)

> 一个数组A中存有N（N>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移M（M>=0）个位置，即将A中的数据由（A0A1……AN-1）变换为（AN-M …… AN-1 A0 A1……AN-M-1）（最后M个数循环移至最前面的M个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？
>
> **输入格式：**每个输入包含一个测试用例，第1行输入N ( 1<=N<=100)、M（M>=0）；第2行输入N个整数，之间用空格分隔。
>
> **输出格式：**在一行中输出循环右移M位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。
>
> 输入样例：
>
> ```
> 6 2
> 1 2 3 4 5 6
>
> ```
>
> 输出样例：
>
> ```
> 5 6 1 2 3 4
> ```

一些注释:

Begin函数 `Iterator begin();`  begin()函数返回一个指向起始元素的迭代器.

举个栗子:

```C++
vector<int> vl(5,789); 
vector<int>::iterator it;
for(it = vl.begin(); it != vl.end(); it++){
  cout << *it << endl;
}
//使用一个迭代器显示出vector的所有元素
```

`reverse` 函数把list所有元素倒转. 不包括右边界.

关于迭代器: 

> 迭代器可被用来访问一个容器类的所包含的全部元素.
>
> |          迭代器           |                    描述                    |
> | :--------------------: | :--------------------------------------: |
> |     input_iterator     |   提供读功能的向前迭代器, 可以被进行增加(++), 比较与解引用(*)    |
> |    output_iterator     |   提供写功能的向前移动迭代器，它们可被进行增加(++)，比较与解引用（*)   |
> |    forward_iterator    | 可向前移动的，同时具有读写功能的迭代器。同时具有input和output迭代器的功能，并可对迭代器的值进行储存。 |
> | bidirectional_iterator | 双向迭代器，同时提供读写功能，同forward迭代器，但可用来进行增加(++)或减少(--)操作。 |
> |    random_iterator     | 随机迭代器，提供随机读写功能.是功能最强大的迭代器， 具有双向迭代器的全部功能，同时实现指针般的算术与比较运算。 |
> |    reverse_iterator    | 如同随机迭代器或双向迭代器，但其移动是反向的。（Either a random iterator or a bidirectional iterator that moves in reverse direction.）（我不太理解它的行为） |

题解代码:

```C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main(){
  int n, m;
  cin >> n >> m;
  vector<int> a(n);
  for(int i = 0; i < n; i++){
    cin >> a[i];
  }
  if(m != n && m != 0){
    if(m > n) m -= n;
    reverse(begin(a), begin(a) + n);
    reverse(begin(a), begin(a) + m);
    reverse(begin(a)+m, begin(a)+n);
  }
  for(int i = 0; i < n - 1; i++){
    cout << a[i] << " ";
  }
  cout << a[n - 1];
  return 0;
}
```

## 说反话

题源:[PAT](https://www.patest.cn/contests/pat-b-practise/1009)

> 给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。
>
> **输入格式：**测试输入包含一个测试用例，在一行内给出总长度不超过80的字符串。字符串由若干单词和若干空格组成，其中单词是由英文字母（大小写有区分）组成的字符串，单词之间用1个空格分开，输入保证句子末尾没有多余的空格。
>
> **输出格式：**每个测试用例的输出占一行，输出倒序后的句子。
>
> 输入样例：
>
> ```
> Hello World Here I Come
>
> ```
>
> 输出样例：
>
> ```
> Come I Here World Hello
> ```

``` C++
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int main(){
  stack<string> v;
  string s;
  while(cin >> s){
    v.push(s);
  }
  cout << v.top();
  v.pop();
  while(!v.empty()){
    cout << " " << v.top();
    v.pop();
  }
  return 0;
}
```

用STL就会快很多

stack常用的操作: 

- empty(). 堆栈为空则返回真.
- pop(). 移除栈顶元素
- push(). 在栈顶增加元素.
- size(). 返回栈中元素数目.
- top(). 返回栈顶元素/

## 一些边界值处理

多想想边界值处理. 坑...

- 题中给n<=多少啊, 问题是n如果取1,2,3之类的怎么办. 
- 没有输入的情况
- 前后空格
- ​

## 拼写错误

- main
- false, true
- 等等...