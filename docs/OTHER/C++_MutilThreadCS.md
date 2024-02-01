## 简介

在windows平台上基于C++的C/S模式多线程,主要包括多线程的创建与使用、基础的网络编程（SOCKET），推荐使用已有的开源包，例如：https、grpc等，首推grpc，可以跨平台且使用简便，容错性高。

## 源码

```
// server.cpp : 此文件包含 "main" 函数。程序执行将在此处开始并结束。

#define _WINSOCK_DEPRECATED_NO_WARNINGS
#include <iostream>
#include <WinSock2.h>
#include <string.h>
#include <string>
#include <Windows.h>
#include <process.h>
#include <fstream>
using namespace std;

#define MAXBUFFER 0xff
#define MAXCLIENT 0xff
//返回$代表登录失败，其他代表登录成功
string logIn(const string& id, const string& passwd) {
    ifstream inFile(".\\idInfor.txt");
    string name, locId, locPasswd, mes;
    while (inFile >> name >> locId >> locPasswd) {
        if (id == locId && passwd == locPasswd) {
            mes = name;
            return mes;
        }
    }
    mes = "$";
    return mes;
}
typedef struct Param {
    SOCKET sockCli;
    sockaddr_in addrCli;
}Param;
Param clientPar[MAXCLIENT];
void serverThread(void* Paramter)
{
    Param* par = (Param*)Paramter;
    char ch[MAXBUFFER];
    string id, passwd, mes;
    int ret;
    cout << "client address:" << par->addrCli.sin_port << endl;
    while (1) {
        ret = recv(par->sockCli, ch, MAXBUFFER, 0);
        id = ch;
        if (ret <= 0) {
            cout << "fail to receiv data from client" << endl;
            break;
        }
        ret = recv(par->sockCli, ch, MAXBUFFER, 0);
        passwd = ch;
        cout << "id:" << id << " passwd:" << passwd << endl;
        mes = logIn(id, passwd);
        if (ret <= 0) {
            cout << "fail to receiv data from client" << endl;
            break;
        }
        ret = send(par->sockCli, mes.data(), MAXBUFFER, 0);
        if (ret == -1) {
            cout << "fail to send message to client";
            break;
        }
    }
    cout << " client has exited" << endl;
    _endthread();
}
int main()
{
#pragma comment(lib,"ws2_32.lib")
    WSADATA data;
    WORD wdVesion = MAKEWORD(2, 2);
    int ret = WSAStartup(wdVesion, &data);
    if (ret) {
        cerr << "initial network error!" << endl;
        return -1;
    }
    SOCKET sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock == -1) {
        cerr << "fail to create socket" << endl;
        WSACleanup();
        return -1;
    }
    sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(9999);
    addr.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");
    ret = bind(sock, (sockaddr*)&addr, sizeof(addr));
    if (ret == -1) {
        cerr << "fail to bind address and port";
        WSACleanup();
        return -1;
    }
    ret = listen(sock, 5);
    if (ret == -1) {
        cerr << "fail to listen socket";
        WSACleanup();
        return -1;
    }
    int len = sizeof(clientPar[0].addrCli), i = 0;
//接收客户端连接，创建线程
    while (1) {
//TODO clientPar参数未实现循环利用
        Param* par = &clientPar[i];
        par->sockCli = accept(sock, (sockaddr*)&(par->addrCli), &len);
        if (par->sockCli == -1) {
            cerr << "fail to connect to client" << endl;
            WSACleanup();
            return -1;
        }
        HANDLE hThread = (HANDLE)_beginthread(serverThread, 0, (void*)par);
        if (hThread == 0) {
            cerr << "fail to create pthread";
            WSACleanup();
            return -1;
        }
        i++;
        if (i >= MAXCLIENT) {
            cout << "the number of client is max" << endl;
            WSACleanup();
            return -1;
        }
    }
    WSACleanup();
    return 0;
}
```

