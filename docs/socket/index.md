# socket


#网络架构：
#七层网络架构：
应用层、表示层、会话层、传输层、网络层、数据链路层、物理层
（基于功能或协议来区分）

#五层网络架构：
应用层、传输层、网络层、数据链路层、物理层（协议来区分）


#tcp/ip:
ipv4:表示32位来表示ip地址

ipv6:表示64位来表示ip地址

ipv4是用点分格式来表示

#socket（套接字）
源ip地址和目标ip地址及源端口号和目标端口号的组合


#tcp通讯步骤: c（客户端）/s（服务端）

1.准备工作，头文件、库文件的导入

2.确定socket的版本信息

3.创建一个socket

4.初始化协议地址（ip）

5.绑定（服务端）

6.监听（服务端）

7.接收连接（服务端）  连接（客户端）

8.通讯、收发数据

9.关闭socket

#TCP特性
tcp是为了建立稳定的传输

传输可靠，面向连接的端对端的传输。

适用于传输量大，可靠性要求比较高

#连接 3次握手
服务端和客户端建立一条稳定的传输通道

1.客户端向服务端发送信息（报文：网络中交互和传输的数据单元） SYN（同步标志）

2.服务端把刚刚客户端发过来的SYN重新发回客户端，再发一个报文 （ACK  确认标志）

3.客户端再向服务端发送报文（ACK）

#断开 4次挥手
1.客户端发送一个FIN  （结束标志），用来关闭客户端到服务端的数据传送

2.服务端收到FIN 发回ACK（确认标志）

3.服务端关闭客户端连接，再发回FIN给客户端

4.客户端发回ACK确认


#单工	
只能从A端到B端
#半双工
A端可以到B端，B端也可以到A端，当某一端正在发送时，不能从另一端发数据
#双工	
可以同时双向通讯


#UDP的特点
面向非连接，不提供可靠性

传输量少，可靠性要求比较低。开销较小


#UDP通讯步骤
1.准备工作

2.确定版本信息

3.创建socket

4.初始化协议地址

5.绑定（服务端）

6.通讯

7.结束


#TCP结合线程，并发服务器
服务端：

	#include "stdafx.h"
	#include <Windows.h>
	//1.
	//#include <WinSock2.h>
	#pragma comment (lib,"ws2_32.lib")
	
	SOCKET Serve;
	
	//线程处理函数
	DWORD WINAPI ThreadProc(
		_In_ LPVOID param
		);
	
	int _tmain(int argc, _TCHAR* argv[])
	{
		//2.
		WSADATA wsaData;
		WSAStartup(MAKEWORD(2, 2), &wsaData);
	
		if (LOBYTE(wsaData.wVersion) != 2 || HIBYTE(wsaData.wVersion) != 2)
		{
			printf("请求版本失败\n");
			return -1;
		}
		printf("请求版本成功\n");
	
		//3.
		Serve = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	
		if (INVALID_SOCKET == Serve)
		{
			printf("创建SOCKET失败\n");
			WSACleanup();
			return -1;
		}
	
		//4.
		SOCKADDR_IN addr = { 0 };
	
		addr.sin_family = AF_INET;
		addr.sin_port = htons(8888);
		addr.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");
	
		//5.
		if (bind(Serve, (SOCKADDR *)&addr, sizeof(addr)) == SOCKET_ERROR)
		{
			printf("绑定协议地址失败\n");
			closesocket(Serve);
			WSACleanup();
			return -1;
		}
		printf("绑定协议地址成功\n");
	
		//6.
		if (listen(Serve, 10) == SOCKET_ERROR)
		{
			printf("监听失败\n");
			closesocket(Serve);
			WSACleanup();
			return -1;
		}
		printf("监听成功\n");
	
	
		//7.
		while (1)
		{
			SOCKADDR_IN client_addr = { 0 };
			int len = sizeof(client_addr);
			SOCKET Client = accept(Serve, (SOCKADDR *)&client_addr, &len);
			if (INVALID_SOCKET == Client)
			{
				printf("连接失败\n");
				closesocket(Serve);
				WSACleanup();//关闭套接字
				return -1;
			}
			printf("连接成功\n");
			HANDLE thread = CreateThread(nullptr, 0, ThreadProc, (LPVOID)Client, 0, nullptr);
			CloseHandle(thread);
		}
	
		
	
	
		//9.
		closesocket(Serve);
		WSACleanup();
		return 0;
	}
	
	DWORD WINAPI ThreadProc(
		_In_ LPVOID param
		)
	{
		//8.通讯
		SOCKET client = (SOCKET)(param);
		char buff[128] = { 0 };
		char sbuff[128] = { 0 };
		while (1)
		{
			ZeroMemory(buff, sizeof(buff));
			ZeroMemory(sbuff, sizeof(sbuff));
			if (recv(client, buff, sizeof(buff) - 1, 0) > 0)
			{
				printf("%s\n", buff);
			}
			scanf_s("%s", sbuff, sizeof(sbuff) - 1);
			if (send(client, sbuff, strlen(sbuff), 0) > 0)
			{
				printf("发送成功\n");
			}
		}
	
	
	
		closesocket(client);
		return 0;
	}

客户端：

	//1.
	#include <WinSock2.h>
	#pragma comment (lib,"ws2_32.lib")
	
	
	
	int _tmain(int argc, _TCHAR* argv[])
	{
		//2.
		WSADATA wsaData;
		WSAStartup(MAKEWORD(2, 2), &wsaData);
	
		if (LOBYTE(wsaData.wVersion) != 2 || HIBYTE(wsaData.wVersion) != 2)
		{
			printf("请求版本失败\n");
			return -1;
		}
		printf("请求版本成功\n");
	
		//3.
		SOCKET client = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	
		if (INVALID_SOCKET == client)
		{
			printf("创建SOCKET失败\n");
			WSACleanup();
			return -1;
		}
	
		//4.
		SOCKADDR_IN addr = { 0 };
	
		addr.sin_family = AF_INET;
		addr.sin_port = htons(8888);
		addr.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");
	
		
		//5.
		if (connect(client, (SOCKADDR*)&addr, sizeof(addr)) == SOCKET_ERROR)
		{
			printf("连接失败\n");
			closesocket(client);
			WSACleanup();
			return -1;
		}
		
		printf("连接成功\n");
		char buff[128] = { 0 };
		char rbuff[128] = { 0 };
		while (1)
		{
			ZeroMemory(buff, sizeof(buff));
			ZeroMemory(rbuff, sizeof(rbuff));
			scanf_s("%s", buff, sizeof(buff) - 1);
			if (send(client,buff,strlen(buff),0) > 0)
			{
				printf("发送成功\n");
			}
			if (recv(client, rbuff, sizeof(rbuff) - 1, 0) > 0)
			{
				
				printf("接收成功:%s\n", rbuff);
			}
		}
	
	
	
		//9.
		closesocket(client);
		WSACleanup();
		return 0;
	}

#select函数，多路io复用
解决开线程开销大的问题

服务端：

	#include "stdafx.h"
	#include <Windows.h>
	#include <vector>
	//1.
	//#include <WinSock2.h>
	#pragma comment (lib,"ws2_32.lib")
	
	
	
	int _tmain(int argc, _TCHAR* argv[])
	{
		FD_SET fd;
		timeval time = {0,0};
		SOCKET Serve;
		SOCKADDR_IN addr = { 0 };
		std::vector<SOCKET> sC;
		int ret = 0;
	
	
		WSADATA wsaData;
		WSAStartup(MAKEWORD(2, 2), &wsaData);
	
		if (LOBYTE(wsaData.wVersion) != 2 || HIBYTE(wsaData.wVersion) != 2)
		{
			printf("请求版本失败\n");
			return -1;
		}
		printf("请求版本成功\n");
	
	
		Serve = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
		
		if (INVALID_SOCKET == Serve)
		{
			printf("创建SOCKET失败\n");
			WSACleanup();
			return -1;
		}
	
		
		addr.sin_family = AF_INET;
		addr.sin_port = htons(8888);
		addr.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");
	
		if (bind(Serve, (SOCKADDR *)&addr, sizeof(addr)) == SOCKET_ERROR)
		{
			printf("绑定协议地址失败\n");
			closesocket(Serve);
			WSACleanup();
			return -1;
		}
		printf("绑定协议地址成功\n");
		
		if (listen(Serve, 10) == SOCKET_ERROR)
		{
			printf("监听失败\n");
			closesocket(Serve);
			WSACleanup();
			return -1;
		}
		printf("监听成功\n");
	
		sC.push_back(Serve);
		
	
		while (1)
		{
			for (size_t i = 0; i < sC.size(); ++i)
			{
				FD_ZERO(&fd);
				FD_SET(sC.at(i), &fd);
				ret = select(1, &fd, nullptr, nullptr, &time);
				if (ret < 0)
				{
					printf("select错误\n");
					break;
				}
				else if (ret == 0)
				{
					continue;
				}
				else
				{
					if (FD_ISSET(sC.at(i), &fd))
					{
						if (i == 0)
						{
							SOCKADDR_IN client_addr = { 0 };
							int len = sizeof(client_addr);
							SOCKET Client = accept(Serve, (SOCKADDR *)&client_addr, &len);
							if (INVALID_SOCKET == Client)
							{
								printf("连接失败\n");
								closesocket(Serve);
								WSACleanup();//关闭套接字
								return -1;
							}
							sC.push_back(Client);
							printf("连接成功\n");
						}
						else
						{
							char buff[128] = { 0 };
							if (recv(sC.at(i), buff, sizeof(buff) - 1, 0) > 0)
							{
								printf("接收信息：%s\n", buff);
							}
						}
					}
					
				}
			}
		}
		return 0;
	
	}


客户端：


	#include "stdafx.h"
	
	
	//1.
	#include <WinSock2.h>
	#pragma comment (lib,"ws2_32.lib")
	
	
	
	int _tmain(int argc, _TCHAR* argv[])
	{
		//2.
		WSADATA wsaData;
		WSAStartup(MAKEWORD(2, 2), &wsaData);
	
		if (LOBYTE(wsaData.wVersion) != 2 || HIBYTE(wsaData.wVersion) != 2)
		{
			printf("请求版本失败\n");
			return -1;
		}
		printf("请求版本成功\n");
	
		//3.
		SOCKET client = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	
		if (INVALID_SOCKET == client)
		{
			printf("创建SOCKET失败\n");
			WSACleanup();
			return -1;
		}
	
		//4.
		SOCKADDR_IN addr = { 0 };
	
		addr.sin_family = AF_INET;
		addr.sin_port = htons(8888);
		addr.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");
	
		
		//5.
		if (connect(client, (SOCKADDR*)&addr, sizeof(addr)) == SOCKET_ERROR)
		{
			printf("连接失败\n");
			closesocket(client);
			WSACleanup();
			return -1;
		}
		
		printf("连接成功\n");
		char buff[128] = { 0 };
		char rbuff[128] = { 0 };
		while (1)
		{
			ZeroMemory(buff, sizeof(buff));
			ZeroMemory(rbuff, sizeof(rbuff));
			scanf_s("%s", buff, sizeof(buff) - 1);
			if (send(client,buff,strlen(buff),0) > 0)
			{
				printf("发送成功\n");
			}
		}
	
	
	
		//9.
		closesocket(client);
		WSACleanup();
		return 0;
	}
