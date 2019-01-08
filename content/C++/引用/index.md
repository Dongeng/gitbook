# 引用


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

