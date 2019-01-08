# 动态内存分配

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



