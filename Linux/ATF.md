# ATF 原理分析  
    ATF将系统启动从最底层进行了完整的统一划分，  
    将secure monitor的功能放到了bl31中进行，这样当 系统完全启动之后，  
    在CA或者TEE OS中触发了smc或者是其他的中断之后，  
    首先是遍历注册到bl31中的对应的service来判定具体的handle，  
    这样可以对系统所有的关键smc或者是中断操作做统一的管理和分配。  
    ATF的code boot整个启动过程框图如下：  
    在上述启动过程中，每个Image跳转到写一个image的方式各不相同，  
    下面将列出启动过程中每个image跳转到下一个image的过程：

## 1. bl1跳转到bl2执行
    在bl1完成了bl2 image加载到RAM中的操作，中断向量表设定以及其他CPU相关设定之后，  
    在bl1_main函数中解析出bl2 image的描述信息，获取入口地址，  
    并设定下一个阶段的cpu上下文，完成之后，  
    调用el3_exit函数实现bl1到bl2的跳转操作，进入到bl2中执行.

## 2.bl2跳转到bl31执行
    在bl2中将会加载bl31, bl32, bl33的image到对应权限的RAM中，  
    并将该三个image的描述信息组成一个链表保存起来，  
     以备bl31启动bl32和bl33使用.在AACH64中，bl31位EL3 runtime software，  
     运行时的主要功能是管理smc指令的处理和中断的主力，运行在secure monitor状态中  
     bl32一般为TEE OS image，本章节以OP-TEE为例进行说明bl33为非安全image，例如uboot, linux  
     kernel等，当前该部分为bootloader部分的image，再由bootloader来启动linux kernel.  
     
    从bl2跳转到bl31是通过带入bl31的entry point info调用smc指令触发在bl1中  
    设定的smc异常来通过cpu将全向交给bl31并跳转到bl31中执行。

## 3.bl31跳转到bl32执行
    在bl31中会执行runtime_service_inti操作，  
    该函数会调用注册到EL3中所有service的init函数，其中有一个service就是为TEE服务，  
    该service的init函数会将TEE OS的初始化函数赋值给bl32_init变量，  
    当所有的service执行完init后，在bl31中会调用bl32_init执行的函数来跳转到TEE OS的执行

## 4.bl31跳转到bl33执行
    当TEE_OS image启动完成之后会触发一个ID为  
    TEESMC_OPTEED_RETURN_ENTRY_DONE的smc调用来告知EL3 TEE OS image  
    已经完成了初始化，然后将CPU的状态恢复到bl31_init的位置继续执行。  
    
    bl31通过遍历在bl2中记录的image链表来找到需要执行的bl33的image。  
    然后通过获取到bl33 image的镜像信息，设定下一个阶段的CPU上下文，  
    退出el3然后进入到bl33 image的执行
    
## 资料
http://harmonyhu.com/2018/06/23/Arm-trusted-firmware/