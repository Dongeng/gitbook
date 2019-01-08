# 哑元


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

