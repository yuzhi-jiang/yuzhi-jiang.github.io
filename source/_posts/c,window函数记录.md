---
title: 'c,window函数记录'
categories: []
date: 2020-03-22 08:40:18
tags:
---

<!--more-->

头文件
#include<windows.h>

函数：获取和设置鼠标坐标

```
POINT mouse;
GetCursorPos(&mouse);//获取坐标
SetCursorPos(mouse.x,mouse.y);//设置坐标
```

```
HWND window;    //定义一个窗口句柄变量，用来储存窗口句柄
/*
FindWindow("这里填窗口类名","这里填窗口标题名")
    窗口类名和窗口标题名可以只填一个，不填的用NULL填充
*/
window = FindWindow(NULL,"out.txt - 记事本");  //查找标题为"out.txt - 记事本"的窗口
SendMessage(window,WM_CLOSE,0,0);              //向窗口发送关闭指令
```

```
POINT mouse;
 HWND window;
 GetCursorPos(&mouse);
 char ch;
 scanf("%c",&ch);
        window = WindowFromPoint(mouse);
        /*SendMessage(窗口句柄,消息类型,消息附带内容,消息附带内容)
        比如我这里选定的消息类型是WM_CHAR
        消息附带内容为WPARAM(ch)//如：ch==a,则输入a
        所以消息附带内容就是模拟键盘向窗口输入ch*/
        SendMessage(window,WM_CHAR,WPARAM(ch),0);
```

```
HWND window;
	window = FindWindow(NULL,"out.txt - 记事本");
	ShowWindow(window,SW_HIDE); //sh_hide               //隐藏窗口
	Sleep(5000);
	ShowWindow(window,SW_MAXIMIZE);  //sw_maximize          //最大化窗口
	Sleep(5000);
	ShowWindow(window,SW_MINIMIZE); //minimize           //最小化窗口
	Sleep(5000);
	ShowWindow(window,SW_RESTORE);     //restore        //还原窗口
	Sleep(5000);
```

```
#define KEY_DOWN(VK_NONAME) ((GetAsyncKeyState(VK_NONAME) & 0x8000) ? 1:0) //背下来
KEY_DOWN(VK_SPACE)){//VK_SPACE 是空格的虚拟键值 //按下空格
KEY_DOWN('W')//按下W

模拟鼠标点击左键
mouse_event(MOUSEEVENTF_LEFTDOWN,0,0,0,0);//按下
		Sleep(100);//要留给某些应用的反应时间 		mouse_event(MOUSEEVENTF_LEFTUP,0,0,0,0);//抬起

右键
mouse_event(MOUSEEVENTF_RIGHTDOWN,0,0,0,0);
		Sleep(100); 
		mouse_event(MOUSEEVENTF_RIGHTUP,0,0,0,0);
```
//示例代码：

```
#include<iostream>
#include<windows.h>
 
using namespace std;
 
int main(){
	POINT p; 
	while(1){
		GetCursorPos(&p);//获取鼠标坐标 
		SetCursorPos(p.x+2,p.y);//更改鼠标坐标 
		Sleep(10);//控制移动时间间隔 
	}
	
	return 0;
} 
```

```
#include <iostream>
#include<windows.h>

int main() {
    HWND window;    //定义一个窗口句柄变量，用来储存窗口句柄
    /*FindWindow("这里填窗口类名","这里填窗口标题名")
    窗口类名和窗口标题名可以只填一个，不填的用NULL填充*/
    window = FindWindow(NULL,"out.txt - 记事本");  //查找标题为"out.txt - 记事本"的窗口

    SendMessage(window,WM_CLOSE,0,0);              //向窗口发送关闭指令

    return 0;
}
```

```
#include <iostream>
#include<windows.h>

int main() {
    POINT mouse;
    HWND window;
    while (1) {
        GetCursorPos(&mouse);
        window = WindowFromPoint(mouse);
        /*SendMessage(窗口句柄,消息类型,消息附带内容,消息附带内容)
        比如我这里选定的消息类型是WM_CHAR
        消息附带内容为WPARAM('c')
        所以消息附带内容就是模拟键盘向窗口输入c*/
        SendMessage(window,WM_CHAR,WPARAM('c'),0);//鼠标位置输入c
        Sleep(100);
    }
    return 0;
}
```

```
#include <iostream>
#include<windows.h>

int main() {
    POINT mouse;        //储存鼠标位置
    HWND window;
    
        GetCursorPos(&mouse);   //获取到当前鼠标位置
        /*WindowFromPoint(鼠标位置变量名)*/
        window = WindowFromPoint(mouse);
        SendMessage(window,WM_CLOSE,0,0);//关闭鼠标所在窗口 
        Sleep(100);
    
    return 0;
}
```

```
#include<windows.h>
#include<time.h>
int main(){
	HWND window;
	window = FindWindow(NULL,"out.txt - 记事本");//要先打开此记事本
	ShowWindow(window,SW_HIDE); //sh_hide               //隐藏窗口
	Sleep(5000);
	ShowWindow(window,SW_MAXIMIZE);  //sw_maximize          //最大化窗口
	Sleep(5000);
	ShowWindow(window,SW_MINIMIZE); //minimize           //最小化窗口
	Sleep(5000);
	ShowWindow(window,SW_RESTORE);     //restore        //还原窗口
	Sleep(5000);
	return 0;
}
```
[原文参考链接](https://blog.csdn.net/everlasting_20141622/article/details/52229887)
```
//网上的，侵删
#include<iostream>
#include<conio.h>
#include<windows.h>
 
#define KEY_DOWN(VK_NONAME) ((GetAsyncKeyState(VK_NONAME) & 0x8000) ? 1:0) //必要的，我是背下来的 
 
using namespace std;
 
int main(){
	char a;
	int now=0;
	printf("按Q开始左键点击\n");
	printf("按W开始右键点击\n");
	printf("按空格停止点击\n");
	while(1){
		if(KEY_DOWN(VK_SPACE)){//VK_SPACE 是空格的虚拟键值 
			now=0;
			Sleep(100);//你的手不会再一瞬间送开，所以要处理一下 
		}
		if(KEY_DOWN('Q')){
			now=1;
			Sleep(100);
		}
		if(KEY_DOWN('W')){
			now=2;
			Sleep(100);
		}
		if(now==1){//模拟点击左键 
			mouse_event(MOUSEEVENTF_LEFTDOWN,0,0,0,0);
			Sleep(100);//要留给某些应用的反应时间 
			mouse_event(MOUSEEVENTF_LEFTUP,0,0,0,0);
		}
		if(now==2){//模拟点击右键 
			mouse_event(MOUSEEVENTF_RIGHTDOWN,0,0,0,0);
			Sleep(100); 
			mouse_event(MOUSEEVENTF_RIGHTUP,0,0,0,0);
		}
		
		Sleep(200);//点击间隔 单位是毫秒 
	}
}
```

