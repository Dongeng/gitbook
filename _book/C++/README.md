# 名字空间

>定义：namespace 空间名
>
>使用：
>
>1.using namespace 空间名
>
>2.作用域限定符 ::
>
>作用：
>
>1.划分逻辑区域
>
>2.解决名字冲突
>
>示例代码：

       #include <iostream>
    
    
    #if 1
    namespace ldc
    {
    	char* name = "ldc";
    }
    namespace lsc
    {
    	char* name = "lsc";
    }
    
    
    using namespace std;
    int main()
    {
    	using namespace ldc;
    	cout << name << endl;
    	cout << ldc::name << endl;
    	cout << lsc::name << endl;
    	return 0;
    }
    #endif

>名字空间合并：

    #if 1
    namespace ldc
    {
    	char* name = "ldc";
    }
    namespace ldc
    {
    	char* showName()
    	{
    		return name;
    	}
    }
    
    
    using namespace std;
    int main()
    {
    	cout << ldc::name << endl;
    	cout << ldc::showName() << endl;
    	return 0;
    }
    #endif
>名字空间嵌套：

    #if 1
    namespace ldc
    {
    	char* name = "ldc";
    	namespace son
    	{
    		char* name = "lp";
    	}
    }
    
    
    using namespace std;
    int main()
    {
    	cout << ldc::son::name << endl;
    	return 0;
    }
    #endif
>空间别名

    #if 1
    namespace ldc
    {
    	char* name = "ldc";
    	namespace son
    	{
    		char* name = "lp";
    	}
    }
    
    
    using namespace std;
    int main()
    {
    	cout << ldc::son::name << endl;
    	namespace son = ldc::son;//别名
    	cout << son::name << endl;
    	return 0;
    }
    #endif
>using namespace 空间名;作用是把名字空间内的定义暴露出来，相当于在使用的地方来定义，这样使用是不可逆转的，即已经暴露的不可再隐藏。

# 结构，联合，枚举
>都可以省略关键字了
>
>C++结构支持函数成员

    #if 1
    
    using namespace std;
    struct student
    {
    	char* name;
    	void showName()
    	{
    		cout << name << endl;
    	}
    };
    int main()
    {
    	student s = { "ldc" };
    	s.showName();
    	return 0;
    }
    
    #endif
>匿名联合

    #if 1
    using namespace std;
    
    
    int main()
    {
    	union
    	{
    		int i;
    		char c;
    	};
    	i = 65;
    	cout << c << endl;//A
    	return 0;
    }
    
    #endif
>枚举

    #if 1
    using namespace std;
    enum Color
    {
    	RED,//1
    	BLUE,//2
    	BLACK//3
    };
    
    int main()
    {
    	Color c = RED;
    	return 0;
    }
    
    #endif

# bool

>bool类型只有两个值，true(真)和false(假);本质为一个字节的1和0
>
>基本类型的值可隐式转换bool类型的值。非0即1，非空即真,指针为NULL为假

# 内联函数
>用已经编译好的二进制代码，替换函数的调用，避免调用的开销
>
>这是期望的优化，是否内联由编译器决定
>
>缺点是占内存，递归不可以使用内联


    #if 1
    using namespace std;
    
    inline int max(int a, int b)
    {
    	return a > b ? a : b;
    }
    
    int main()
    {
    	cout << max(1, 2) << endl;
    	return 0;
    }
    
    #endif
# 重载
>在同一作用域下，同名的函数，参数列表不一致则构成重载
>
>参数列表不一致表现为：参数类型不同，参数个数不同，参数顺序不同
>
>重载通过C++编译器对函数名改名实现
>
>示例代码：

    #if 1
    using namespace std;
    
    int add(int a, int b)//编译过后的函数名：_Z3addii
    {
    	return a + b;
    }
    
    double add(double a, int b)//编译过后的函数名：_Z3adddi
    {
    	return a + b;
    }
    
    double add(int a, double b)//编译过后的函数名：_Z3addid
    {
    	return a + b;
    }
    
    int add(int a, int b,int c)//编译过后的函数名：_Z3addiii
    {
    	return a + b + c;
    }
    
    int main()
    {
    	cout << add(1, 2) << endl;//3
    	cout << add(1.1, 2) << endl;//3.1
    	cout << add(1, 2.1) << endl;//3.1
    	cout << add(1, 2,3) << endl;//6
    	return 0;
    }
    
    #endif
# 缺省
>给函数参数设置默认值
>
>禁止声明和定义同时设置参数默认值
>
> 函数声明可以多次，定义只能是一次。建议声明时设置默认参数值

    #if 1
    using namespace std;
    
    void fun(int a,int b);//声明
    
    void fun(int a = 10, int b = 20)//定义
    {
    	cout << "a:" << a << "b:" << b << endl;
    }
    int main()
    {
    	fun();
    	return 0;
    }
    #endif


# 哑元
> 函数的参数只指定类型，不指定名字
> 用于兼容旧版本无用的参数

    #if 1
    using namespace std;
    
    
    //旧版本
    //void fun(int a , int b)
    //{
    //	cout << "a:" << a << "b:" << b << endl;
    //}
    //新版本
    void fun(int , int b )
    {
    	cout  << "b:" << b << endl;
    }
    int main()
    {
    	fun(10,20);
    	return 0;
    }
    #endif

# 引用 #
> 引用是变量的别名，本质是一个指针
> 
> 引用必须初始化，不能为空

    #if 1
    using namespace std;
    
    int main()
    {
    	int age = 18;
    	int& a = age;
    	cout << "age:" << age << endl;
    	cout << "int& a:" << a << endl;
    	//地址相同
    	cout << "age地址：" << &age << endl;
    	cout << "int& a地址：" << &a << endl;
    	return 0;
    }
    #endif
# 动态内存分配
> 使用new/delete操作符来申请/释放内存

    #if 1
    using namespace std;
    
    int main()
    {
    	int* age = new int(18);
    	cout << *age << endl;
    	delete age;
    	age = NULL;
    
    	int* arr = new int[3];
    	for (size_t i = 0; i < 3; i++)
    	{
    		arr[i] = i;
    	}
    	for (size_t i = 0; i < 3; i++)
    	{
    		cout << "arr[i]:" << arr[i] << endl;
    
    	}
    	delete[] arr;
    	arr = NULL;
    	return 0;
    }
    #endif

# 面向对象
> 适合开发大型软件
> 
>数据抽象，代码复用，提升效率，减低成本
>
> 三大特性：封装，继承，多态
> 
> 万物皆对象

# 类
> 拥有相同属性和行为的一组对象
> 
> 类是一种自定义的数据类型，包括表示属性的成员变量和表示行为的成员函数
> 
> 类是对象的抽象，对象是类的具体实例
> 
> class 类名 :public/protected/private（继承方式） 基类
> 
> {
> 
> 访问控制限定符：
> 
>    成员变量
>    
>    成员函数
> 
> }
> 
> public:外部，自己以及派生类都可以访问
> 
> protected:外部不可访问，自己和派生类可以访问
> 
> private:外部和派生类不可访问，只有自己可以访问
> 
> 类的成员默认是被private修饰
> 
> 封装：将实现的细节隐藏，暴露公有接口

    #if 1
    using namespace std;
    
    class Ldc
    {
    public:
    	int m_age;
    	void setAge(int age)
    	{
    		m_age = age;
    	}
    	int getAge()
    	{
    		return m_age;
    	}
    };
    
    int main()
    {
    	//栈区对象
    	Ldc z_ldc;
    	z_ldc.setAge(24);
    	cout << z_ldc.getAge() << endl;
    
    	//堆区对象
    	Ldc* d_ldc = new Ldc;
    	d_ldc->setAge(24);
    	cout << d_ldc->getAge() << endl;
    	delete d_ldc;
    	d_ldc = NULL;
    	return 0;
    }
    #endif

# 构造函数
> 
> 为成员变量赋初值，分配资源，设置对象的初始状态
> 
> 函数名与类同名，无返回类型。
> 
> 对象创建时自动调用
> 
> 创建对象过程：
> 
> 1.为整个对象分配内存（先计算对象所需的内存）
> 
> 2.构造基类。（如果有基类）
> 
> 3.构造成员变量
> 
> 4.执行构造函数代码

> 构造函数的重载：若一个类无定义任何构造函数，编译器会提供一个默认的无参构造函数，若定义了构造函数，编译器将不再提供默认的构造函数

# 析构函数
> 用于释放对象所占的内存，在对象销毁时系统自动调用，无参，无返回类型
> 
> 销毁过程：
> 
> 1.执行析构函数代码
> 
> 2.析构成员变量
> 
> 3.析构基类部分
> 
> 4.释放整个对象所占内存

# this
> this是成员函数默认的参数值，是个指针，指向当前的对象