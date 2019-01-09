# socket


#server端
> 1.加载套接字，创建套接字（WSAStartup()/socket）
> 
> 2.绑定套接字到一个ip地址和端口（bind()）
> 
> 3.套接字设置监听模式等待连接请求（listen()）
> 
> 4.请求到来，接受连接请求，返回一个新的对应于此次连接的套接字（accept()）
> 
> 5.用返回的套接字和client进行通信（send()/recv()）
> 
> 6.返回 等待另一个请求
> 
> 7.关闭套接字，关闭加载的套接字库（closesocket()/WSAClean()）


    #include <iostream>
    
    #include <winSock2.h>
    
    #pragma comment (lib,"ws2_32.lib")
    
    using namespace std;
    
    #define BUF_SIZE 128
    
    int main()
    {
    	WSADATA wsd;
    
    	char  sendBuf[BUF_SIZE];
    	char  recvBuf[BUF_SIZE];
    
    	//加载套接字库  WSAStartup()
    	if (WSAStartup(MAKEWORD(2, 2), &wsd) != 0)
    	{
    		cout << "WSAStartup failed" << endl;
    		return 1;
    	}
    
    	//创建套接字  socket()
    	SOCKET wServer = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    	if (INVALID_SOCKET == wServer)
    	{
    		cout << "socket failed" << endl;
    
    		WSACleanup();
    		return 1;
    	}
    
    	//绑定ip  端口 bind()
    	/*
    		bind(SOCKET s,const sockaddr *addr,int  namelen)
    
    		s 是要连接的套接字
    
    	*/
    	sockaddr_in addr;
    	addr.sin_family = AF_INET;
    	addr.sin_addr.s_addr = inet_addr("127.0.0.1");
    	addr.sin_port = htons(4999);
    
    	if (SOCKET_ERROR == bind(wServer, (sockaddr *)&addr, sizeof(addr)))
    	{
    		cout << "bind failed" << endl;
    		closesocket(wServer);
    		WSACleanup();
    		return 1;
    	}
    
    	//设置监听模式
    	if (SOCKET_ERROR == listen(wServer, 1))
    	{
    		cout << "listen failed" << endl;
    		closesocket(wServer);
    		WSACleanup();
    		return 1;
    	}
    
    
    	//接受客户端的连接  accept
    	SOCKET sClient = accept(wServer, 0, 0);
    	if (INVALID_SOCKET == sClient)
    	{
    		cout << "accept failed" << endl;
    		closesocket(wServer);
    		WSACleanup();
    		return 1;
    	}
    
    	while (true)
    	{
    		ZeroMemory(recvBuf, BUF_SIZE);//先清空
    		recv(sClient, recvBuf, BUF_SIZE, 0);//再接收
    		cout << "服务器接收的消息:" << recvBuf << endl;
    
    		if (recvBuf[0] == 0)
    		{
    			break;
    		}
    
    		ZeroMemory(sendBuf, BUF_SIZE);//先清空
    		cout << "要发送给客服端的消息" << endl;
    		cin >> sendBuf;
    		send(sClient, sendBuf, BUF_SIZE, 0);
    	}
    	closesocket(wServer);
    	closesocket(sClient);
    	WSACleanup();
    	return 0;
    }


#client端
> 1.加载套接字，创建套接字（WSAStartup()/socket）
> 
> 2.向服务器发出连接请求（connect()）
> 
> 3.和服务器进行通信（send()/recv()）
> 
> 4.关闭套接字，关闭加载的套接字库（closesocket()/WSAClean()）


    #include "winsock2.h"  
    #include <iostream>  
    #pragma comment(lib, "ws2_32.lib")  
    
    using namespace std;
    
    #define BUF_SIZE 128
    
    int main()
    {
    	WSADATA wsd;
    
    	char  sendBuf[BUF_SIZE];
    	char  recvBuf[BUF_SIZE];
    
    
    	//加载套接字库  WSAStartup()
    	if (WSAStartup(MAKEWORD(2, 2), &wsd) != 0)
    	{
    		cout << "WSAStartup failed" << endl;
    		return 1;
    	}
    
    	/*
    		socket(int af,int type, int protocol)
    
    		af 参数表示互联网协议版本   AF_INET 是 ipv4
    		type表示使用哪种socket, 
    		protocol表示数据传输协议  IPPROTO_TCP 是 tcp协议
    	*/
    
    	//创建套接字  socket()
    	SOCKET wServer = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    
    	if (INVALID_SOCKET == wServer)
    	{
    		cout << "socket failed" << endl;
    
    		WSACleanup();
    		return 1;
    	}
    
    
    	/*连接套接字   connect(SOCKET s,const sockaddr *name,int namelen)
    	 s  表示要连接的socket
    
    	*/
    	sockaddr_in addr;
    	addr.sin_family = AF_INET;
    	addr.sin_addr.s_addr = inet_addr("127.0.0.1");
    	addr.sin_port = htons(4999);
    
    	if (SOCKET_ERROR == connect(wServer, (sockaddr*)&addr, sizeof(addr)))
    	{
    		cout << "connect failed" << endl;
    
    		WSACleanup();
    		return 1;
    	}
    	int results;
    	while (true)
    	{
    		ZeroMemory(sendBuf, BUF_SIZE);//先清空
    		cout << "要发送给服务器的消息" << endl;
    		cin >> sendBuf;
    		results = send(wServer, sendBuf, BUF_SIZE, 0);
    		if (SOCKET_ERROR == results)
    		{
    			cout << "connect failed" << endl;
    			closesocket(wServer);
    			WSACleanup();
    			return 1;
    		}
    		ZeroMemory(recvBuf, BUF_SIZE);//先清空
    		recv(wServer, recvBuf, BUF_SIZE, 0);//再接收
    		cout << "服务器返回的消息:" << recvBuf << endl;
    
    	}
    	closesocket(wServer);
    	WSACleanup();
    
    	return 0;
    }
