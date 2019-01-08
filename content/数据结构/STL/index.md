# STL

# STL  Standard Template Library 标准模板库
> 序列式容器：vector,deque,list 
> 
> 是一种各元素之间有顺序关系的线性表
> 
> 关联式容器：set,map,multiset,multimap
> 
> 是非线性的树结构

# 迭代器
> 一个所谓的智能指针，具备遍历复杂数据结构的能力，可遍历STL容器内全部或部分元素的对象，
用来指出容器中的一个特定的位置

# vector 向量
> 头文件： `#include <vector>`
> 
> 使用：


	int main()
	{
		
		vector<int> vec1;//定义一个int型的向量
	
		vector<int> vec2(5);//定义一个初始化大小为5的int型向量
	
		vector<int> vec3(5,1);//定义一个初始化大小为5值为1的int型向量
	
		vector<int> vec4 = vec3;//拷贝构造
		vector<int> vec5(vec3);//拷贝构造
	
		int arr[5] = { 1, 2, 3, 4, 5 };
	
		vector<int> vec6(arr, arr + 5);
		vector<int> vec7(&arr[0], &arr[5]);
	
		vec1.push_back(1);//末尾添加元素
		vec1.push_back(2);//末尾添加元素
		vec1.push_back(3);//末尾添加元素
	
		vec1.pop_back();//删除最后一个元素
	
		cout << *vec1.begin() << endl;//vec1.begin()  返回开始迭代器  
		cout << *(vec1.end()-1) << endl;//vec1.end()  指向最后一个元素的下一个位置
	
		cout << vec1[0] << endl;//下标访问  不检查越界
		cout << vec1.at(0)<< endl;// 检查越界
	
		cout << vec1.front() << endl;// 访问第一个元素 不检查元素是否存在
		cout << vec1.back() << endl;// 访问最后一个元素 不检查元素是否存在
	
	
		int *p = vec1.data(); //返回指向数组的指针  c++11
	
		vec1.clear();//清空数据  清空栈区对象
	
		for (size_t i = 0; i < 10; i++)
		{
			vec1.push_back(i);
		}
	
		//遍历向量的数据
		for (vector<int>::iterator it = vec1.begin(); it != vec1.end(); ++it)
		{
			cout << *it << endl;
		}
	
		cout << "实际元素个数" << endl;
		cout << vec1.size() << endl;//实际元素个数
	
		cout << "元素翻转" << endl;
		//元素翻转
		reverse(vec1.begin(), vec1.end());
		for (vector<int>::iterator it = vec1.begin(); it != vec1.end(); ++it)
		{
			cout << *it << endl;
		}
		//排序
		sort(vec1.begin(), vec1.end());//需要头文件 #include <algorithm> 算法库
	
		cout << "排序" << endl;
		for (vector<int>::iterator it = vec1.begin(); it != vec1.end(); ++it)
		{
			cout << *it << endl;
		}
	
		vec2 = { 1, 2, 3, 4, 5 };//c++11
	
		//交换数据
		vec1.swap(vec2);
	
		//判空
		vec1.empty();//空真，非空假
	
		//扩容
		vec3.reserve(10);
		vec3.reserve(2);//不能改小
	
		vec3.assign(3, 5);//改变存放个数和值
	
		vec3.resize(12, 6);//扩实际个数，原来的值不变，多的赋值为6
	
		return 0;
	}



> 简单源码实现：


	template <typename T>
	class CMyVector
	{
	public:
		typedef T* iterator;
		typedef const T* const_iterator;
		//构造
	public:
		//单参，无参构造
		explicit CMyVector(int capacity = 0)
		{
			if (capacity > 0)
			{
				pBuff = new T[capacity];
				memset(pBuff, 0, sizeof(T)*capacity);
			}
			else
			{
				pBuff = nullptr;
			}
	
			m_capacity = capacity;
			m_size = capacity;
			
			
			
		};
		//双参构造
		CMyVector(int capacity, T const& data)
		{
			
	
			if (capacity > 0)
			{
				pBuff = new T[capacity];
			}
			else
			{
				pBuff = nullptr;
			}
	
			m_capacity = capacity;
			m_size = capacity;
	
			for (int i = 0; i < m_size; ++i)
			{
				pBuff[i] = data;
			}
	
		}
		//拷贝构造
		CMyVector(CMyVector<T> const& that)
		{
			this->m_capacity = that.m_capacity;
			this->m_size = that.m_size;
			if (this->m_capacity != 0)
			{
				this->pBuff = new T[this->m_capacity];
				//拷贝内存
				memcpy(this->pBuff,that.pBuff, sizeof(T)*this->m_size);
			}
			else
			{
				this->pBuff = nullptr;
			}
			
		}
		//拷贝赋值
		CMyVector<T>& operator=(CMyVector<T> const& that)
		{
			if (&that != this)//避免自赋值
			{
				T* p_tmp = new T[that.m_capacity];//申请内存
				memcpy(p_tmp, that.pBuff, sizeof(T)*that.m_size);//拷贝资源
				clear();//释放旧资源 避免内存泄漏
				this->pBuff = p_tmp;//接管资源
	
				this->m_capacity = that.m_capacity;
				this->m_size = that.m_size;
			}
			return *this;//返回自引用
		}
		~CMyVector()
		{
			clear();
		}
		//成员变量
	private:
		T      *pBuff;//动态数组首地址
		int m_capacity;//容量
		int m_size;//实际元素个数
		//运算符重载
	public:
		bool operator==(CMyVector<T> const& that) const
		{
			if (this->m_size != that.m_size)
			{
				return false;
			}
			for (int i = 0; i < this->m_size; ++i)
			{
				if (pBuff[i] != that.pBuff[i])
				{
					return false;
				}
			}
			return true;
		}
		bool operator!=(CMyVector<T> const& that) const
		{
			return !(*this==that)
		}
		T& operator[](int index) const
		{
			//不检查越界
			return pBuff[index];
		}
		//成员函数
	public:
		iterator begin()
		{
			return pBuff;
		}
		iterator end()
		{
			return pBuff + m_size;
		}
		const_iterator begin() const
		{
			return pBuff;
		}
		const_iterator end()const
		{
			return pBuff + m_size;
		}
		T& at(int index) const
		{
			//检查越界抛异常
			if (index >= 0 && index < m_size)
			{
				return pBuff[index];
			}
			throw out_of_range("out_of_range");
		}
		void assign(int n, T const& data)
		{
			clear();
			m_capacity = n;
			m_size = n;
			pBuff = new T[n];
			for (int i = 0; i < n; ++i)
			{
				pBuff[i] = data;
			}
		};
		void swap(CMyVector<T> & that)
		{
			CMyVector<T> temp;
			temp = *this;
			*this = that;
			that = temp;
		};
		void reserve(int n)
		{
			if (n > m_capacity)
			{
				T* temp = new T[n];
				memcpy(temp, pBuff, sizeof(T)*m_size);
				if (pBuff)
				{
					delete[] pBuff;
				}
				pBuff = temp;
				m_capacity = n;
	
			}
		};
		void resize(int n)
		{
			if (n > m_size)
			{
				reserve(n);
				memset(&pBuff[m_size], 0, sizeof(T)*(n - m_size));
			}
			m_size = n;
		};
		void resize(int n, T const& data)
		{
			size_t tempSize = m_size;
			resize(n);
			for (size_t i = tempSize; i < n; ++i)
			{
				
				pBuff[i] = data;
			}
		};
	
		T& front()
		{
			return pBuff[0];
		}
		T& back()
		{
			return pBuff[m_size -1];
		}
	
		void push_back(T const& data)
		{
			if (m_capacity == m_size)
			{
				//扩容
				reserve((m_capacity+1)*2);
			}
			pBuff[m_size++] = data;
		}
		void pop_back()
		{
			if (m_size > 0)
			{
				m_size--;
			}
		}
	
		void clear()
		{
			if (pBuff)
			{
				delete[] pBuff;
				pBuff = nullptr;
				m_size = 0;
				m_capacity = 0;
			}
			
		}
	
		int size()
		{
			return m_size;
		};
		int capacity()
		{
			return m_capacity;
		};
		bool empty()
		{
			return m_size == 0;
		};
	};


# list 
> 头文件： `#include <list>`

> 使用：

	/*
	list  环状双向链表
	
	每一个节点分为 ： 数据域  前驱指针  后驱指针
	
	随机插入 ： O(1)
	
	随机访问：  O(n)
	
	
	
	*/
	
	bool foo(int a)
	{
		return a % 2 == 0;
	}
	
	#if 0
	
	int main()
	{
	
	
		list<int> list1;
		list<int> list2(5);//5个节点的链表  数据默认为0
	
	
		list<int> list3(3,6);//3个节点的链表  数据为6
	
	
		list<int> list4(list3);
	
		list<int> list5 = (list3);
	
		list1.push_back(20);
		list1.push_back(20);
		list1.push_back(20);
		list1.pop_back();
	
		list1.push_back(5);
		list1.push_back(6);
		list1.push_back(50);
	
		list1.sort();//排序
	
	
	
		//merge  合并并排序
	
		list1.clear();
		list2.clear();
	
		list1.push_back(3);
		list1.push_back(6);
	
	
		list2.push_back(4);
		list2.push_back(7);
	
		list1.merge(list2);
	
		//remove()  清除指定值
	
		list1.remove(3);
	
		//remove_if()  清除满足条件的值  参数是返回bool的函数指针
		//list1.remove_if(foo);
		list1.remove_if([](int x){return x == 7; });
	
	
		// splice()  切割
	
		// unique()  删除连续的重复值  保证唯一
		list1.push_back(4);
		list1.push_back(4);
		list1.push_back(4);
		list1.push_back(4);
		list1.push_back(4);
	
		list1.unique();
	
		for (list<int>::iterator it = list1.begin(); it != list1.end(); ++it)
		{
			cout << *it;
		}
		cout << endl;
	
		return 0;
	}
	
	#endif

> 简单源码实现：


	#pragma once
	#include <memory>
	
	using namespace std;
	
	/*
	
	list  节点
			*prev
			*next
			data
	*/
	
	template <typename T>
	struct _list_node
	{
		_list_node*		prev;
		_list_node*		next;
		T				data;
	};
	
	
	/*
	list iterator
	
	
	
	*/
	
	template <typename T>
	class _list_iterator
	{
	public:
		typedef _list_iterator<T>			self;
		typedef _list_node<T>*				link_type;
	
	public:
		link_type							ptr;  // 指向list节点的指针  头指针
	
	public:
		_list_iterator(link_type p = nullptr)
		{
			ptr = p;
		}
	
		_list_iterator(self const& that)
		{
			ptr = that.ptr;
		}
	
		bool operator==(self const& that)
		{
			return ptr == that.ptr;
		}
	
		bool operator!=(self const& that)
		{
			return ptr != that.ptr;
		}
	
		T& operator*() const
		{
			return ptr->data;
		}
	
		T* operator->()const
		{
			return &(ptr->data);
		}
		//前++
		self& operator++()
		{
			ptr = ptr->next;
			return *this;
		}
		//后++
		self& operator++(int)
		{
			self temp(*this);
			++*this;
			return temp;
		}
	
		//前--
		self& operator--()
		{
			ptr = ptr->prev;
			return *this;
		}
		//后--
		self& operator--(int)
		{
			self temp(*this);
			--*this;
			return temp;
		}
	};
	
	/*
	
	list
	
	
	
	
	*/
	
	
	template <typename T>
	class MyList
	{
	protected:
		 typedef _list_node<T>					list_node;
		 typedef T								value_type;
		 typedef T*								pointer;
		 typedef const T*						const_pointer;
		 typedef T&								reference;
	
		 //空间配置器
	
		 ; allocator<list_node>   nodeAllcator;
	
	public:
		typedef list_node*						link_type;
		typedef _list_iterator<T>				iterator;
	
		/*
		分配内存
		*/
	protected:
		link_type alloc_node()
		{
			return nodeAllcator.allocate(sizeof(list_node));
		}
	
		link_type alloc_dtor_node(const T& value)
		{
			link_type p = alloc_node();
			nodeAllcator.construct(&(p->data), value);
			return p;
		}
	
		void dealloc_node(link_type p)
		{
			nodeAllcator.deallocate(p);
		}
	
		void dealloc_all_node(link_type p)
		{
			destroy(p->data);
			dealloc_node(p);
		}
	
		/*
		
		构造函数
		*/
	public:
		MyList()
		{
			node = alloc_node();
			node->prev = node;
			node->next = node;
		};
	
	private:
		link_type								node;//指向list节点
	
	
	public:
		iterator begin()
		{
			return node->next;
		}
	
		iterator end()
		{
			return node;
		}
	
		iterator cbegin()const
		{
			return node->next;
		}
	
		iterator cend()const
		{
			return node;
		}
	
		bool empty()
		{
			return node == node->next;
		}
	
		iterator insert(iterator pos,const T& value)
		{
			link_type p = alloc_dtor_node(value);
	
	
			//先连接
			p->next = pos.ptr;
			p->prev = pos.ptr->prev;
	
			pos.ptr->prev->next = p;
			pos.ptr->prev = p;
	
			return p;
		}
	
		void push_back(const T& value)
		{
			insert(end(), value);
		}
	
	};