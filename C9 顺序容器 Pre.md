---
marp: true
size: 16:9
paginate: true

---
C++学习分享
===
C9  顺序容器

---
顺序容器
===

1. vector容器————数组
   **支持快速随机访问**

2. list容器————链表
   **支持快速插入/删除**
   
3. deque容器————双端队列/双端数组
    **随机访问强于list**
    **插入/删除强于vector**

---
vector容器
===
1. 构造函数
```c++
#include<vector>
vector<int> v1;//默认构造
vector<int> v2(v1.begin(),v1.end());//区间方式构造
vector<int> v3(10,100);//n个elem方式构造
vector<int> v4(v3);//拷贝构造
```
容器元素类型满足：
- 元素类型必须支持赋值运算（*排除引用类型*）
- 元素类型的对象必须可以复制

v1.begin()返回迭代器指向v1的第一个元素；
v1.end()返回迭代器指向v1最后一个元素的下一个位置。

---
迭代器
===
每个容器都有它的迭代器————指针
对v1的遍历算法：
```c++
for(vector<int>::iterator it = v1.begin(); it != v1.end(); ++v1)
{
    cout << *it << endl;
}
for(vector<Person>::const_iterator it = v1.begin(); it != v1.end(); ++v1)
{
    cout << (*it).name << endl;
    cout << it->name << endl;
}
//嵌套类型
vector<vector<string>> lines;//error
vector< vector<string> > lines;//ok
```
---

vector容器
===
2. 赋值操作
```c++
#include<vector>
vector<int> v1;
vector<int> v2;
v2 = v1;//=
vector<int> v3;
v3.assign(v1.begin(),v1.end());//assign，区间赋值
vector<int> v4;
v4.assign(10,100);//assign，n个elem
v1.swap(v2);//交换v1和v2的元素
```
---
vector容器
===
3. 容量和大小
```c++
#include<vector>
vector<int> v1;
v1.empty();//判断容器是否为空
v1.capacity();//返回容器的容量
v1.size();//返回容器中元素的个数
v1.resize(int num);//重新指定容器的size为num，可能也会改变capacity
v1.resize(int num,elem);
v1.reserve(int num);//预留空间capacity=num，好处：减少动态扩容的次数
```
小心迭代器失效

---
vector容器
===
4. 插入和删除
```c++ 
#include<vector>
vector<int> v1;
v1.push_back(int num);
v1.pop_back();
v1.insert(const_iterator pos,elem);//在迭代器pos前插入elem
v1.insert(const_iterator pos,int count,elem);//在迭代器pos前插入count个elem
v1.erase(const_iterator pos);//删除迭代器pos指向元素
v1.erase(const_iterator start,const_iterator end);//删除迭代器从start到end之间的元素
v1.clear();//清空容器
```


---
vector容器
===
5. 数据存取
```c++
#include<vector>
vector<int> v1;
v1[index];
v1.at(index);
v1.front();//返回第一个元素
v1.back();//返回最后一个元素
```
---
deque容器
===
deque插入删除强于vector
deque支持**随机访问**
```c++
//容器迭代器共有的操作
iter++;
*iter;
iter1 == iter2;
//支持随机存取的容器
iter + n;
iter1 - iter2;
iter1 > iter2;
```
deque内部工作原理（右图）
从逻辑上看，deque使用连续的内存空间
在两端进行插入/删除操作不会移动元素
![bg right 99%](deque实现原理.png)

---
deque容器
===
1. 构造函数
   同vector
2. 赋值操作
   同vector
3. 容量和大小
   没有capacity和reserve操作
4. 插入和删除
   相比较vector多了**push_front**和**pop_front**操作
5. 数据存取
   同vector

---
list容器
===
1. 构造函数
   同vector
2. 赋值操作
   同vector
3. 容量和大小
   没有capacity和reserve操作
4. 插入和删除
   相比较vector多了**push_front**，**pop_front**，**remove(elem)** 等操作
   **remove(elem)**//删除所有值为elem的元素
5. 数据存取
   不可以用索引访问[]/at，只有front和back操作

---
顺序容器适配器
===
1. stack
   **后进先出栈**
2. queue
   **先进先出队列**
3. priority_queue
   **有优先级管理的队列**


适配器包括 容器适配器，迭代器适配器，函数适配器。
***本质上，适配器是使一事物的行为类似于另一事物的行为的一种机制。***
例如：stack适配器可以使任何一种顺序容器以栈的方式工作。
类比学过的基于顺序表实现的顺序栈，基于链表实现的链栈

---
顺序容器适配器
===
stack可以建立在vector,list,deque容器之上；

queue可以建立在list，deque容器上，不能建立在vector上；

priority_queue要求提供随机访问功能，可以建立在vector,deque上，不能建立在list上。

priority_queue默认是**大根堆**，根结点是优先级最高的。
联想用数组存放完全二叉树，在构建大根堆时需要调堆操作，子结点需要和父结点比较，自然要求提供随机访问的功能。

---
stack常用接口
===
1. 构造函数
```c++
#include<stack>
stack<int> s1;//默认构造
stack<int> s2(s1);//拷贝构造
```
2. 赋值操作
```c++
s2 = s1;//=
```
3. 数据存取
```c++
push(elem);//入栈
pop();//移除栈顶元素
top();//取栈顶元素
```
4. 大小操作
```c++
empty();size();
```
---
queue常用接口
===
1. 构造函数
2. 赋值操作
3. 数据存取
```c++
#include<queue>
push(elem);//入队
pop();//出队
back();//返回队首元素
front();//返回队尾元素
```
4. 大小操作

---
priority_queue常用接口
===
1. 构造函数
2. 赋值操作
3. 数据存取
```c++
#include<queue>
push(elem);//基于优先级在适当位置插入元素
pop();//删除队首元素，不返回其值
top();//返回最高优先级的元素值
```
4. 大小操作

---
string容器
===
1. 构造函数
```c++
#include<string>
string s1;//默认构造
char * str = "zhangsan";
string s2(str);//用c风格字符串来初始化
string s3(s2);//拷贝构造
string s4(5, 'a');//n个char方式构造
```
---
string容器
===
2. 赋值操作
```c++
#include<string>
// =
string str1;
str1 = "zhangsan";
str1 = 'a';
string str2;
str2 = str1;
// assign
str2.assign("zhangsan");
str2.assign("zhangsan",3);//将"zhangsan"的前3个字符赋给str2
str2.assign(5,'a');//n个char方式赋值
str2.assign(str1);
```
---
string容器
===
3. string字符串拼接
```c++
#include<string>
// +=
string str1;
str1 += "zhangsan";
str1 += 'a';
string str2;
str2 += str1;
// append
str2.append("zhangsan");
str2.append("zhangsan",3);//将"zhangsan"的前3个字符拼接到str2后面
str2.append(str1);
str2.append(str1, 1, 3);//将str1第1个位置开始截取3个字符拼接到str2后面
```
---
string容器
===
4. string查找和替换

```c++
str.find(args);//在str中查找args的第一次出现
str.rfind(args);//在str中查找args的最后一次出现
str.find_first_of(args);//在str中查找"args的任意字符"的第一次出现
str.find_first_not_of(args);//在str中查找第一个不属于args的字符
str.find_last_of(args);//在str中查找"args的任意字符"的最后一次出现
str.find_last_not_of(args);//在str中查找最后一个不属于args的字符

str.replace(int pos, int n, args);//args不能是迭代器表示的范围
str.replace(b, e, args);//args不能有下标
```
找到则返回下标(string::size_type类型的值)，找不到则返回一个名为**string::npos**的特殊值，string类将其定义成保证大于任何有效下标的值；
```c++
string str1 = "abcdef";
cout << str1.find('g') << endl;

18446744073709551615
```
---
string容器
===
5. string字符串比较
```c++
#include<string>
string str1 = "abc";
string str2 = "d";
str1.compare(str2);
char * str = "zhangsan";
str1.compare(str);
```
按照ASCII码进行比较：
= 返回 0
\> 返回 1
< 返回 -1

---
string容器
===
6. string字符存取
```c++
string str;
str[index];
str.at(index);
```
7. string插入和删除
```c++
string str;
str.insert(int pos,string);
str.insert(int pos,char *);
str.insert(int pos,int n, char c);//指定位置pos插入n个c
str.erase(int pos, int n);//从指定位置pos开始删除n个字符
```
8. string子串
```c++
string str;
str.substr(int pos, int n);//返回从pos开始的n个字符组成的字符串
```
---
总结
===
- vector, list, deque, stack, queue, priority_queue, string的常用操作
- vector, list, deque三者的优缺点
- 注意那些可能导致迭代器失效的操作（插入/删除元素，改变容器长度等等）

