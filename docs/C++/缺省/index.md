# 缺省

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
