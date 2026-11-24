# CanalotuUefi

此代码库是无需edk2构建系统的UEFI库，使用BSD-2-Clause-Patent进行开源。  
CanalotuUefi基于edk2，使用C++重写，适用于MSVC，仅支持x64.  

更新进度取决于我的需求。  

---

## 使用方法

### 1. 重写以下函数

VOID EFIAPI ProcessLibraryConstructorList(IN EFI_HANDLE ImageHandle, IN EFI_SYSTEM_TABLE* SystemTable);  

EFI_STATUS EFIAPI ProcessModuleEntryPointList(IN EFI_HANDLE ImageHandle, IN EFI_SYSTEM_TABLE* SystemTable);  

VOID EFIAPI ProcessLibraryDestructorList(IN EFI_HANDLE ImageHandle, IN EFI_SYSTEM_TABLE* SystemTable);  

示例见example/UefiMain.cpp  

### 2. 编译/链接选项

使用此代码库进行开发，需要对你的UEFI程序项目进行额外设置，可以直接复制example/UefiMain.vsxproj，也可以自行设置。  

以下是在Visual Studio 2026中建立C++空项目时需要做出的调整，很多设置都是默认就行，但是以防万一仍列出。  

#### 2.1 VC++目录

（1）删除 包含目录  
（2）删除 外部包含目录  
（3）删除 引用目录  
（4）删除 库目录  
（5）删除 WIndows运行库目录  

#### 2.2 C/C++
2.2.1 常规
（1）附加包含目录 = $(SolutionDir)Uefi\Include  
（2）[Release]调试信息格式 = 无  
（3）公共语言运行时支持 = 无公共语言运行时支持  
（4）使用Windows运行时扩展 = 否  
（5）警告等级 = 启用所有警告（/Wall）  
（6）SDL检查 = 否（/sdl-）  
2.2.2 优化
（1）优选大小速度 = 代码速度优先（/Ot）  
2.2.3 预处理器  
（1）删除 预处理器定义  
2.2.4 代码生成  
（1）启用C++异常 = 否  
（2）基本运行时检查 = 默认值  
（3）安全检查 = 禁用安全检查（/GS-）  
2.2.5 语言  
（1）禁用语言扩展 = 是（/Za）  
（2）将WChar_t视为内置类型 = 否（/Zc:wchar_t-）  
（3）启用运行时类型信息 = 否（/GR-）  
（4）OpenMP支持 = 否（/openmp-）  
（5）C++语言标准 = ISO C++20标准（/std:c++20）  
（6）C语言标准 = ISO C17标准（/std:c17）  
2.2.6 高级  
（1）禁用特定警告 = 5045  

#### 2.3 链接器
2.3.1 常规  
（1）[Release]启用增量链接 = 否（/INCREMENTAL:NO）  
2.3.2 输入  
（1）忽略所有默认库 = 是 (/NODEFAULTLIB)  
2.3.3 清单文件  
（1）生成清单 = 否 (/MANIFEST:NO)  
（2）启用用户账户控制(UAC) = 否 (/MANIFESTUAC:NO)  
2.3.4 调试  
（1）[Release]生成调试信息 = 否  
2.3.5 系统  
（1）子系统 = EFI 应用程序 (/SUBSYSTEM:EFI_APPLICATION)  
2.3.6 优化  
（1）引用 = 否 (/OPT:NOREF)  
（2）启用COMDAT折叠 = 否 (/OPT:NOICF)  
2.3.7 Windows元数据  
（1）生成Windows元数据 = 否 (/WINMD:NO)  
2.3.8 高级  
（1）入口点 = EfiMain  
（2）基址 = 0  
（3）随机基址 = 否 (/DYNAMICBASE:NO)  
（4）固定基址 = 否 (/FIXED:NO)  
（5）目标计算机 = MachineX64 (/MACHINE:X64)  