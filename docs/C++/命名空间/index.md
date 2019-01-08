# 命名空间

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

