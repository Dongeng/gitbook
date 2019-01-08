# 结构体

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