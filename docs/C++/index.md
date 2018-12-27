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

# 常成员
> 常成员变量：必须通过初始化表来赋值，初始化后不能修改
> 
> 每个重载的构造函数都需初始化常成员变量，确保常成员变量有值
    
    #if 1
    
    using namespace std; 
    
    class Human
    {
    public:
    	Human(string const& name) :m_name(name){};
    
    private:
    	const string m_name;
    public:
    	void look_name()
    	{
    		cout << this->m_name << endl;
    	}
    };
    
    
    int main()
    {
    	Human ldc = Human("ldc");
    	ldc.look_name();
    	return 0;
    }
    #endif

> 常成员函数：在圆括号后面加个const 修饰this参数，即不能通过this指针修改成员变量的值，除非该成员变量被mutable修饰
> 
> 和普通同名成员函数构成重载


    #if 1
    
    using namespace std;
    
    class Human
    {
    public:
    	Human(int age) :m_age(age){};
    
    private:
    	int m_age;
    public:
    	//成员函数
    	void look_name()
    	{
    		cout << m_age++ << endl;
    	}
    	//常成员函数
    	void c_look_name()const
    	{
    		//不可修改
    		//cout << m_age++ << endl;
    		cout << m_age << endl;
    	}
    };
    
    
    int main()
    {
    	Human ldc = Human(24);
    	ldc.c_look_name();//24
    	ldc.look_name();//24
    	ldc.c_look_name();//25
    	
    	return 0;
    }
    #endif

# 常对象
> 被const 修饰的对象，对象指针或对象引用
> 
> 常对象只能调用常函数，非常对象即可调用常函数，也可以调用非常函数

    #if 1
    
    using namespace std;
    
    class Human
    {
    public:
    	Human(int age) :m_age(age){};
    
    private:
    	int m_age;
    public:
    	//成员函数
    	void look_name()
    	{
    		cout << m_age++ << endl;
    	}
    	//常成员函数
    	void c_look_name()const
    	{
    		//不可修改
    		//cout << m_age++ << endl;
    		cout << m_age << endl;
    	}
    	//重载的常成员函数
    	void look_name()const
    	{
    		cout << m_age << endl;
    	}
    };
    
    
    int main()
    {
    	Human ldc = Human(24);
    	ldc.c_look_name();//24
    	ldc.look_name();//24
    	ldc.c_look_name();//25
    	
    	const Human _ldc = Human(24);
    	_ldc.look_name();//24
    	_ldc.look_name();//24
    
    	return 0;
    }
    #endif

# 拷贝构造

    #if 1
    using namespace std;
    
    class Complex
    {
    public:
    	Complex(double real, double vir = 0) :m_real(real), m_vir(vir){};
    private:
    	double m_real;
    	double m_vir;
    public:
    	void print()
    	{
    		cout << m_real << "+" << m_vir << "i" << endl;
    	}
    };
    
    int main()
    {
    	Complex c1(2.5);//显示调用单参构造
    	c1.print();
    
    	Complex c2 = 2.5;//隐式调用单参构造 实际语句为  Complex c2 = Complex (2.5);,用一个临时构造的匿名变量来转换
    	c2.print();
    	return 0;
    }
    #endif
> explicit：不能隐式调用构造函数

    #if 1
    using namespace std;
    
    class Complex
    {
    public:
    	//用explicit修饰，则不能隐式调用
    	explicit Complex(double real, double vir = 0) :m_real(real), m_vir(vir){};
    private:
    	double m_real;
    	double m_vir;
    public:
    	void print()
    	{
    		cout << m_real << "+" << m_vir << "i" << endl;
    	}
    };
    
    int main()
    {
    	Complex c1(2.5);//显示调用单参构造
    	c1.print();
    
    	//Complex c2 = 2.5;//隐式调用单参构造 实际语句为  Complex c2 = Complex (2.5);,用一个临时构造的匿名变量来转换 若单参构造用explicit修饰，则不能隐式调用
    	//c2.print();
    	return 0;
    }
    #endif
> 拷贝构造

    #if 1
    using namespace std;
    
    class Complex
    {
    public:
    	//普通构造
    	Complex(double real, double vir)
    	{
    		cout << "调用普通构造" << endl;
    		this->m_real = real;
    		this->m_vir = vir;
    	}
    	//拷贝构造
    	Complex(Complex const& that)
    	{
    		cout << "调用拷贝构造" << endl;
    		this->m_real = that.m_real;
    		this->m_vir = that.m_vir;
    	};
    private:
    	double m_real;
    	double m_vir;
    public:
    	void print()
    	{
    		cout << m_real << "+" << m_vir << "i" << endl;
    	}
    };
    
    int main()
    {
    	Complex c1(1, 2.3);//调用普通构造
    	c1.print();
    	Complex c2 = c1;//调用拷贝构造  隐式调用  Complex c2 = Complex (c1);
    	c2.print();
    	return 0;
    }
    #endif
> 若没有定义拷贝构造函数，会提供默认的拷贝构造函数

# 浅拷贝，深拷贝
> 浅拷贝：对内存地址的复制，产生一个新指针和原来的指针指向同一片内存，只要通过期中一个指针去释放内存，那么其他指针都会变成野指针
> 
> 深拷贝：对拷贝对象内存的复制，拷贝结束后，两个对象的值相同，内存地址不同。即两个独立的个体。互不影响，互不干涉。

# 友元
> 友元函数 ： friend + 函数的声明部分。即声明一个函数为友元函数

    #if 1
    using namespace std;
    
    
    class Ldc
    {
    	friend void print(Ldc const& that);//声明友元，
    public:
    	Ldc(int age) :m_age(age)
    	{
    		
    	};
    	
    private:
    	
    	int m_age;
    
    };
    
    void print(Ldc const& that)
    {
    	cout << that.m_age << endl;//友元函数可以访问到that对象的私有成员m_age;
    }
    
    
    int main()
    {
    	Ldc ldc = 25;
    	print(ldc);
    	return 0;
    }
    #endif
> 友元类： friend + 类的声明部分。声明一个类为友元类。友元类所有成员函数都是友元函数

    #if 1
    using namespace std;
    
    
    
    class Point2D
    {
    	friend void print(Point2D const& that);
    	friend class Point3D;//声明友元类
    public:
    	Point2D(double x = 0, double y = 0) :m_x(x), m_y(y)
    	{
    
    	};
    
    private:
    	double m_x;
    	double m_y;
    };
    class Point3D
    {
    public:
    	Point3D(double x = 0, double y = 0, double z = 0)
    	{
			//可以访问Point2D类对象内的私有成员
    		m_p.m_x = x;
    		m_p.m_y = y;
    		m_z = z;
    	}
    	
    private:
    	Point2D m_p;
    	double m_z;
    
    public:
    	void print()
    	{
    		//可以访问Point2D类对象内的私有成员
    		cout << "空间点坐标：(" << m_p.m_x << "," << m_p.m_y << ","<<m_z<<")" << endl;
    	}
    };
    void print(Point2D const& that)
    {
    	
    	cout <<"平面点坐标：("<< that.m_x << "," << that.m_y << ")" << endl;
    }
    
    int main()
    {
    	Point2D p1(1, 2);
    	print(p1);
    
    	Point3D p2(1, 2, 3);
    	p2.print();
    	return 0;
    }
    #endif
> 友元关系是单向的，不具有传递性

# 运算符重载
> operator+运算符  ： 重载该运算符

    #if 1
    using namespace std;
    
    
    
    class Complex
    {
    	friend Complex& operator+(Complex const& c1, Complex const& c2);
    public:
    	Complex(double real = 0, double vir = 0) :m_real(real), m_vir(vir){};
    
    private:
    	double m_real;
    	double m_vir;
    public:
    	//成员函数重载运算符
    	/*Complex& operator+(Complex const& that)const
    	{
    		Complex tmp(that.m_real + this->m_real, that.m_vir + this->m_vir);
    		return tmp;
    	}*/
    	void print()
    	{
    		cout << m_real << "+" << m_vir << "i" << endl;
    	}
    };
    //外部友元函数重载运算符
    Complex& operator+(Complex const& c1, Complex const& c2)
    {
    	Complex tmp(c1.m_real + c2.m_real, c1.m_vir + c1.m_vir);
    	return tmp;
    }
    
    int main()
    {
    
    	Complex c1(1, 2);
    	c1.print();//1+2i
    
    	Complex c2(2, 2);
    	c2.print();//2+2i
    
    	Complex c3 = c1 + c2;
    	c3.print();//3+4i
    	return 0;
    }
    #endif

> 单目运算符建议使用成员函数的方式重载
> 
> 双目运算符建议使用友元函数的方式重载
> 
> 三目运算符不能重载
> 
> ::作用域限定符 .成员访问运算符 .*成员指针运算符 sizeof长度运算符不能重载
> 
>  <<左插，>>右插只能使用友元函数方式重载
>  
> = () [] -> ->* 只能是成员函数

# 拷贝赋值重载
> 避免自赋值
> 
> 分配新资源
> 
> 拷贝新内容
> 
> 释放久资源
> 
> 返回自引用

# 静态成员

> static成员 ： 
> 
> 1.为类的所有成员共有，属于类。生命周期是进程级的
> 
> 2.内存在程序运行时分配。
> 
> 3.使用前必须初始化，且只能初始化一次
> 
> 4.初始化不能在类中定义，需要通过作用域限定符初始化 
> 
> 5.初始化：  值类型 类::类静态成员变量名 = 值;

# 单例模式
> 饿汉式：

    #if 1
    using namespace std;
    
    
    //饿汉式
    
    class Single
    {
    
    public:
    
    	static Single& getInstabce()
    	{
    		
    		return instance;
    	}
    private:
    	//程序运行时就创建了
    	static Single instance;
    	int m_data;
    	Single(int data) :m_data(data)
    	{
    	}
    
    };
    Single Single::instance(1);//初始化
    int main()
    {
    	Single& ldc = Single::getInstabce();
    	Single& lsx = Single::getInstabce();
    	//得到的地址一样
    	cout << "&ldc:" << &ldc << endl;
    	cout << "&lsx:" << &lsx << endl;
    	return 0;
    }
    
    #endif
> 
> 懒汉式：

    #if 1
    using namespace std;
    
    
    //懒汉式
    
    class Single
    {
    	
    public:
    	
    	static Single& getInstabce()
    	{
    		if (!instance)
    		{
    			//...TO DO
    			//在需要的时候再开辟内存
    			instance = new Single();
    		}
    		return *instance;
    	}
    private:
    	//程序运行时只有一个4字节的指针
    	static Single* instance;
    	Single()
    	{
    	}
    	
    };
    Single* Single::instance = NULL;//先指向空 
    int main()
    {
    	Single& ldc = Single::getInstabce();
    	Single& lsx = Single::getInstabce();
    	//得到的地址一样
    	cout << "&ldc:" << &ldc << endl;
    	cout << "&lsx:" << &lsx << endl;
    	return 0;
    }
    
    #endif

# 成员指针
> 成员变量在对象中的相对地址，指向成员变量的指针

    #if 1
    
    using namespace std;
    
    class Ldc
    {
    public:
    	Ldc(int age) :m_age(age){};
    
    public:
    	int m_age;
    };
    
    int main()
    {
    	Ldc l1 = Ldc(24);
    	int Ldc::*Pvalue = &Ldc::m_age;
    	(l1.*Pvalue)++;
    	cout << l1.m_age << endl;//25
    	return 0;
    }
    
    #endif

# 去常 const_cast
> 强转不改变原来的变量

    #if 1
    
    using namespace std;
    
    int main()
    {
    	const int rmb = 100;
    	int *temp = const_cast<int*>(&rmb);
    	*temp = 200;
    
    	cout << "*temp:" << *temp << endl;//200
    	cout << "rmb:" << rmb << endl;//100
    
    	cout << "temp:" << temp << "   " << "&rmb:" << &rmb << endl;//地址相同
    	return 0;
    }
    
    #endif
> 寄存器读内存，再丢给变量值；寄存器看到 rmb 被const修饰。认为内存是不变的。所以直接把100给出；
> 
> 使用关键字 volatile 修饰变量为异变的

    #if 1
    
    using namespace std;
    
    int main()
    {
    	volatile const int rmb = 100;
    	int *temp = const_cast<int*>(&rmb);
    	*temp = 200;
    
    	cout << "*temp:" << *temp << endl;//200
    	cout << "rmb:" << rmb << endl;//200
    
    	cout << "temp:" << temp << "   " << "&rmb:" << (void*)&rmb << endl;//地址相同
    	return 0;
    }
    
    #endif

# 继承
> class 类名 :public 基类1,protected 基类2,private 基类3,......
> {}
> 
> 共性和个性
> 
> 子类继承了父类的共性派生了个性

    #if 1
    
    using namespace std;
    
    
    class Human
    {
    public:
    	Human(const string& name) :m_name(name){};
    
    private:
    	string m_name;
    public:
    	void print_name()
    	{
    		cout << this->m_name << endl;
    	}
    };
    
    class Ldc :public Human
    {
    public:
    	Ldc(const string& name, int age) :Human(name), m_age(age){};
    
    private:
    	int m_age;
    };
    
    int main()
    {
    	Ldc _ldc = Ldc("ldc", 18);
    	_ldc.print_name();
    	return 0;
    }
    
    #endif

> public 公有继承 继承父类的成员 访问控制属性不变
> 
> protected 保护继承  继承父类的成员 public转为protected。 protected和private不变
> 
> private 私有继承  继承父类的成员 private不变。public和protected转为private

# 皆然性/is a 
> 在公有继承下，子类对象或引用在任何时候可以看成基类
> 
> 在构造对象时，先构造基类子对象
> 
> 可以用父类的一个指针或引用去接子类的地址，当成父类来用，但仅限于子类的该父类类型的子对象，子类中的其他基类子对象和子类中的个性成员无法访问。

> 访问范围缩小在编译器看来是安全的
> 
> 访问范围扩大在编译器看来是危险的

# 多态
> 一个函数在不同场景下不同的功能

> 虚函数： 使用 `virtual`  关键字声明的成员函数
> 
> 覆盖：


> 子类成员函数和基类的虚函数具有相同的函数原型，该成员函数也是虚函数。对基类虚函数构成覆盖。
> 
> 覆盖条件：
> 
> 1.函数为成员函数（非静态）
> 
> 2.基类使用 `virtual`
> 
> 3.函数原型严格相同

> 基类指针调用会根据实际指向的类型来分别调用不同的函数。这种现象称为多态

# 虚析构
> 避免皆然性导致的子类其他基类子对象和个性内存泄漏

# 纯虚函数
> 形如 virtual 返回值 函数名（形参表） =0;的虚函数，叫纯虚函数。

# 抽象类
> 有一个成员函数是纯虚函数的类称为抽象类。

# 纯抽象类（接口类）
> 所有成员函数都是纯虚函数
> 
> 子类必须完全覆盖基类虚函数

# 虚函数表

# io流
> `cout` : 标准输出流（有缓冲区）
> 
> `cerr` : 标准错误流（无缓冲区）
> 
> `clog` : 标准错误流（有缓冲区）
> 
> `cin`  : 标准输入流
> 
> `cout`输出可以重定向到一个文件中，`cerr`必须显示在显示器上
> 
> 获取行
> `cin.getline(str/*目标*/,count/*大小*/,endChar/*结束字符*/);`

# 字符串流
> 头文件 `<sstream>`

# 文件流
> 头文件 `<fstream>`

# 异常
> 语法错误 逻辑错误 功能错误 设计错误 需求不符 环境异常 操作错误

> 解决：
> 
> 1.利用返回值 例如-1;
> 
> 2.`setjmp`/ `longjmp`
> 
> 3.抛出-捕获异常  `throw` / `try-catch`
> > 捕获顺序：根据异常对象类型，从上到下匹配，并非最优匹配，因此对子类类型的异常的捕获不要放在基类类型异常捕获的后面
> 

# 标准库异常

# RTTI () 运行时类型识别
> 只能用在包含虚函数的类

# reinterpret_cast 重解释类型

# static_cast<目标类型>（源类型变量）
> 隐式类型的逆转换
> 
> 下行转换 向下造型 没有RTTI 不安全

# dynamic_cast<目标类型>（源类型变量）
> 在有虚函数的前提下 动态类型转换 有RTTI 会比较类型 使用typeid比较声明和实际类型 对于指针，不一致会返回`nullptr`,对于引用的比较，不一致抛出 `bad_typeid`异常 安全

# typeid 和 type_info类
> typeid 可以确定两个对象是否为相同类型，和sizeof类似，接收类名和结果和对象的表达式，返回一个type_info对象的引用
> 
> typeid(类名) == typeid(对象指针) ， 对象指针为空，引发bad_typeid异常
> 


# c++11
> 1.nullptr  空指针
> 
> 2.`using dint = int;` //类型别名
> 
> 3.auto类型修饰符 `auto a = 10;//typeid(a).name() == int`
> 
> 4.`long long 类型` `unsigned long long 类型` `char16_t 类型` `char32_t 类型`
> 
> 5.类内初始化

    class Ldc
    {
    public:
    	int m_age { 24 };
    };
    
    int main()
    {
		Ldc _ldc;
		cout << _ldc.m_age << endl;
    	return 0;
    }

> 6.范围for 循环

    for(auto i : arr)
    {
    	cout << i << endl;
    }

> 7.返回类型后置

> 8.默认构造函数 `default`
> 
> 9.已删除函数 `delete`
> 
> 10.委托构造函数

> 11.右值引用 用于移动构造 移动数据  

    int num = 10;
    int &lnum = num;//左值引用
    int &&rnum = 10;//右值引用

> 12.lambda 表达式

    auto sum = [](int a,int b) { return a+b; };
    cout << sum(1,3) << endl;

> 13.智能指针

> 14.`override`  确保多态 子类虚函数覆盖父类虚函数

> 15.`final`  若修饰类 则该类不能被继承。若修饰虚函数，则该虚函数不能被覆盖

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
