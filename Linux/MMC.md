主流的两种分区方式：1、MBR 2、GPT
大家所最为熟知的分区方式同时也是最主流的主要有两种：MBR（Master Boot Record）
和GPT（GUID Partition Table）。前者应用于绝大多数使用BIOS引导的PC设备（苹果使用EFI的方式），
而后者主要是针对MBR的一些缺点进行了改进同时还可以兼容MBR并且支持2TB以上的存储（
MBR不支持2TB以上的存储设备）。
Android 2.x.x 版本上使用的是MBR，4.0版本以后就是使用的GPT分区方式。



SD 传输模式有以下 3 种： 
   SPI mode （required ） 
   1-bit mode 
   4-bit mode 

依据 SD 标准，所有的 SD （记忆卡）与 SDIO （外围）都必须支持 SPI mode ，
因此 SPI mode 是「required 」。

kernel/driver/MMC 
   HOST 部分是针对不同主机的驱动程序，这一部是驱动程序工程师需要根据自己的特点平台来完成的
   CORE 部分: 这是整个MMC 的核心存，这部分完成了不同协议和规范的实现，并为HOST 层的驱动提供了接口函数。 
   CARD 部分：因为这些记忆卡都是块设备，当然需要提供块设备的驱动程序，这部分就是实现了将你的SD 卡如何实现为块设备的。 

LINUX 当中对目录的划分是很有讲究的，这些文件被分布在 3 个目录下，正好对应 MMC/SD 驱动程序的 3 个层次（关于层的划分这里浏览一下，
有个概念即可，当我们分析完了后再回头来看，你会觉得很形象）：

（1)区块层 driver/mmc/card
      主要是按照 LINUX 块设备驱动程序的框架实现一个卡的块设备驱动，这 block.c 当中我们可以看到写一个块设备驱动程序时需要的 
      block_device_operations 结构体变量的定义，其中有 open/release/request 函数的实现，而 queue.c 则是对内核提供的请求队列的封装，
      我们暂时不用深入理解它，只需要知道一个块设备需要一个请求队列就可以了。
（2)核心层 driver/mmc/core
      核心层封装了 MMC/SD 卡的命令，例如存储卡的识别，设置，读写。例如不管什么卡都应该有一些识别，设置，和读写的命令，
      这些流程都是必须要有的，只是具体对于不同的卡会有一些各自特有的操作。 Core.c 文件是由 sd.c 、 mmc.c 两个文件支撑的， 
      core.c 把 MMC 卡、 SD 卡的共性抽象出来，它们的差别由 sd.c 和 sd_ops.c 、 mmc.c 和 mmc_ops.c 来完成。
（3)主机控制器层 driver/mmc/host
      主机控制器则是依赖于不同的平台的，例如 s3c2410 的卡控制器和 atmel 的卡控制器必定是不一样的，所以要针对不同的控制器来实现。
      以 s3cmci.c 为例，它首先要进行一些设置，例如中断函数注册，全能控制器等等。然后它会向 core 层注册一个主机（ host ），
      用结构 mmc_host_ops 描述，这样核心层就可以拿着这个 host 来操作 s3c24xx 的卡控制器了，而具体是 s3c24xx 的卡控制器还是 atmel 的卡控制器， 
      core 层是不用知道的。


Linux 统一设备模型的基本结构

类型                               所包含的内容                     对应内核数据结构                          对应/sys项

设备(Devices)                设备是此模型中最基本的类型，
		             以设备本身的连接按层次组织               struct device                          /sys/devices/*/*/.../ 

设备驱动(Device Drivers)     在一个系统中安装多个相同设备，
				只需要一份驱动程序的支持 		   struct device_driver 		   /sys/bus/pci/drivers/*/ 

总线类型(Bus Types)	   在整个总线级别对此总线上连接的所有
				设备进行管理 			      struct bus_type 			    /sys/bus/*/ 

设备类别(Device Classes)    这是按照功能进行分类组织的设备层次树；
			如 USB 接口和 PS/2 接口的鼠标都是输入设备，
			    都会出现在 /sys/class/input/ 下 		struct class 			     /sys/class/*/ 

