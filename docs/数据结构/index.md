# 数据结构 + 算法 = 程序

> ## 数据结构

> 
> 数据结构：数据与数据的关系
> 
> 1.逻辑结构：集合结构，线性结构，树形结构，图形结构
> 
> 3.物理结构：连续：数组，链式：链表

> ## 算法
> 求解问题的步骤
> 
> 算法的特征：
> 
> 1.输入项：一个算法有0个或多个输入
> 
> 2.输出项：一个算法有一个或多个输出
> 
> 3.有穷性：必须能在执行有限个步骤之后终止
> 
> 4.确切性：每⼀步骤必须有确切的定义
> 
> 5.可⾏性：每个计算步都可以在有限时间内完成（也称之为有效性）
> 
> 6.可读性：算法要便于阅读、理解和交流
> 
> 7.健壮性：算法不应该得到莫名其妙的结果
> 
> 8.性价比：利用最少的资源得到满足要求的结果
> 
 ##时间复杂度
> 是算法运行后对时间需求量的定性描述  一条语句为 O(1)


##空间复杂度
> 算法运行后对空间需求量的定性描述
> 
> 通常使用S（n）表⽰算法的空间复杂度。通常情况下，算法的时间复杂度更受关注。可以通过增加额外空间降
低时间复杂度

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




# 树

> 一种数据结构  n个节点的有限集
> 

>  n = 0 时  为空树
> 
> n>0时，有限集的元素构成一个具有层次感的数据结构
> 
> 特点：
> 
> 1. n>0时，根节点是唯⼀的，不可能存在多个根节点。
> 
> 2. 每个节点有零个至多个子节点；除了根节点外，每个节点有且仅有一个父节点。根节点没有父节点。


# 根
> 有且仅有一个结点的非空树，那个结点就是根。

# 子树

> 在一棵非空树中，除根外，其余所有结点可以分为m（m≥0）个互不相交的集合。每个集合本身又是一棵树，
称为根的子树。

# 结点
> 包含一个数据元素及若干指向子树的分支

#孩子结点和双亲结点
> 结点的子树的根称为该结点的孩子，B结点是A结点的孩子，则A是B的双亲结点

#兄弟结点和堂兄结点
> 同一双亲的孩子结点称为兄弟结点，同一层上结点称为堂兄结点

# 祖先结点
> 从根到该结点的所经分支上的所有结点

#子孙结点
> 以某结点为根的子树中任一结点称为该结点的子孙结点

# 叶子结点
> 终端结点 是度为0（无子树）的节点

# 分支节点
> 除叶子结点之外的结点

# 内部结点
> 除根之外的分支节点

# 度
> 结点拥有的子树的数量为结点的度，树的所有结点中度的最大值为树的度






# 排序


## 冒泡排序  

> 1.比较相邻的元素，若元素大小关系不符合，交换他们两个
> 
> 2.对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对
> 

> 时间复杂度：O(n^2)



> 版本1：
> 外层循环 比较len 轮   内层循环 为相邻两个元素的比较 

    void  BubbleSort(int *arr,int len)
    {
    	for (size_t i = 0; i < len; ++i)
    	{
    		for (size_t j = 0; j < len - 1; ++j)
    		{
    			if (arr[j + 1] < arr[j])
    			{
    				int temp = arr[j + 1];
    				arr[j + 1] = arr[j];
    				arr[j] = temp;
    			}
    		}
    	}
    }


> 版本2：
> 外层 每循环一次  都有一个已经排好了序的元素  

    void  BubbleSort(int *arr,int len)
    {
    	for (size_t i = 0; i < len; ++i)
    	{
    		for (size_t j = 0; j < len -1 - i; ++j)
    		{
    			if (arr[j + 1] < arr[j])
    			{
    				int temp = arr[j + 1];
    				arr[j + 1] = arr[j];
    				arr[j] = temp;
    			}
    		}
    	}
    }


> 版本3：
> 若在一次外层循环中  发现没有相邻元素交换 说明 所有 元素已经是有序的

    void  BubbleSort(int *arr,int len)
    {
    	bool flag = false;
		for (size_t i = 0; i < len; ++i)
		{
			flag = false;
			for (size_t j = 0; j < len - i - 1; ++j)
			{
				if (arr[j + 1] < arr[j])
				{
					flag = true;
					int temp = arr[j + 1];
					arr[j + 1] = arr[j];
					arr[j] = temp;
				}
			}
			if (!flag)
			{
				break;
			}
		}
    }

## 快速排序

> 1.从待排序序列中挑出一个元素，作为”基准”
> 
> 2.把所有比基准值小的元素放在基准前面，所有比基准值大的元素放在基准的后面（相同的数可以到任一边）,这个过程叫分组或分区
> 
> 3.递归地把”基准值前面的子数列”和”基准值后面的子数列”进行排序。不多于一个为止


    /*
    1.选基准  一般选第一个 或 最后一个
    
    */
    
    void QuickSort(int *arr, int left,int right)
    {
    	if (left<right)
    	{
    		int _p = arr[left];//基准
    		int _l = left;
    		int _r = right;
    		while (_l<_r)
    		{
    			//从右边找  直到找到  小于基准值
    			while (_l<_r && arr[_r]>_p)
    			{
    				--_r;
    			}
    			// 放在左边
    			if (_l < _r)
    			{
    				arr[_l++] = arr[_r];
    			}
    			//从左边找 直到找到  大于基准值
    			while (_l<_r && arr[_l]<_p)
    			{
    				++_l;
    			}
    			// 放在右边
    			if (_l < _r)
    			{
    				arr[_r--] = arr[_l];
    			}
    		}
    		// 此时 _l == _r   吧基准值放在这里  此时 左边的小于基准值  右边的大于基准值  对于基准值来说是有序的
    		arr[_r] = _p;
    		// 左边的一部分 递归 对于某个数  又是有序的
    		QuickSort(arr, left, _l - 1);
    		// 右边的一部分 递归 对于某个数  又是有序的
    		QuickSort(arr, _l + 1, right);
    	}
    }


## 插入排序
> 1.从第一个元素开始，该元素可以认为已经被排序；


> 2.取出下一个元素，在已经排序的元素序列中从后向前扫描；


> 3.如果该元素（已排序）大于新元素，将该元素移到下一位置；


> 4.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；


> 5.将新元素插入到该位置后；


> 6.重复步骤2~5。
    
    void InsertSort(int *arr, int len)
    {
    	int temp;
    	int j;
    	for (size_t i = 1; i < len; ++i)
    	{
    		temp = arr[i];
    		j = i - 1;
    		while (j>=0 && arr[j] > temp)
    		{
    			arr[j + 1] = arr[j];
    			j--;
    		}
    		arr[j + 1] = temp;
    	}
    }
    

## 选择排序
> 1.未排序的数列中找到最小(or最大)元素，然后将其存放到数列的起始位置；


> 2.从剩余未排序的元素中继续寻找最小(or最大)元素，然后放到已排序序列的末尾；


> 3.以此类推，直到所有元素均排序完毕；

    void SelectSort(int *arr, int len)
    {
    	size_t temp;
    	for (size_t i = 0; i < len -1; ++i)
    	{
    		temp = i;
    		// 循环找当前最小值
    		for (size_t j = i+1; j < len ; ++j)
    		{
    			if (arr[temp] > arr[j])
    			{
    				temp = j;
    			}
    		}
    		// 交换值
    		swap(arr[temp], arr[i]);
    	}
    }


## 希尔排序
> 部分有序  再排序     
> 
> 通过步长 使得部分已经有序   最后使用插入排序
> 
> 插入排序的优化版

    
    void ShellSort(int *arr, int len)
    {
    	int temp;
    	int j;
    	int gap;
    	for (gap = len / 2; gap>0; gap/=2)
    	{
    		for (size_t i = gap; i < len; ++i)
    		{
    			temp = arr[i];
    			j = i - gap;
    			while (j >= 0 && arr[j] > temp)
    			{
    				arr[j + gap] = arr[j];
    				j -= gap;
    			}
    			arr[j + gap] = temp;
    		}
    	}
    	
    }

# 堆
> 一颗完全二叉树
> 
> 大根堆 ： 根大于孩子结点
> 
> 小根堆 ： 根小于孩子结点


# 哈希表
> 通过散列函数  算出  数据存储的位置 使之达到 O(1) 级的访问效率


	|  姓名| 班级 |  成绩
	
	|  张三| 1|  70
	
	|  李四| 2|  86
	
	哈希表：

	|a   |b   |c   |d   |e   |f   |g   |h   |i   |j   |k 


	|0   |1   |2   |3   |4   |5   |6   |7   |8   |9   |10

	-------------------------------------------------------
	|l   |m   |n   |o   |p   |q   |r  |s   |t   |u   |v     
	
	|11  |12  |13  |14  |15  |16  |17 |18  |19  |20  |21

	-------------------------------------------------------
	|w   |x   |y   |z
	
	|22  |23  |24  |25

	
	//张三  姓名首字母为 z 和 s  对比哈希表得出 25 + 18 = 43； 算出这一条数据的存储位置
	//那么  arr[43]  =  70/*张三成绩*/

	//李四  姓名首字母为 l 和 s  对比哈希表得出 11 + 18 = 29；算出这一条数据的存储位置
	//那么  arr[29]  =  86/*李四成绩*/

# 哈希冲突

> 两个以上的键值映射到同一个索引
> 
> 通过散列函数 算出数据存储位置  得到高效率的访问效率，但不可避免的产生了哈希冲突 
> 
> 哈希冲突指两个独立的个体（数据） 通过 散列函数 得到相同的结果（存储位置），比如 下面的例子

	
	|  姓名 |  班级   |  成绩
		
	|  李斯 |  3     |  75

	//李斯  姓名首字母为 l 和 s  对比哈希表得出 11 + 18 = 29；算出这一条数据的存储位置
	//那么  arr[29]  =  75/*李斯成绩*/

> 这个例子得到的结果和上面李四的结果是一致的  李斯和李四的存储位置都是29  。 但一个位置不会存两份数据  。 这就是哈希冲突

> 
> 哈希冲突是不可避免的 需要解决

# 解决哈希冲突的方式

> 1.开放定址法：产生冲突时，先从该位置后面找，找到空位，再填入数据 并记录这个位置
> 
> 需要大于数据量的空间
> 
> 2.再哈希法：发生冲突时 使用第二个、第三个散列函数 、第四个散列函数 算位置 直到没有冲突
> 
> 3.链地址法（常用）：相同位置冲突的形成一个链表  一个冲突指向下一个冲突
> 
> 4.选取恰当的散列函数 减少哈希冲突  

# 散列函数
>选取：
>
> 1.尽可能平均分布在散列表
> 
> 2.关键字极小的变化引起哈希值极大的变化




> 方法：

> 1.除法散列法  ：
> 
> f (key) = value%p;
> 
> p 为 0-value的最大质数

> 
> 2.平方散列法  ：
> 
> f (key) = value*value >> d ;
> 
> d 为一整数 保证数字不溢出
> 
> 2.斐波那契散列法  ：
> 

> 
> 16       Fibonacci = 40503
> 
> 32       Fibonacci = 2654435769
> 
> 64       Fibonacci = 11400714819323198485

> f (key) = value*Fibonacci >> 28 ;



