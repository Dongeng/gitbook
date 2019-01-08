# C++11

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