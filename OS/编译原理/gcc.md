-c:编译目标文件，不会连接库  
-S:编译汇编代码  
-E:预处理，与处理之后的代码将送往标准输出  
-g:产生供gdb调试用的可执行文件  
-Wwarn:设置警告，通常用-Wall 开启所有警告  
-Olevel:设置优化级别，level 可以是0,1,2...，默认-O0，不进行优化  
-Dname=definition... :在命令行定义宏有两种方式，-Dname 或 -Dname=definition  
-Uname:取消宏定义name，  
-Idir:将dir加到头文件的搜索路径中，gcc会在搜索标准头文件之前先搜索dir  
-llibrary:在连接时搜索library库，库是一些archieve文件,其成员是目标文件.如果有文件引用library,library在命令行的位置应该在那个文件之后,因此,越底层的库越要放在后面.比如如果你要连接pcap库,那么你就需要使用-lpcap对源文件进行编译 -Ldir:把dir加到库文件的搜索路径中，gcc会在搜索标准库文件之前先搜索dir  
-pthread:通过pthreads库加入对多线程的支持，通过pthreads库加入对多线程的支持,这为预处理和连接设置了标志.pthread是POSIX指定的标准线程库.  
-std=standard 设置采用的标准,该选项是针对C语言的,比如-std=c99表示编译器遵循C99标准.该选项较少使用.而且有时反而会把你搞糊涂.  
-o outfile 指定输出文件的文件名,默认为a.out  
