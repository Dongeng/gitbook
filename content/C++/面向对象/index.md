# 面向对象



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