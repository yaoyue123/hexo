---
title: 使用SetWindowsHookEx进行dll注入
date: 2021-01-25 00:00:00
categories: Code
tags: windows, process-inject, secuity, SetWindowsHookEx
cover: https://p1.ssl.qhimg.com/t011b2047aa5101086b.jpg
description: 通过SetWindowsHookEx钩子进行dll注入
---

# 相关函数介绍

钩子(Hook)，是Windows消息处理机制的一个平台，应用程序可以在上面设置子程以监视指定窗口的某种消息，而且所监视的窗口可以是其他进程所创建的。当消息到达后，在目标窗口处理函数之前处理它。钩子机制允许应用程序截获处理window消息或特定事件。

钩子实际上是一个处理消息的程序段，通过系统调用，把它挂入系统。每当特定的消息发出，在没有到达目的窗口前，钩子程序就先捕获该消息，亦即钩子函数先得到控制权。这时钩子函数即可以加工处理（改变）该消息，也可以不作处理而继续传递该消息，还可以强制结束消息的传递。

我们通常使用通常使用SetWindowsHookEx来安装消息钩子，函数原型如下：

```c++
//SetWindowsHookEx
HHOOK SetWindowsHookExA(
  int       idHook,//设置钩子的类型.意思就是我要设置的钩子是什么钩子. 可以是监视窗口过程.可以是监视消息队列.
  HOOKPROC  lpfn, //根据钩子类型.设置不同的回调函数.
  HINSTANCE hmod, //钩子设置的Dll实例句柄,就是DLL的句柄
  DWORD     dwThreadId //设置钩子的线程ID. 如果为0 则设置为全局钩子.
); // HHOOK 返回值. 是一个钩子过程句柄.
```

idHook钩子类型如下

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644975(v=vs.85))

****
[](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nc-winuser-hookproc)

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644977(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644978(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644980(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644981(v=vs.85))

****
[](https://docs.microsoft.com/en-us/windows/desktop/winmsg/about-hooks)[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644982(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644983(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644984(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644985(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644988(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644986(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644987(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644991(v=vs.85))

****
[](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms644992(v=vs.85))

```c++
BOOL InjectDllBySetWindowsHook(ULONG32 ulTargetProcessID)
{
	HANDLE  TargetProcessHandle = NULL;
	TargetProcessHandle = OpenProcess(PROCESS_ALL_ACCESS, FALSE, ulTargetProcessID);

	if (NULL == TargetProcessHandle)
	{
		return FALSE;
	}
	HMODULE DllModule;
#ifdef _WIN64
	DllModule = LoadLibrary(L"E:\\Dll644.dll");
#else
	DllModule = LoadLibrary(L"E:\\Dll.dll");
#endif

	if (DllModule == NULL)
	{
		printf("Can Not Find Dll");
		return FALSE;
	}

	HOOKPROC   Func_Address = NULL;
	Func_Address = (HOOKPROC)GetProcAddress(DllModule, "OurFunction");
	if (Func_Address == NULL)
	{
		printf("function do not Exist!");
		return FALSE;
	}

	DWORD ThreadID = getThreadID(ulTargetProcessID);

	HHOOK Handle = SetWindowsHookEx(WH_KEYBOARD,
		Func_Address, DllModule, ThreadID);

	if (Handle == NULL)
	{
		printf("Hook Failed!");
		return FALSE;
	}
	printf("Hook Success");
	getchar();
	UnhookWindowsHookEx(Handle);
	FreeLibrary(DllModule);
	return true;
}
```
