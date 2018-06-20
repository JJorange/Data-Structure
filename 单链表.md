```c++
#include<stdlib.h>
#include<iostream>

using namespace std;

template<class T>
struct LinkNode	//定义成模板之后不能使用typedef
{
	T _data;//数据域
	LinkNode<T>* _pNext;//指针域

	LinkNode(LinkNode<T> * ptr = NULL)//仅初始化指针成员的构造函数
	{
		_pNext = ptr;
	}
	LinkNode(T& item, LinkNode<T>* ptr = NULL)
	{
		_data = item;
		_pNext = ptr;
	}
};


template<class T>
class List
{
public:
	List(){ pHead = new LinkNode<T>; }						//构造函数
	List(const T& x){ pHead = new LinkNode<T>(x); }			//构造函数
	List(List<T>& L);										//复制构造函数
	~List(){ makeEmpty(); }									//析构函数
	void makeEmpty();										//链表置空
	int Length()const;										//求长度
	LinkNode<T> *getHead()const{ return pHead; }			//返回附加头结点地址
	LinkNode<T> *Search(T x);								//搜索数据x的位置
	LinkNode<T> *Locate(int i);								//搜索第i个元素的地址
	bool getData(int i, T& x)const;							//取第i个元素的值
	void setData(int i, T& x);								//用x修改第i个元素的值
	bool Insert(int i, T& x);								//在第i个元素后插入x
	bool Remove(int i, T& x);								//删除第i个元素的值，x为元素的值
	bool IsEmpty()const										//判空，空true
	{
		return pHead->_pNext == NULL ? true : false;
	}
	bool IsFull()const{ return false; }						//判满，true满
	void Sort();											//排序
	void inputFront(T& end);								//头插输入
	void inputRear(T& end);											//尾插输入
	void output();											//输出
	List<T>& operator=(List<T>& L);							//重载函数：赋值
private:
	LinkNode<T> * pHead;									//链表头指针
};

//复制构造函数
template<class T>
List<T>::List(List& L)
{
	T value;
	LinkNode<T> * srcptr = L.getHead();
	LinkNode<T> * destptr = pHead = new ListNode<T>;

	while (srcptr->_pNext！ = NULL)
	{
		value = srcptr->_pNext->_data;
		destptr->_pNext = new ListNode<T>(value);
		destptr = destptr->_pNext;
		srcptr = srcptr->_pNext;
	}
	destptr->_pNext = NULL;
};


//链表置空
template<class T>
void List<T>::makeEmpty()
{
	LinkNode<T> * del;
	while (pHead->_pNext != NULL)
	{
		del = pHead->_pNext;
		pHead->_pNext = del->_pNext;
		delete del;
	}
};


//求单链表长度
template<class T>
int	List<T>::Length()const
{
	LinkNode<T>* q = pHead->_pNext;
	int count = 0;
	while (q != NULL)
	{
		q = q->_pNext;
		count++;
	}
	return count;
};

//找到第一个含x的值，没有返回NULL
template<class T>
LinkNode<T> * List<T>::Search(T x)
{
	//在表中搜索含数据x的结点，找到返回该节点地址，否则返回NULL
	LinkNode<T> * current = pHead->_pNext;
	while (current != NULL)
	{
		if (current->_data == x)
			break;
		else
			current = current->_pNext;
	}
	return current;
};

//搜索第i个元素的地址
template<class T>
LinkNode<T> * List<T>::Locate(int i)
{
	//i<0或i超出表中结点数，返回NULL
	if (i < 0)
		return NULL;
	LinkNode<T> * current = pHead;
	int k = 0;
	while (current != NULL && k < i){
		current = current->_data;
		k++;
	}
	return current;
}

//取第i个元素的值
template<class T>
bool List<T>::getData(int i, T& x)const
{
	if (i <= 0)
		return false;
	LinkNode<T> * current = Locate(i);
	if (current == NULL)
		return false;
	else
	{
		x = current->_data;
		return true;
	}
}

//用x修改第i个元素的值
template<class T>
void List<T>::setData(int i, T& x)
{
	if (i < 0)
		return;
	LinkNode<T> * current = Locate(i);
	if (current == NULL)
		return;
	else
	{
		current->_data = x;
	}
}

//在第i个元素后插入x
template<class T>
bool List<T>::Insert(int i, T& x)
{
	if (i < 0)
		return false;
	LinkNode<T> * current = Locate(i);
	if (NULL == current)
		return false;
	LinkNode<T> * newnode = new LinkNode<T>(x);
	if (newnode == NULL)
	{
		cerr << "内存分配错误" << endl;
		exit(1);
	}
	newnode->_pNext = current->_pNext;
	current->_pNext = newnode;
	return true;
}

//删除第i个元素的值，x为元素的值
template<class T>
bool List<T>::Remove(int i, T& x)
{
	if (i <= 0)
		reuturn false;
	LinkNode<T> * current = Locate(i - 1);
	if (current == NULL || current->_pNext == NULL)
		return false;
	LinkNode<T> * del = current->_pNext;
	if (del == NULL)
		return false;
	current->_pNext = del->_pNext;
	x = del->_data;
	delete del;
	return true;
}

//输出到屏幕上
template<class T>
void List<T>::output()
{
	LinkNode<T> * current = pHead->_pNext;
	while (current != NULL)
	{
		cout << current->_data << "--->";
		current = current->_pNext;
	}
	cout << "NULL" << endl;
}

//重载函数：赋值
template<class T>
List<T>& List<T>::operator=(List<T>& L)
{
	T value;
	LinkNode<T> * srcptr = L.getHead();//被复制表的附加头结点地址
	LinkNode<T> * desptr = new LinkNode<T>;
	while (srcptr->_pNext != NULL)//逐个结点复制
	{
		value = srcptr->_pNext->_data;
		desptr->_pNext = new LinkNode<T>(value);
		srcptr = srcptr->_pNext;
		desptr = desptr->_pNext;
	}
	desptr->_pNext = NULL;
	return *this;
}


//头插输入
template<class T>
void List<T>::inputFront(T& end)
{
	//end是约定的序列结束的标志
	//输入序列为正整数，end可以为0或者负整数
	//输入序列为字符，end可以为"\0"
	LinkNode<T> * newNode;
	T val;
	makeEmpty();
	cin >> val;
	while (val != end)
	{
		newNode = new LinkNode<T>(val);//创建新结点
		if (newNode == NULL)
		{
			cerr << "存储分配错误！" << endl;
			exit(1);
		}
		newNode->_pNext = pHead->_pNext;
		pHead->_pNext = newNode;
		cin >> val;
	}

}

//尾插输入
template<class T>
void List<T>::inputRear(T& end)
{
	//end是约定的序列结束的标志
	//输入序列为正整数，end可以为0或者负整数
	//输入序列为字符，end可以为"\0"
	LinkNode<T> * newNode;
	LinkNode<T> * last = pHead;
	T val;
	makeEmpty();
	cin >> val;
	while (val != end)
	{
		newNode = new LinkNode<T>(val);//创建新结点
		if (newNode == NULL)
		{
			cerr << "存储分配错误！" << endl;
			exit(1);
		}
		last->_pNext = newNode;
		last = last->_pNext
		cin >> val;
	}
	last->_pNext = NULL;
};


int main()
{
	int i = 0;
	List<int> L;
	L.inputFront(i);
	L.output();
	return 0;
}
```
