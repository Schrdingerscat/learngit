# **C++面向对象的编程：类（Class）**

> 这个笔记是关于C语言向C++过渡的一些知识点。

## 1、class与struct的不同

* C的struct是属于面向过程的编程方法，而C++的Class是属于面向对象的编程方法。

  > > [一个关于面向过程和面向对象思想的生动诠释][(22 封私信 / 80 条消息) 如何通俗易懂地举例说明「面向对象」和「面向过程」有什么区别？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/27468564/answer/757537214)

* C的struct相当于Class中的成员部分（Data）（该类的属性），却没有Class中设定函数的功能（该类的方法）（Operation）

* Class中private一般存储类的属性，public一般储存函数的接口。public保证了外部不能访问调用内部数据，保护数据安全。C的struct中的成员被默认在public里面——容易在外部随意更改。

* 小程序中用面向过程比较方便：想改啥就改啥？大程序用面向对象比较方便，只需要修改一部分接口，更能够实现继承、封装和多态。

## 2、class的基本语法结构
```C++
class 类名
{
    public:
    //公共的行为或属性
 
    private:
    //公共的行为或属性
};
```
## 3、class的代码放置的位置

Declaration放在.h的头文件中，Defination放在.cpp的源文件中。

使用的时候需要#include"XXX.h"

但是对于模板类函数，declaration和definiton不能分开

([(9条消息) 模板类成员函数的定义和声明为什么要放在一个文件中_心如止水_03的博客-CSDN博客](https://blog.csdn.net/qq_37655087/article/details/81202883))

## 4、class与struct对比的代码实例(以一个简单链表为例)

```c++
//第一次用c++写数据结构居然不是在VC？！
//linklist.h
class node
{	
    public:
    int value;
    node* next;
}
class linklist 
{
    private:
    node *head; 
 	public:
    node* link_add(node* head,int number);
    node* link_search (node *head ,int sequence);
	node* link_add(head,number);
    int link_sum(head);
}
//时间紧了，没时间写代码实现了。
```



```c
//linklist
#ifndef _NODE_H_
#define _NODE_H_

#include<stdio.h>
#include<stdlib.h>

typedef struct struct_node {
	int value;
	struct struct_node* next;
}node;		
node* link_add(node* head,int number){
	//add to linkedlist
	node* p = (node*)malloc(sizeof(node));
	p->value = number;
	p->next = NULL;
	//find the last
	node *last = head;
	if(last)
	{
	while(last->next){
		last = last->next;
	}last->next = p;//attach
	}else head = p; 
	printf("DONE:link_add\n");
	return head;
}

node* link_search (node *head ,int sequence){
	if(head){
		node *p = head;
		int i = 1;
		for (;i<sequence;i++){
			p = p ->next; 
			}
			printf("DONE:link_search\n");
			return p;
	}
	
}
node *link_delete(node *head,int number){
	node* p = head ;
	//find the node before the target
	for (;p->value;p= p->next){
		if ((p->next)->value == number){
			break;
		}}
	node *del = p->next ;
	p->next = (p->next)->next;
	free (del);
	printf("DONE:link_delete");
	return head;	
}
int link_sum(node *head){
	int sum =0;
	node *last = head;
	do{
		if(head){
		
		sum+= last->value;
		last = last->next;
		}
	}while(last!= NULL);
	printf("DONE:link_sum\n");
return sum;
}
int main(void){
	int number = 0;
	node* head =NULL;//why null? but not malloc?
	do{
	scanf("%d",&number);
	if(number != -1)
	{
	head = link_add(head,number);
	}
	}while (number != -1);
//	head=link_create(&head);
	printf("the sum of the linklist is %d\n",link_sum(head));
	//fine a member which had a sequence and get a pointer as an return
	int sequence = 0;
	printf("enter your sequence :");
	scanf("%d",&sequence);
//	printf("Debug");
	printf("\nHere is your result %d\n",link_search(head,sequence)->value);
	printf("please enter the number you want to delete:");
	int k = 0;
	scanf("%d",&k);
	head = link_delete(head,k);
	printf("\nenter your sequence again:");
	scanf("%d",&sequence);
//	printf("Debug");
	printf("\nHere is your result %d",link_search(head,sequence)->value);
	return 0;
}
#endif

```

## 5、面向对象的特点 OOP 
>> by alan kay

1.Everything is an object.
2.A program is a bunch of objects telling each other what to do by sending messages.
3.Each object has its own memory made up of other objects.
4.Every object has a type.
5.All objects of a particular type can receive the same messages.

## 6、用class实现最新的双向链表
``` c++
#ifndef LINK_H
#define LINK_H
#include "Node.h"
#include <iostream>
template <class T>
class List
{
public:
    List();//默认构造函数
    List(const List& ln);//拷贝构造函数
    ~List();//析构函数
    void add(T e);//向链表添加数据
    void ascSort();//升序排序
    void remove(T index);//移除某个结点
    T find(int index);//查找结点
    bool isEmpty();//判断是否为空
    int size();//链表长度
    void show();//显示链表
    void resShow();//链表反向显示
    void removeAll();//删除全部结点
private:
    Node<T> *head;
    Node<T> *tail;
    int length;
};
 
//
//默认构造函数
template <typename T>
List<T>::List()
{
    head = new Node<T>;
    tail = new Node<T>;
    head->next = tail;
    head->prev = nullptr;
    tail->next = nullptr;
    tail->prev = head;
    length = 0;
}
//拷贝构造函数
template <typename T>
List<T>::List(const List &ln)
{
    head = new Node<T>;
    head->prev = nullptr;
    tail = new Node<T>;
    head->next = tail;
    tail->prev = head;
    length = 0;
    Node<T>* temp = ln.head;
    while(temp->next != ln.tail)
    {
        temp = temp->next;
        tail->data = temp->data;
        Node<T> *p = new Node<T>;
        p->prev = tail;
        tail->next = p;
        tail = p;
        length++;
    }
    tail->next = nullptr;
}
//向链表添加数据
template <typename T>
void List<T>::add(T e)
{
    Node<T>* temp = this->tail;
    tail->data = e;
    tail->next = new Node<T>;
    Node<T> *p = tail;
    tail = tail->next;
    tail->prev = p;
    tail->next = nullptr;
    length++;
}
//查找结点
template <typename T>
T List<T>::find(int index)
{
    if(length == 0)
    {
        std::cout << "List is empty";
        return NULL;
    }
    if(index >= length)
    {
        std::cout << "Out of bounds";
        return NULL;
    }
    int x = 0;
    T data;
    Node<T> *p;
    if(index < length/2)
    {
        p = head->next;
        while (p->next != nullptr && x++ != index)
        {
            p=p->next;
        }
    } else{
        p = tail->prev;
        while(p->prev != nullptr && x++ != index)
        {
            p = p->prev;
        }
    }
    return p->data;
}
//删除结点
template <typename T>
void List<T>::remove(T index)
{
    if(length == 0)
    {
        std::cout << "List is empty";
        return ;
    }
    Node<T> *p = head;
    while(p->next!=nullptr)
    {
        p = p->next;
        if(p->data == index)
        {
            Node<T> *temp = p->prev;
            temp->next = p->next;
            p->next->prev = temp;
            delete p;
            length--;
            return ;
        }
    }
}
//删除所有结点
template <typename T>
void List<T>::removeAll()
{
    if(length == 0)
    {
        return ;
    }
    Node<T> *p = head->next;
    while(p != tail)
    {
        Node<T>* temp = p;
        p = p->next;
        delete temp;
    }
    head->next = tail;
    tail->prev = head;
    length = 0;
}
//升序排序
template <typename T>
void List<T>::ascSort()
{
    if(length <= 1) return;
    Node<T> *p = head->next;
    for (int i = 0; i < length-1; i++)
    {
        Node<T> *q = p->next;
        for (int j = i+1; j < length; j++)
        {
            if(p->data > q->data)
            {
                T temp = q->data;
                q->data = p->data;
                p->data = temp;
            }
            q=q->next;
        }
        p = p->next;
    }
}
//判断是否为空
template <typename T>
bool List<T>::isEmpty()
{
    if(length == 0)
    {
        return true;
    } else {
        return false;
    }
}
//链表长度
template <typename T>
int List<T>::size()
{
    return length;
}
//输出链表
template <typename T>
void List<T>::show()
{
    if(length == 0)
    {
        std::cout << "List is empty" << std::endl;
        return;
    }
    Node<T> *p = head->next;
    while (p != tail)
    {
        std::cout << p->data << " ";
        p = p->next;
    }
    std::cout << std::endl;
}
//反向输出链表
template <typename T>
void List<T>::resShow()
{
    if(length == 0)return;
    Node<T> *p = tail->prev;
    while (p != head)
    {
        std::cout << p->data << " ";
        p = p->prev;
    }
    std::cout << std::endl;
}
//析构函数
template <typename T>
List<T>::~List()
{
    if(length == 0)
    {
        delete head;
        delete tail;
        head = nullptr;
        tail = nullptr;
        return;
    }
    while(head->next != nullptr)
    {
        Node<T> *temp = head;
        head = head->next;
        delete temp;
    }
    delete head;
    head = nullptr;
}
 
#endif //TEST1_LINK_H
```

## 7、2021-9-27最新学习成果

*类的建构和结构函数*

1、由于类的初始化只能由类的成员函数完成，所以对类的初始化需要一个建构函数作为类的一个函数接口。比如：

```c++
class student
{
    private:
    float height;
    float weight;
    float BMI;
    
    public:
    //第一类建构函数default -- do nothing but to initialize the member;
    student1();
    //第二类建构函数赋值 --initialize the member with a specific value;
    student2(float i1,float i2);
    //第三类建构函数拷贝  --copy an identical member of the same class to initialize the new member;
    student3(student &anotherstudent);
    //析构函数
    ~student();
    
}
```

2、如果没有声明建构函数和析构函数，系统会自动生成3类default的建构函数和析构函数。但是会有风险

> 这里涉及到一个浅拷贝和深拷贝的问题：

> > 浅拷贝：简单的赋值拷贝操作
> >
> > 深拷贝：在堆中重新申请空间，进行拷贝操作

```c++
class Person {
public:
 //无参（默认）构造函数
 Person() {
  cout << "无参构造函数!" << endl;
 }
 //有参构造函数
 Person(int age ,int height) {
  
  cout << "有参构造函数!" << endl;

  m_age = age;
        //m_height是指向height的一个int型指针
  m_height = new int(height);
  
 }
 //拷贝构造函数  
 Person(const Person& p) {
  cout << "拷贝构造函数!" << endl;
  //如果不利用深拷贝在堆区创建新内存，会导致浅拷贝带来的重复释放堆区问题
  //因为使用new开的堆区，也一定要手动释放该区域，可以在析构函数中进行
        //但是，默认的构造函数进行的是浅拷贝，也就是说p1和p2的m_height指向的是同一个堆区
        //这样由于test01()中，p2先释放，堆区会变为空，那么p1再释放的时候就会报错
        //所以，一定要自己写拷贝构造函数，这样才会执行深拷贝，不会出错
        
        //使用new的时候，m_height必须是一个指针型的变量，
        //并且m_height的类型和new后面的int必须是一致的。
        //括号里就是开辟新的堆时，需要拷贝过去的变量
        m_age = p.m_age;
  m_height = new int(*p.m_height);
  
 }

 //析构函数
 ~Person() {
  cout << "析构函数!" << endl;
  if (m_height != NULL)
  {
   delete m_height;
  }
 }
public:
 int m_age;
 int* m_height;
};

void test01()
{
 Person p1(18, 180);

 Person p2(p1);
    //p1.m_height是指针，所以要再加一个*号取出他所指向的值。
 cout << "p1的年龄： " << p1.m_age << " 身高： " << *p1.m_height << endl;

 cout << "p2的年龄： " << p2.m_age << " 身高： " << *p2.m_height << endl;
}

int main() {

 test01();

 system("pause");

 return 0;
}
```

