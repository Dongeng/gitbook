# bool和内联函数


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

