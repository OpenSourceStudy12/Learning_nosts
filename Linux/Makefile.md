# 基本语法
## = 、：=、 ？= 的区别
- = 赋值
- := 清空之前的值，赋新值
- ?= 若变量值为空，则赋值 

## wildcard、notdir、patsubst
- wildcard 扩展通配符 eg: src=$(wildcard ./*.c) 获取当前目录下所有.c文件
- notdir 去除路径 eg: dir=$(notdir ./\*.c ./build/\*.c) dir=*.c 去除了./build/\*.c 文件的路径
- patsubst 替换通配符 eg: obj=$(patsubst *.c *.o $(dir)) 将dir中所有\*.c 替换成\*.o 

# 变量
- MAKECMDGOALS  
记录了命令行参数行指定的目标列表

# 常用函数
- Makefile 中函数调用以 ${} 或 $() 方式调用
- 函数名与变量以空格分开，参数之间以逗号分离
## 字符串操作函数
### $(subst ,,)
- 字符串处理函数
- 功能：替换符串中的字串
- 返回被替换后的字符串
- eg :$(subst ee,EE,feet on the street) 将feet 中的ee替换成EE

### $(patsubst ,,)
- 模式字符串处理函数
- 功能：查找单词（以空格、tab、或回车、换行分隔）是否符合模式，若匹配则替换
- 返回被替换后的字符串
- eg：$(patsubst %.c,%.o,x.c.c bar.c) :x.c.c 和 bar.c 被替换成 x.c.o 和 bar.o

### $(strip )
- 去空格函数
- 在字符串中开头和结尾的空字符
- 返回去掉首尾空格的字符串
- eg：$(strip a b c )：去掉"a b c " 成 "a b c"

### $(findtring ,)
- 查找字符串函数
- 在字符串中查找字串
- 返回：找到返回，否则返回空
- eg：$(findstring a,a b c):返回a

### $(filter ,)
- 过滤函数
- 以模式过滤字符产中的单词，保留复合模式的单词，可以有多个模式
- 返回符合模式的单词
- eg：  
 source := foo.c bar.c baz.s ugh.h  
 foo : $(source)  
 cc = $(filter %.c %.s,$(source))
 则 cc = foo.c bar.c baz.s
### $(filter-out ,)
- 反过滤函数
- 以模式过滤字符产中的单词，去除符合模式的单词，可有多个模式
- 返回不符合模式的字串
- eg：
 obj := main1.o foo.o main2.o bar.o
 mains := main1.o main2.o
 则 \$(filter-out \$(mains),\$(obj)) 返回值时"foo.o bar.o"

### $(sort )
- 排序函数
- 功能：给字符串中的单词排序（升序）
- 返回排序后的字符串
- note:sort 会去掉相同的单词
- eg：$(sort foo bar bar lose) 返回"bar foo lose"

### $(word ,)
- 取单此函数
- 功能：字符串的第x个单词
- 返回字符串中的第x个单词，没找到返回空字符
- eg：$(word 2,foo bar baz) 返回 "bar"

### $(wordlist ,,)
- 取单词串函数
- 功能：从字符串中取第x到第y个的单词
- eg：$(wordlist 2,3,foo bar baz) 返回"bar baz"

### $(words )
- 单词个数统计
- 统计字符串中单词个数
- 返回单词个数
- eg：$(words foo bar baz)返回3

### $(firstword )
- 首单词函数
- 功能：取字符串中的第一个单词。
- 返回字符串的第一个单词。
- eg：$(firstword foo bar)返回值是“foo”。
- note：这个函数可以用word函数来实现：$(word
1,)。

## 文件名操作函数
### $(wildcard )
- 文件名展开函数
- 功能：展开成一系列所有符合其参数描述的的文件名，以空格隔开
- 返回展开后的文件名列表
- eg：$(wildcard *.c) 返回所有以.c结尾的文件列表

### $(dir )
- 取目录函数
- 功能：从文件序列中取出目录部分（最后一个"/"之前的部分，若没有"/",则返回"./"）
- eg：$(dir src/foo hacks) 返回"src/ ./"

### $(nodir )
- 取文件函数
- 从文件序列中取出非目录部分（最后一个"/"之后部分）
- 返回文件序列中非目录部分
- eg：$(nodir src/foo hacks) 返回"foo hacks"

### $(suffix )
- 取后缀函数
- 功能：从文件名序列中取出各个文件名的后缀。
- 返回返回文件名序列的后缀序列，如果文件没有后缀，则返回空字串。
- eg：$(suffix src/foo.c src-1.0/bar.c hacks)返回值是“.c
.c”。

### $(basename )
- 取前缀函数——basename。
- 功能：从文件名序列中取出各个文件名的前缀部分。
- 返回返回文件名序列的前缀序列，如果文件没有前缀，则返回空字串。
- eg：$(basename src/foo.c src-1.0/bar.c hacks)返回值是“src/foo
src-1.0/bar hacks”。

### $(addprefix )
- 加前缀函数——addprefix。
- 功能：把前缀加到中的每个单词后面。
- 返回返回加过前缀的文件名序列。
- eg：$(addprefix src/,foo bar)返回值是“src/foo
src/bar”。

### $(join ,)
- 连接函数——join。
- 功能：把中的单词对应地加到的单词后面。如果的单词个数要比的多，那么，中的多出来的单词将保持原样。如果的单词个数要比多，那么，多出来的单词将被复制到中。
- 返回返回连接过后的字符串。
- eg：$(join aaa bbb , 111 222 333)返回值是“aaa111
bbb222 333”。

## foreach 函数
- 把参数中的单词逐一取出放到参数所指定的变量中，然后再执行所包含的表达式。每一次会返回一个字符串，循环过程中，的所返回的每个字符串会以空格分隔，最后当整个循环结束时，所返回的每个字符串所组成的整个字符串（以空格分隔）将会是foreach函数的返回值。
- eg：  
     names := a b c d  
    files := \$(foreach n,\$(names),\$(n).o)  
上面的例子中，\$(name)中的单词会被挨个取出，并存到变量“n”中，“\$(n).o”每次根据“$(n)”计算出一个值，这些值以空格分隔，最后作为foreach函数的返回，所以，\$(files)的值是“a.o b.o c.o d.o”。

## if 函数
- 语法：$(if ,) 或 $(if ,,)
- 

### call 函数
- 语法：$(call ,,,,.......)
- call函数是唯一一个可以用来创建新的参数化的函数。你可以写一个非常复杂的表达式，这个表达式中，你可以定义许多参数，然后你可以用call函数来向这个表达式传递参数

### shell 函数
- 它的参数应该就是操作系统Shell的命令。它和反引号“`”是相同的功能。这就是说，shell函数把执行操作系统命令后的输出作为函数返回
- eg：      
  contents := $(shell cat foo)  
  files := $(shell echo *.c)