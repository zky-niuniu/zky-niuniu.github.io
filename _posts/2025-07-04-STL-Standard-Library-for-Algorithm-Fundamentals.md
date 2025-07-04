# 算法STL基础

## STL
包含容器、算法、迭代器、仿函数、适配器、空间适配器等。
## vector存放内置数据类型和自定义数据类型
### 内置数据类型
~~~c++
vector<int>v1;
v.push_back(10);
v.push_back(20);
v.push_back(30);
v.push_back(40);
for(vector<int>::iterator it=v.begin();it!=v.end();it++)
{
  cout<<*it;
}
//还可以通过for_each遍历，while循环遍历
~~~
### 自定义数据类型
~~~c++
class person{
  public:
    person(string name,int age)
    {
      mage=age;
      mname=name;
    }
  public:
    string mname;
    int mage;
};
 vector<person> v;
 person p1("aaa",10);
 person p2("bbb",10);
 person p3("ccc",10);
 person p4("ddd",10);
 v.push_back(p1);
 v.push_back(p2);
 v.push_back(p3);
 v.push_back(p4);
for(vector<person>::iterator it=v.begin();it!=v.end();it++)
{
   cout<<it->mname<<" "<<it->mage<<endl;
}
~~~
## 容器嵌套容器
~~~c++
vector<vector<int>>v;
vector<int>v1;
vector<int>v2;
vector<int>v3;
vector<int>v4;
for(int i=0;i<4;i++)
{
  v1.push_back(i+1);
  v2.push_back(i+2);
  v3.push_back(i+3);
  v4.push_back(i+4);
}
v.push_back(v1);
v.push_back(v2);
v.push_back(v3);
v.push_back(v4);
for(vector<vector<int>>::iterator it =v.begin();it!=v.end();it++)
{
  for(vector<int>::iterator vit=(*it).begin();vit!=(*it).end();vit++)
  {
    cout<<*vit<<" ";
  }
  cout<<endl;
}
~~~
## string容器
### string构造函数
~~~c++
//无参构造，默认
string s;
//使用字符串s初始化
const char* str="hello";
string s1(str);
//使用一个string创建另一个string
string s2(s1);
//用n个字符初始化
string s3(10,'a');
~~~
### string 赋值
~~~c++
//等号方法
string str1;
str1="hello";
string str2;
str2=str1;
string str3;
str3='1';
//assign方法
string str4;
str4.assign("hello");
str4.assign("hello c++",5);
str4.assign(str5);
str4.assign(10,'2');
~~~
### 字符串拼接
~~~c++
string str1="c++";
string str2="hello";
//+=重载
str1+="你好"；
str1+=':';
str1+=str2;
//append
str1.append("love");
str1.append("game abcd",4)
str1.append(str2);
str1.append(str2,0,3)
~~~
### 字符串查找替换
~~~c++
//find
string str1="abcd";
int pos=str1.find("cd");
//rfind
int pos=str1/rfind("cd");
//rfinds是从右往左查找
str1.replace(1,3,"111");
~~~
### 字符串比较
~~~c++
string str1="hello";
string str2="hello";
str1.compare(str2);//-1 0 1
~~~
### 字符串存取
~~~c++
string str="hello"
for(int i=0;i<str.size();i++)
{
  cout<<str[i];
}
for(int i=0;i<str.size();i++)
{
  cout<<str.at(i);
}
//修改
str[0]='w';
str.at(1)='2';
~~~
### 字符串插入删除
~~~c++
string str="hello";
str.insert(1,"1111");
str.erase(1,4);
~~~
### 子串获取
~~~c++
string str1="hello";
str1.substr(1,3);
~~~
## vector容器
动态数组
### 构造
~~~c++
vector<Type>v;
vector<int>v2(v.begin(),v.end());
vector<int>v3(10,100);
vector<int>v4(v3);
~~~
### 赋值
~~~c++
vector<int>v;
for(int i=0;i<10;i++)
{
  v.push_back(i);
}
vector<int>v1;
v1=v;
v1.assign(v.begin();v.end());
v1.assign(10;100);
~~~
### 容量大小
~~~c++
//empty,capacity,size,resize
vector<int>v1;
for(int i=0;i<10;i++)
{
  v1.push_back(i);
}
v1.empty();//判断空
v1.capacity();//容量
v1.size();//有多少元素
v1.resize(15);//指定大小，没有的补上0，超出删掉
~~~
### 插入删除
~~~c++
vector<int>v1;
v1.push_back(1);
v1.pop_back();
v1.insert(v1.begin(),10);
v1.erase(v1.begin(),10);
v1.clear();
~~~
### 数据存取
~~~c++
vector<int>v;
v[0];
v.at(0);
v.front();
v.back();
~~~
### 互换容器
~~~c++
vector<int>v1;
vector<int>v2;
v1.swap(v2);
//收缩大小,v大小13万,插入三个数字
vector<int>(v).swap(v);
~~~
### 预留空间
~~~c++
vector<int>v;
//插入10万数
int num=0;
int *p=NULL;
for(int i=0;i<100000;i++)
{
  v.push_back(i);
  if(p!=&v[0])
  {
    p=&v[0];
    num++;
  }
}
v.reserve(100000);
~~~
## deque容器
双端数组，可以对头尾进行插入删除操作
### 构造
~~~c++
deque<int>d1;
d1.push_back(1);
d1.push_back(2);
~~~
### 赋值
~~~c++
deque<int>d1;
for(int i=0;i<10;i++)
{
  deque.push_back(i);
}
deque<int>d2;
d2=d1;
d2.assign(d1);
~~~
### 大小操作
~~~c++
deque.empty();
deque.size();
deque.resize(num);
deque.ressize(num,ele);
~~~
### 插入删除
~~~c++
deque<int>d;
d.push_back(elem);//尾插
d.push_front(elem);//头插
d.pop_back();//尾删
d.front_front();//头删
d.insert();//指定位置插入指定个数的元素
d.clear();//全部删除
d.erase();//指定位置删除
~~~
### 数据存取
~~~c++
deque<int>d;
d[pos];
d.at(pos);
d.front();
d.back();
~~~
### 排序
~~~c++
deque<int>d;
sort(d.begin(),d.end());
~~~
## stack容器
### 常用接口
先进后出的数据结构,弹匣
~~~c++
stack<int>stk;
//入
stk.push(elem);
//查栈顶
stk.top();
//查大小
stk.size();
//出
stk.pop();
//判空
stk.empty();
~~~
## queue容器
### 常用接口
先进先出
~~~c++
queue<int>que;
//入队
que.push(elem);
//队头
que.front();
//队尾
que.back();
//出队
que.pop();
//队大小
que.size();
//判空
que.empty();
~~~
## list容器
### 概念
双向循环链表！
### 构造函数
~~~c++
list<int>lst;
lst.push_back(10);
~~~
### 赋值交换
~~~c++
list<int>lst;
lst.push_back(10);
lst.push_back(20);
lst.push_back(30);

list<int>lst1;
lst1=lst;
lst1.assign(lst.begin(),lst.end());
lst1.assign(4,10);

lst1.swap(lst);
~~~
### 大小操作
~~~c++
list<int>lst;
lst.size();
lst.empty();
lst.resize(num);
~~~
### 插入删除
~~~c++
list<int>l;
l.push_back(10);
l.push_front(10);
l.pop_back();
l.pop_front();

l.insert();//指定位置插入指定元素
l.remove(value);
l.erase(迭代器);
l.clear();
~~~
### 数据存取
~~~c++
list<int>l;
l.front();
l.back();
~~~
### 反转排序
~~~c++
list<int>l;
l.reserve();
l.sort();
~~~
## set容器
自动排序，不重复，二叉树实现
### 构造赋值
~~~c++
set<int>s;
s1.insert(10);
s1.insert(30);
s1.insert(20);

set<int>s2;
s2=s;
~~~
### 大小交换
~~~c++
set<int>s;
set<int>s1;
s.size();
s.empty();
s.swap(s1);
~~~
### 插入删除
~~~c++
set<int>s;
s.insert(elem);
s.clear();
s.erase(pos);
s.erase(begin,end);
s.erase(elem);
~~~
### 查找统计
~~~c++
set<int>s;
s.find(key);//返回位置
s.count(key);//返回个数
~~~
### set和multiset区别
后者可以插入重复值。
## pair的使用
~~~
pair<type,type> p(value,value);
p.first;
p.second;
~~~
## set内置类型排序
~~~c++
class mycompare
{
public:
  bool operator()(int v1,int v2)
  {
    return v1>v2;
  }
};

set<int>s;
s.insert(1);
s.insert(2);
s.insert(3);
s.insert(4);

set<int,mycompare>s2;
s2.insert(1);
s2.insert(2);
s2.insert(3);
s2.insert(4);
~~~
## set自定义数据类型排序
~~~c++
class person
{
public:
  person(string name,int age)
  {
    this->m_name=name;
    this->m_age=age;
  }
  string m_name;
  int m_age;
};
class mycompare
{
public:
  bool operator()(const person& p1,const person& p2)
  {
    return p1.m_age>p2.m_age
  }
};


set<person,mycompare>s;
person p1("aaa",1);
person p2("bbb",2);
person p3("ccc",3);
person p4("ddd",4);
s.insert(p1);
s.insert(p2);
s.insert(p3);
s.insert(p4);
~~~
## map容器
所有元素都是pair，第一个元素是key，第二个是value
所有自动排序，二叉树实现，快速查找
map不可重复key，multimap可以重复key
### 构造、赋值
~~~c++
map<int,int>m;
m.insert(pair<int,int>(1,10));
~~~
### 大小、交换
~~~c++
map<int,int>m;
map<int,int>m2;
m.size();
m2.swap(m);
~~~
### 插入、删除
~~~c++
map<int,int>m;
m.insert(make_pair(2,20));
m[4]=40;

m.clear();
m.erase(pos);
m.erase(beigin,end);
m.erase(key);
~~~
### 查找、统计
~~~c++
map<int,int>m;
m.find(key);//返回位置
m.count(key);//返回个数
~~~
### 排序
~~~c++
class mycompare
{
  bool operator()(int v1,int v2)
  {
    return v1>v2;
  }
};
map<int,int,mycompare>m;
~~~























