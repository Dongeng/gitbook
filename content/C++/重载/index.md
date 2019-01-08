# 重载

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