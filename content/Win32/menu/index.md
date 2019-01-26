# menu

# 菜单  
> 最重要的用户界面
> 
> 按照菜单在程序中的构造过程分为静态菜单、动态菜单、快捷菜单
> 
> 菜单允许嵌套
> 
> 每一个菜单项都有两个最基本要素：菜单项名称（用户见名知其功能）、菜单项ID（便于查找）



	case WM_COMMAND:
			wmId    = LOWORD(wParam);//低两字节保存菜单项id
			wmEvent = HIWORD(wParam);
			// 分析菜单选择: 
			switch (wmId)
			{
			//匹配菜单ID  添加功能
			case ID_NEWFILE:
				MessageBox(0, 0, 0, 0);
				break;
			case IDM_ABOUT:
				DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
				break;
			case IDM_EXIT:
				DestroyWindow(hWnd);
				break;
			default:
				return DefWindowProc(hWnd, message, wParam, lParam);
			}
			break;


### 静态菜单 （方便） ###

> 在菜单资源编辑器中预先定义好的
> 
> WM_COMMAND消息中  
> 
> wmId    = LOWORD(wParam);////低两字节保存菜单项id

	case WM_COMMAND:
		wmId= LOWORD(wParam);//低两字节保存菜单项id
		wmEvent = HIWORD(wParam);
> 第二种加载菜单方式：通过`LoadMenu()`函数加载菜单，并把 `CreateWindow`中的 `menu`参数改成`LoadMenu()`得到的菜单句柄

### 动态菜单（灵活） ###
> 在程序运行过程中生成的
> 
> `CreateMenu()`  创建空菜单
> 
> `AppendMenu()`  尾部添加菜单项  


	 HMENU hMenu = nullptr;
	
	   hMenu = CreateMenu();
	
	   AppendMenu(
		   hMenu, //菜单句柄
		   0, //风格
		   1001, //菜单项ID
		   _T("文件(&F)")//菜单名
		   );

> 菜单嵌套

	   HMENU hMenu = nullptr;
	
	   hMenu = CreateMenu();
	
	   //菜单嵌套
	   HMENU hMenu_1 = nullptr;
	
	   hMenu_1 = CreateMenu();
	
	   AppendMenu(hMenu_1, 0, 1005, _T("新建(&N)"));
	   AppendMenu(hMenu_1, 0, 1006, _T("打开(&O)"));
	
	   //嵌套
	   //把菜单理解为二维数组，如果需要给一个菜单项做嵌套一个字菜单，这个菜单的id将不再给值，用子菜单句柄代替，菜单风格为MF_POPUP(弹出式菜单)
	   AppendMenu(hMenu, MF_POPUP, (UINT)hMenu_1, _T("文件(&F)"));

> `EnableMenuItem(hMenu, 1009, MF_GRAYED);`//菜单项变灰
> 
> `EnableMenuItem(hMenu, 1009, MF_ENABLED);`//菜单项激活

### 快捷菜单 ###
> 前两种菜单的组合，在菜单编辑中预先编辑好，在程序运行过程中动态显示