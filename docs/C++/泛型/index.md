# 泛型

# 泛型 
> 泛型允许强类型语言定义一部分可变部分，这部分在使用时必须声明
> 
> 通过二次编译实现
> 

> 函数模板 ： 类型参数化

    #include <iostream>
    
    using namespace std;
    
    template<typename T>
    T max(T a, T b)
    {
    	return a > b ? a : b;
    }
    
    
    int main()
    {
    
    	cout << max<int>(11, 22) << endl;//22
    	cout << max<double>(11.11, 22.22) << endl;//22.22
    	return 0;
    }

> 模板的隐式推断

    #include <iostream>
    
    using namespace std;
    
    template<typename T>
    T max(T a, T b)
    {
    	cout << typeid(T).name() << endl;
    	return a > b ? a : b;
    }
    
    
    int main()
    {
    
    	cout << max(11, 22) << endl;//这里尖括号不传类型名  编译器通过参数值推断为int
    	cout << max(11.11, 22.22) << endl;//这里尖括号不传类型名  编译器通过参数值推断为double
    	cout << max("hello", "would") << endl;//这里尖括号不传类型名  编译器通过参数值推断为 char const*  所以这里是比较两个地址的大小， 并不是比较两个字符串的大小
    	return 0;
    }

> 函数模板的重载

> 编译器优先选择普通的具体的函数

    #include <iostream>
    
    using namespace std;
    
    template<typename T>
    T max( T a,  T b)
    {
    	cout << "T 版本 重载的模板函数" << endl;
    	return a > b ? a : b;
    }
    
    template<typename T>
    T* max(T* const a, T* const b)
    {
    	cout << "T* 版本 重载的模板函数" << endl;
    	return *a > *b ? a : b;
    }
    
    
    const char* const& max(const char* const& a, const char* const& b)
    {
    	cout << "const char* 版本 普通函数" << endl;
    	return strcmp(a, b) > 0 ? a : b;
    }
    
    
    int main()
    {
    
    	const char* a = "abc";
    	const char* b = "abd";
    
    	cout << max(a, b) << endl;//普通函数
    	cout << max<const char*>(a, b) << endl;// T 版本 重载的模板函数
    	cout << max<const char>(a, b) << endl;//T* 版本 重载的模板函数
    	return 0;
    }

> 类模板


    #include <iostream>
    
    using namespace std;
    template<typename T>
    class Ldc
    {
    public:
    	Ldc(T data)
    	{
    		m_data = data;
    	};
    private:
    	T m_data;
    public:
    	void print()
    	{
    		cout << m_data << endl;
    	}
    };
    
    int main()
    {
    	Ldc<int> ldc(20);
    	ldc.print();//20
    	return 0;
    }

> 类模板所有非静态成员函数都是函数模板
> 
> 类外定义成员函数需要带上 `template <typename T>`

> 类模板的成员函数 声明和定义不可以分开，必须在同一个文件中


> 

> 友元函数的声明和定义分开 需要增加泛型支持

    template<typename T>
    class Ldc;
    
    template<typename T>
    void lookData(Ldc<T> const& that)
    {
    	cout << that.m_data << endl;
    }
    
    template<typename T>
    class Ldc
    {
    public:
    	friend void lookData <T>(Ldc<T> const& that);
    	Ldc(T data)
    	{
    		m_data = data;
    	};
    private:
    	T m_data;
    
    };
    
    
    
    int main()
    {
    	Ldc<int> iLdc(20);
    	lookData(iLdc);//20
    
    	Ldc<char> cLdc('a');
    	lookData(cLdc);//'a'
    	return 0;
    }

> 模板类的继承

> 1.父类自定义了构造函数，子类必须用构造函数列表来初始化
> 
> 2.继承时，父类是模板类，子类不是模板类，则必须指定继承父类的类型，这样才能知道父类对象的大小。


    template <typename T>
    class Parent
    {
    public:
    	Parent(T data)
    	{
    		m_data = data;
    	}
    
    private:
    	T m_data;
    
    public:
    	void lookData()
    	{
    		cout << m_data << endl;
    	}
    };
    
    class Son :public Parent<int>/*必须指定类型*/
    {
    public:
    	Son(int data) :Parent(data){};/*必须使用父类构造函数来初始化*/
    private:
    
    };
    
    
    
    int main()
    {
    	Son son(20);
    	son.lookData();//20
    	return 0;
    }

> 3.如果子类是模板类，要么指定继承父类的类型，要么用子类的泛型来指定父类的类型


    template <typename T>
    class Parent
    {
    public:
    	Parent(T data)
    	{
    		m_data = data;
    	}
    
    private:
    	T m_data;
    
    public:
    	void lookData()
    	{
    		cout << m_data << endl;
    	}
    };
    
    
    template <typename T>
    class Son :public Parent<T>/*用子类的泛型来指定类型*/
    {
    public:
    	Son(int data) :Parent<T>(data){};
    private:
    
    };
    
    
    
    int main()
    {
    	Son<int> son(20);
    	son.lookData();//20
    	return 0;
    }

> 模板特化
> 
> 对普通版本的补充
