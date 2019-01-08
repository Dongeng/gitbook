# 算法

> ## 算法
> 求解问题的步骤
> 
> 算法的特征：
> 
> 1.输入项：一个算法有0个或多个输入
> 
> 2.输出项：一个算法有一个或多个输出
> 
> 3.有穷性：必须能在执行有限个步骤之后终止
> 
> 4.确切性：每⼀步骤必须有确切的定义
> 
> 5.可⾏性：每个计算步都可以在有限时间内完成（也称之为有效性）
> 
> 6.可读性：算法要便于阅读、理解和交流
> 
> 7.健壮性：算法不应该得到莫名其妙的结果
> 
> 8.性价比：利用最少的资源得到满足要求的结果


# 排序


## 冒泡排序  

> 1.比较相邻的元素，若元素大小关系不符合，交换他们两个
> 
> 2.对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对
> 

> 时间复杂度：O(n^2)



> 版本1：
> 外层循环 比较len 轮   内层循环 为相邻两个元素的比较 

    void  BubbleSort(int *arr,int len)
    {
    	for (size_t i = 0; i < len; ++i)
    	{
    		for (size_t j = 0; j < len - 1; ++j)
    		{
    			if (arr[j + 1] < arr[j])
    			{
    				int temp = arr[j + 1];
    				arr[j + 1] = arr[j];
    				arr[j] = temp;
    			}
    		}
    	}
    }


> 版本2：
> 外层 每循环一次  都有一个已经排好了序的元素  

    void  BubbleSort(int *arr,int len)
    {
    	for (size_t i = 0; i < len; ++i)
    	{
    		for (size_t j = 0; j < len -1 - i; ++j)
    		{
    			if (arr[j + 1] < arr[j])
    			{
    				int temp = arr[j + 1];
    				arr[j + 1] = arr[j];
    				arr[j] = temp;
    			}
    		}
    	}
    }


> 版本3：
> 若在一次外层循环中  发现没有相邻元素交换 说明 所有 元素已经是有序的

    void  BubbleSort(int *arr,int len)
    {
    	bool flag = false;
		for (size_t i = 0; i < len; ++i)
		{
			flag = false;
			for (size_t j = 0; j < len - i - 1; ++j)
			{
				if (arr[j + 1] < arr[j])
				{
					flag = true;
					int temp = arr[j + 1];
					arr[j + 1] = arr[j];
					arr[j] = temp;
				}
			}
			if (!flag)
			{
				break;
			}
		}
    }

## 快速排序

> 1.从待排序序列中挑出一个元素，作为”基准”
> 
> 2.把所有比基准值小的元素放在基准前面，所有比基准值大的元素放在基准的后面（相同的数可以到任一边）,这个过程叫分组或分区
> 
> 3.递归地把”基准值前面的子数列”和”基准值后面的子数列”进行排序。不多于一个为止


    /*
    1.选基准  一般选第一个 或 最后一个
    
    */
    
    void QuickSort(int *arr, int left,int right)
    {
    	if (left<right)
    	{
    		int _p = arr[left];//基准
    		int _l = left;
    		int _r = right;
    		while (_l<_r)
    		{
    			//从右边找  直到找到  小于基准值
    			while (_l<_r && arr[_r]>_p)
    			{
    				--_r;
    			}
    			// 放在左边
    			if (_l < _r)
    			{
    				arr[_l++] = arr[_r];
    			}
    			//从左边找 直到找到  大于基准值
    			while (_l<_r && arr[_l]<_p)
    			{
    				++_l;
    			}
    			// 放在右边
    			if (_l < _r)
    			{
    				arr[_r--] = arr[_l];
    			}
    		}
    		// 此时 _l == _r   吧基准值放在这里  此时 左边的小于基准值  右边的大于基准值  对于基准值来说是有序的
    		arr[_r] = _p;
    		// 左边的一部分 递归 对于某个数  又是有序的
    		QuickSort(arr, left, _l - 1);
    		// 右边的一部分 递归 对于某个数  又是有序的
    		QuickSort(arr, _l + 1, right);
    	}
    }


## 插入排序
> 1.从第一个元素开始，该元素可以认为已经被排序；


> 2.取出下一个元素，在已经排序的元素序列中从后向前扫描；


> 3.如果该元素（已排序）大于新元素，将该元素移到下一位置；


> 4.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；


> 5.将新元素插入到该位置后；


> 6.重复步骤2~5。
    
    void InsertSort(int *arr, int len)
    {
    	int temp;
    	int j;
    	for (size_t i = 1; i < len; ++i)
    	{
    		temp = arr[i];
    		j = i - 1;
    		while (j>=0 && arr[j] > temp)
    		{
    			arr[j + 1] = arr[j];
    			j--;
    		}
    		arr[j + 1] = temp;
    	}
    }
    

## 选择排序
> 1.未排序的数列中找到最小(or最大)元素，然后将其存放到数列的起始位置；


> 2.从剩余未排序的元素中继续寻找最小(or最大)元素，然后放到已排序序列的末尾；


> 3.以此类推，直到所有元素均排序完毕；

    void SelectSort(int *arr, int len)
    {
    	size_t temp;
    	for (size_t i = 0; i < len -1; ++i)
    	{
    		temp = i;
    		// 循环找当前最小值
    		for (size_t j = i+1; j < len ; ++j)
    		{
    			if (arr[temp] > arr[j])
    			{
    				temp = j;
    			}
    		}
    		// 交换值
    		swap(arr[temp], arr[i]);
    	}
    }


## 希尔排序
> 部分有序  再排序     
> 
> 通过步长 使得部分已经有序   最后使用插入排序
> 
> 插入排序的优化版

    
    void ShellSort(int *arr, int len)
    {
    	int temp;
    	int j;
    	int gap;
    	for (gap = len / 2; gap>0; gap/=2)
    	{
    		for (size_t i = gap; i < len; ++i)
    		{
    			temp = arr[i];
    			j = i - gap;
    			while (j >= 0 && arr[j] > temp)
    			{
    				arr[j + gap] = arr[j];
    				j -= gap;
    			}
    			arr[j + gap] = temp;
    		}
    	}
    	
    }
