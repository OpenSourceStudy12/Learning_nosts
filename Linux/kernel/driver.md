# kernel 启动过程中 driver 的加载
- linux 中有两方式加载系统中的模块：动态和静态
## 静态加载
- 将所有模块编译到内核中，由
```
do_initcalls(0)
```
进行加载。  
- location : kernel/init/main.c

```
start_kernel-->rest_init-->kernel_init-->kernel_init_freeable-->do_basic_setup-->do_initcalls
```
do_initcalls 会根据优先级顺序先后加载模块，相同优先级的模块取决于链接的顺序，在 Makefile 中修改，在 system.map 中可以查看各模块的加载顺序

- 模块优先级：定义在 kernel/include/linux/init.h

```
#define pure_initcall(fn)		__define_initcall(fn, 0)  
#define core_initcall(fn)	  	__define_initcall(fn, 1)  
#define core_initcall_sync(fn)   		__define_initcall(fn, 1s)
#define postcore_initcall(fn)		__define_initcall(fn, 2)
#define postcore_initcall_sync(fn)	__define_initcall(fn, 2s)
#define arch_initcall(fn)		__define_initcall(fn, 3)
#define arch_initcall_sync(fn)		__define_initcall(fn, 3s)
#define subsys_initcall(fn)		__define_initcall(fn, 4)
#define subsys_initcall_sync(fn)	__define_initcall(fn, 4s)
#define fs_initcall(fn)			__define_initcall(fn, 5)
#define fs_initcall_sync(fn)		__define_initcall(fn, 5s)
#define rootfs_initcall(fn)		__define_initcall(fn, rootfs)
#define device_initcall(fn)		__define_initcall(fn, 6)
#define device_initcall_sync(fn)	__define_initcall(fn, 6s)
#define late_initcall(fn)		__define_initcall(fn, 7)
#define late_initcall_sync(fn)		__define_initcall(fn, 7s)


#define __define_initcall(fn, id) \
	static initcall_t __initcall_##fn##id __used \
	__attribute__((__section__(".initcall" #id ".init"))) = fn; \
	LTO_REFERENCE_INITCALL(__initcall_##fn##id)
	
```
从上到下优先级依次减小，通常看到的模块优先级数为6
```
#define __initcall(fn) device_initcall(fn)
```
在 kernel/include/linux/module.h 中
```
#define module_init(X) __initcall(X)
```

- ".initcall" #id ".init" 定义在 vmlinux.lds 中：

```
 .init.data : {
  *(.init.data) *(.meminit.data) *(.init.rodata) . = ALIGN(8); __start_ftrace_events = .; *(_ftrace_events) __stop_ftrace_events = .; __start_ftrace_enum_maps = .; *(_ftrace_enum_map) __stop_ftrace_enum_maps = .; *(.meminit.rodata) . = ALIGN(8); __clk_of_table = .; *(__clk_of_table) *(__clk_of_table_end) . = ALIGN(8); __reservedmem_of_table = .; *(__reservedmem_of_table) *(__reservedmem_of_table_end) . = ALIGN(8); __clksrc_of_table = .; *(__clksrc_of_table) *(__clksrc_of_table_end) . = ALIGN(8); __iommu_of_table = .; *(__iommu_of_table) *(__iommu_of_table_end) . = ALIGN(8); __cpu_method_of_table = .; *(__cpu_method_of_table) *(__cpu_method_of_table_end) . = ALIGN(8); __cpuidle_method_of_table = .; *(__cpuidle_method_of_table) *(__cpuidle_method_of_table_end) . = ALIGN(32); __dtb_start = .; *(.dtb.init.rodata) __dtb_end = .; . = ALIGN(8); __irqchip_of_table = .; *(__irqchip_of_table) *(__irqchip_of_table_end) . = ALIGN(32); __earlycon_table = .; *(__earlycon_table) *(__earlycon_table_end) . = ALIGN(8); __earlycon_of_table = .; *(__earlycon_of_table) *(__earlycon_of_table_end)
  . = ALIGN(16); __setup_start = .; *(.init.setup) __setup_end = .;
  __initcall_start = .; *(.initcallearly.init) __initcall0_start = .; *(.initcall0.init) *(.initcall0s.init) __initcall1_start = .; *(.initcall1.init) *(.initcall1s.init) __initcall2_start = .; *(.initcall2.init) *(.initcall2s.init) __initcall3_start = .; *(.initcall3.init) *(.initcall3s.init) __initcall4_start = .; *(.initcall4.init) *(.initcall4s.init) __initcall5_start = .; *(.initcall5.init) *(.initcall5s.init) __initcallrootfs_start = .; *(.initcallrootfs.init) *(.initcallrootfss.init) __initcall6_start = .; *(.initcall6.init) *(.initcall6s.init) __initcall7_start = .; *(.initcall7.init) *(.initcall7s.init) __initcall_end = .;
  __con_initcall_start = .; *(.con_initcall.init) __con_initcall_end = .;
  __security_initcall_start = .; *(.security_initcall.init) __security_initcall_end = .;
  . = ALIGN(4); __initramfs_start = .; *(.init.ramfs) . = ALIGN(8); *(.init.ramfs.info)
 }
```
## 动态加载

