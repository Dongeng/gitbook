


#GDI（Graphics Device Interface）
> 图形设备接口
> 
> 主要任务：负责系统与绘图程序之间的信息交互
> 
> gdi是一个组件，有许多函数，大概分为5类
> 
> 
> 
> 1.取得和释放设备上下文的函数
> 
> 2.取得有关设备内容信息的函数
> 
> 3.绘图函数
> 
> 4.设定和取得设备上下文参数的函数
> 
> 5.调用gdi对象的函数


# DC
> 设备上下文 即设备环境  渲染管线



# gdi可以操作的图形图像
> 1.画线
> 
> 2.填充区域
> 
> 3.显示文字
> 
> 4.位图


# gdi画图的步骤
> 1.得到设备环境句柄 HDC hdc = GetDC(hwnd)
> 
> 2.设置设备属性
> 
> 3.绘图
> 
> 4.释放设备环境句柄（必须）（共用一套设备）

# 两种方法得到设备环境句柄（客户区的设备环境）
> 1.在 WM_PAINT 消息中 通过BeginPaint()/EndPaint()来得到/释放设备环境句柄
> 
> 2.在 WM_PAINT 消息外 通过GetDC()/ReleaseDC()来得到/释放设备环境句柄


#WM_PAINT 消息触发条件
> 1.窗口最初创建时
> 
> 2.窗口出现无效区域
> >1.窗口移动或大小改变
> >
> >2.窗口隐藏后重新显示或被其他窗口遮住后重新显示
> >
> >3..InvalidateRect()是矩形区域失效、InvalidateRgn()使区域失效
> >
> >4.ScrollWindow滚动窗口 、ScrollDC滚动设备客户区

#窗口不出现无效区域的四种情况：
> 1.光标穿越客户区
> 
> 2.图标拖过客户区
> 
> 3.显示对话框
> 
> 4.显示菜单后释放

#画笔
> 下面代码是使用画笔画一条从（0,0）到（200,300）的黑线
> 在win32中设备环境有一只默认的黑色的画笔

	{
		hdc = GetDC(hWnd);
		LineTo(hdc, 200, 300);
		ReleaseDC(hWnd, hdc);
	}
> 得到预设的画笔

	//GetStockObject 得到预设的gdi对象
	{
		hdc = GetDC(hWnd);
		HPEN hPen = nullptr;

		hPen = (HPEN)GetStockObject(BLACK_PEN);

		SelectObject(hdc, hPen);//修改设备属性
		MoveToEx(hdc, 200, 200, nullptr);//把起点移动到（200,200）
		LineTo(hdc, 200, 300);


		DeleteObject(hPen);//（必须，释放内存）
		ReleaseDC(hWnd, hdc);
	}
> 创建自定义的画笔

	{
		hdc = GetDC(hWnd);
		HPEN hPen = nullptr;

		hPen = CreatePen(PS_SOLID, 1, 0x00ff00);//创建一个自定义的画笔
		//如果线宽大于1  不管什么风格都会变成实线风格
		SelectObject(hdc, hPen);//同一时刻，同一类型的对象只能有一种
		LineTo(hdc, 200, 300);


		DeleteObject(hPen);

		ReleaseDC(hWnd, hdc);
	}

> 颜色，用 红、绿、蓝三基色表示任何颜色
> 
> win32用32位表示颜色 有4个字节（颜色通道） argb
> 

    #define RGB(r,g,b)  ((COLORREF)(((BYTE)(r)|((WORD)((BYTE)(g))<<8))|(((DWORD)(BYTE)(b))<<16)))



> BYTE    : typedef unsigned char  (无符号char)  1字节
> 
> WORD   ： typedef unsigned short (无符号short) 2字节
> 
> DWORD  ： typedef unsigned long  (无符号long)  4字节



> r 转成1字节的BYTE  
> 
> g 转成1字节的BYTE 再转成2字节的WORD 再左移8位（1字节） 低位补0 
> 
> b g 转成1字节的BYTE 再转成4字节的DWORD 再左移16位（2字节） 低位补0
> 
> 最后强转为 COLORREF 类型

> r | g | b =====>  0x00bgr;（16进制表示）

# 画圆
> Arc()

	//画圆 在程序中指定外接矩形的大小  逆时针绘制
	Arc(hdc, 
	50, 50, 150, 150,//表示外接矩形大小
	0, 0, //圆弧起点  圆心和此点连线与圆的交点为起点
	//若此点是圆心，则做一条与x轴平行且圆心在这条线上的一条线，这条线与圆的交点为起点，终点亦如此
	200, 200//圆弧终点  圆心和此点连线与圆的交点 
	);

#设置像素点
> SetPixel()

#获得像素点
> GetPixel()

# 画刷
> 系统默认有一支黑色的画笔，白色的画刷，
> 
> 用画刷去填充时 ， 会自动的用画笔来描边