# 进程线程

# 进程
> 一个正在运行的应用程序实例

# 线程
> 进程中的某个执行序列（函数）


# 量程
> 操作系统为每一个线程安排了一定的cpu时间，它通过循环的方式为线程提供了时间片


# 关系
> 进程活动性不强
> 
> 如果需要一个进程去完成某项操作，它必须拥有一个在它的环境里面运行的线程
> 
> 线程总是在某个进程的环境中创建，线程的生命周期会受到进程的影响
> 
> 进程一旦创建，系统必然会同时创建一个主线程
> 
> 进程一旦死亡，这个进程里面的所有线程都会死亡
> 
> 线程需要的开销比进程少
> 
> 如果需要去异步解决问题，尽量创建线程，避免创建新进程

# 进程的两个组成部分
> 1.管理进程的内核对象
> 
> 2.地址空间


# 创建进程
> `CreateProcess(/*可执行应用程序路径文件名*/,/*命令行参数*/,/*是否被子进程继承*/,/*是否被子线程继承*/,/*新进程是否在调用进程处继承*/,/*创建标记*/,/*新进程的环境块*/,/*新进程的工作路径*/,/*新进程的窗口特性*/,/*进程句柄，id等信息*/)`


	STARTUPINFO si;
	ZeroMemory(&si, sizeof(si));
	si.cb = sizeof(si);
	PROCESS_INFORMATION pi;
	CreateProcess(_T(""), nullptr, nullptr, nullptr, false, 0, nullptr, nullptr, &si, &pi);
	g_hInst = (HINSTANCE)pi.hProcess;//保存此进程  用于关闭等

# 退出进程
> 1.主线程入口函数返回（建议使用）
> 
> 2.`TerminateProcess(g_hInst,0)`
> 
> 3.`ExitProcess(0)`//结束当前进程

# 线程的两个组成部分
> 1.操作系统内核对象，用来存放线程统计信息
> 
> 2.线程堆栈，用来维护线程在执行代码过程中需要的所有函数的参数和局部变量

# 创建线程
> `CreateThread(/*安全属性*/,/*初始堆栈大小，默认给0*/,/*线程函数*/,/*传给线程函数的参数*/,/*线程创建参数*/,/*线程id*/)`

# 退出线程
> 1.在线程函数中主动退出
> 
> 2.在某个线程中调用TerminateThread结束某个线程
> 
> 3.在某个线程中调用ExitThread函数去结束当前调用的线程


# 多线程
> 线程通讯
> 
> 线程需要在两种情况下进行通讯
> 
> 1.如果多个线程访问共享资源而不能够让这个共享资源被破坏
> 
> 2.当一个线程将某个任务完成之后通知另一个线程接着去完成这一系列的任务

# 线程互斥（解决方式）
> 1.用户区间
> 
> 2.内核对象（并不意味着只能用来做线程互斥）

# 用户区间
> 临界区  定义一个临界区的变量
> 
> `CRITICAL_SECTION g_cs;`
> 
> 初始化`InitializeCriticalSection(&g_cs)`
> 
> 进入临界区`EnterCriticalSection(&g_cs)`
> 
> 离开临界区`LeaveCriticalSection(&g_cs)`
> 
> 释放临界区`DeleteCriticalSection(&g_cs)`

# 线程死锁
> 有优先级的情况下 ，某一个低优先级的得到了cpu时间片。带着线程锁，之后得不到cpu时间片去解锁。此时其他线程即使得到了cpu时间片，但因为有锁，也不能处理


# 内核对象
> 内核对象都可以处于已通知和未通知两种状态，状态的切换时系统的规则来决定
> 
> 在线程运行的过程中，线程内核对象处于未通知状态
> 
> 当线程结束时，线程内核对象处于已通知状态


# 事件：基于内核对象的同步机制
> 
> 
> 1.定义事件句柄
> 
> 2.事件初始化 CreateEvent(nullptr,false,true,nullptr)
> 
> 3.无限制的等待一个通知`WaitForSingleObject(hEve, INFINITE)`
> 
> 4.设置事件通知`SetEvent(hEve)`;
> 
> 5.关闭事件 `CloseHandle(hEve)`


# 互斥对象：确保线程对于单个资源的互斥访问权。
> 包含一个使用数量，一个线程id，一个递归的计数器
> 
> 互斥对象的行为和临界区有点类似，互斥属于内核，临界区属于用户
> 
> id用来标识在系统中哪个线程当前拥有互斥对象
> 
> 递归计算器表示这个线程拥有互斥对象多少次
> 
> 互斥对象的使用规则：如果线程id为0,表示无效的id，互斥对象没有被线程所拥有，互斥对象发出信号
> 
> 如果id非0,表示有线程拥有互斥对象
> 
> 使用：
> 
> 1. HANDLE g_hMetux
> 
> 2.创建互斥对象 `g_hMetux = CreateMutex(nullptr, true, nullptr)`
> 
> 3.`WaitForSingleObject(g_hMetux, INFINITE)`
> 
> 4.`ReleaseMutex(g_hMetux)`
> 
> 5.关闭互斥对象 `CloseHandle(g_hMetux)`


# 信号量
> 用于资源进行计数
> 
> 使用规则
> 
> 1.当前资源量数量大于0，可以去使用资源，发出信号
> 
> 2.当前资源数量为0，没有可以使用的资源，不发出信号
> 
> 3.系统不允许资源数量为负数
> 
> 4.当前可以使用过的资源数量不能大于最大的资源数量
> 
> 
> 使用：
> 
> 1. HANDLE g_hSemap;
> 
> 2.`g_hSemap = CreateSemaphore(nullptr,1,1,nullptr)`
> 
> 3.`WaitForSingleObject(g_hSemap, INFINITE)`
> 
> 4.重置信号量 `ReleaseSemaphore(g_hSemap,1,nullptr)`
> 
> 5.关闭信号量 `CloseHandle(g_hSemap)`

# 结束线程
> 结束主线程之前，确认子线程结束
> 
> `GetExitCodeThread()` 得到线程退出码