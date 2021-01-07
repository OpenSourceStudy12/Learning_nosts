# 一. shell中的特殊字符
## 1.通配符
*：代表任何字符串  *text* : sstextsd sdfsftextkjfgj  
?: 匹配单个字符  tex? : texd texh  
[]: 代表单个字符的范围 [ab-dm] : a b c d m  
## 2.引号  
单引号' ：由单引号括起来的字符都作为普通字符出现，单引号字符中的变量是无效的，可以作为字符串拼接使用  

$ string = '$PATH'  
$ echo $string  
$ $PATH  

双引号" ：用"" 括起来的字符，除了$,',\,"几个字符仍是特殊字符并保留其特殊功能外，其余字符都作为普通字符

$ TextString = "\$PATH"   
$ echo TextString  
.:/usr/bin:/bin"$PATH  

反引号` : `` 中为命令  

$ pwd  
/home/xyz  
$ Textstr = "current directory is 'pwd'"  
$ echo $Text  
current direction is /home/xyz  

- '' 可以嵌套使用，内层的'' 必须要用\将其转义 
- '' 中特殊字符$,\,'," 仍具有特殊功能
 
# 二 变量
- 局部变量：局部变量在脚本或命令中定义。仅在当前shell中有效，其他shell启动的程序不能访问局部变量  
 局部变量(本地变量)不能被子shell 使用
- 环境变量：所有程序，都能访问环境变量  
 环境变量(全局变量) 可以被子shell 使用，父shell 不能使用子shell 中的全局变量 
- shell变量(系统变量)：由shell 程序设置的特殊变量
- 无论是不是配置文件中的变量，本地变量都不能传给子程序  
- export Var="Var" 创建全局变量，后续子程序可以访问。没有export 的变量为本地变量，后续子程序不能访问  
- 特殊变量  
\$0 当前脚本名  
\$n 传递给脚本或函数的第n个参数  
\$# 传递给脚本或函数的参数个数  
\$* 传递给脚本或函数的所有参数  
\$@ 传递给脚本或函数的所有参数  
\$? 上个命令的退出状态，或函数返回值  
\$$ 当前shell 进程ID。对于shell脚本，就是脚本所在进程ID 
- $# $@ 输出参数形式是"$1" "$2" "$3"...  
 只有"S#" 时输出形式为"$1 $2 $3..."  

# 三 shell 中的替换
## 1.echo 中的转义字符

转义字符 | 含义
---|---
\\\ | 反斜杠
\a | 警报
\b | 退格
\f | 换页，将当前位置移到下页开头
\n | 换行
\r | 回车
\t | 水平制表符(tab键)
\v | 垂直制表符
## 2. 命令替换
- $ directory=\`pwd\`  
$ echo $directory
- wc "`ls table.c`"
## 3. 变量替换
-可以根据变量是否定义，是否为空来改变它的值
形式 | 说明
--- | ---
${var} | 变量的值
${var:-word} | 若变量的值为空或已被删除(unset),则返回word,但不改变var的值
${var:=word} | 若变量的值为空或已被删除(unset),则返回word,同时改变var的值
${var:?message} | 若变量的值为空或已被删除(unset),那么将消息message送到error
${var:+word} | 若var被定义，则返回word,但不改变var

# 四 shell 运算符
## 1.算术运算
原生bash 不支持简单数学运算，用awk 或expr 进行    
运算符 | 说明 | 举例
--- | --- | ---
+ | 加法 | expr $a + $b
- | 减法 | expr $a - $b
* | 乘法 | expr $a \\* $b
/ | 除法 | expr $a / $b
% | 取余 | expr $a % $b
= | 赋值 | a = $b

note:1. expr 中乘为\*
2. expr 中表达式与运算符之间要有空格


## 2. 逻辑运算 
运算符 | 说明 | 举例
--- | --- | ---
== | 相等 | [$a == $b] 返回true
!= | 不等 | [$a != $b] 返回true

## 3. 关系运算
只支持数字
运算符 | 说明 | 举例
--- | --- | ---
-eq | 相等 | [$a -eq $b] 相等返回true
-ne | 不等 | [$a -ne $b] 不等返回true
-gt | 左边>右边 | [$a -gt $b] 返回true
-lt | 左边<右边 | [$a -lt $b] 返回true
-ge | 左边>=右边 | [$a -ge $b] 返回true
-le | 左边<=右边 | [$a -le $b] 返回true

## 4. 布尔运算
运算符 | 说明 | 举例
--- | --- | ---
! | 非运算 | [! false] 返回true
-o | 或运算 | [$a -lt 20 -o $b gt 100]
-a | 与运算 | [$a -lt 20 -a $b gt 100]

## 5. 字符串运算
运算符 | 说明 | 举例
--- | --- | ---
= | 检测两个字符串是否相等 | [$a = $b] 返回 true
!= | 检测两个字符串是否相等 | [$a ！= $b] 返回 true
-z | 检测字符串长度是否为0 | [-z $a] 返回 true
-n | 检测字符串长度是否不为0 | [-n $a] 返回 true
str | 检测字符串是否不为空 | [str $a] 返回 true

- 获取字符串长度：  
    string = "abcd";  
    echo ${#string};--->4  
- 提取字符串  
    string="alibaba is a great company";  
    echo ${string:1:}; -->liba
- 查找字符串  
    string="alibaba is a great company";
    echo 
- 文件路径处理  
1. basename  获取文件名 ，参数：-a 处理多个路径；-s 去掉指定文件的后缀名
```
    $ basename /home/lrh/1.txt  -->1.txt  
    $ basename -a /home/lrh/1.txt /home/lrh/sh.txt --> 1.txt sh.txt  
    $ basename -s .txt /home/lrh/1.txt --> 1  
```
2. dirname  
```
    $ dirname /usr/bin/ --> /usr  
    $ dirname /dir1/str dir2/str --> /dir1  /dir2 
    $ dirname stdio.h --> .
```
- shell中#*,##*,#*,##*,% *,%% *的含义及用法
file=/dir1/dir2/dir3/my.file.txt  
可以用${ }分别替换得到不同的值：  
```
${file#*/}：删掉第一个 /   及其左边的字符串：dir1/dir2/dir3/my.file.txt  
${file##*/}：删掉最后一个 /    及其左边的字符串：my.file.txt  
${file#*.}：删掉第一个 .    及其左边的字符串：file.txt  
${file##*.}：删掉最后一个 .  及其左边的字符串：txt  
${file%/*}：删掉最后一个  / 及其右边的字符串：/dir1/dir2/dir3  
${file%%/*}：删掉第一个 /  及其右边的字符串：(空值)  
${file%.*}：删掉最后一个  .    及其右边的字符串：/dir1/dir2/dir3/my.file
${file%%.*}：删掉第一个  . 及其右边的字符串：/dir1/dir2/dir3/my  
记忆的方法为：  
# 是 去掉左边（键盘上#在 $ 的左边）  
% 是去掉右边（键盘上% 在$ 的右边）  
单一符号是最小匹配；两个符号是最大匹配  
```
## 6. 文件测试运算符
检测unix 文件各种属性  
操作符 | 说明 | 举例
--- | --- | ---
-b file | 检测是否为块设备文件 | [-b $file] 返回 true
-c file | 检测是否为字符设备文件 | [-c $file] 返回 true
-f file | 是否为普通文件(非目录，非设备) | [-f $file] true
-g file | 检测文件是否设置了SGID 位 | [-g file] true
-p file | 检测是否是具名管道 | [-p file] true
-r file | 检测文件是否可读 | [-r $file] true
-w file | 检测文件是否可读 | [-w $file] true
-x file | 检测文件是否可读 | [-x $file] true
-s file | 检测文件是否不为空(大小大于0) | [-s $file] true
-e file | 检测文件是否存在(包括目录) | [-e $file] true

## 7. 数组
- 定义数组
  array_name=(value0 value1 value2)；  
  或：  
  array_name[0]=value0,array_name[1]=value1,array_name[2]=value2,
- 读取数组
  ${array_name[index]};  
  读取全部元素用'@'或'#': ${array_name[*]},${array_name[@]};  
- 获取数组元素个数
  length=\${#array_name[@]},或:  
  length=\${#array_name[*]};
- 获取数组下标  
  length=\${!array_name[@]},或:  
  length=${!array_name[*]};
- 获取单个元素长度  
  lengthn=${#array_name[n]}; 
## 8. printf 函数
- 1.printf 不用加括号
- 2.format-string 可以没有括号
- 3.参数多于格式控制符(%)时，format-string可以重用，可以将所有参数都转换
- arguments 使用空格分隔，不用逗号
- eg：  
1. 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用  
```
$ printf %s abc def  
abcdef  
$ printf "%s\n" abc def  
abc  
def  
$ printf "%s %s %s\n" a b c d e f g h i j  
a b c  
d e f  
g h i  
j  
```
2. 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替  
$ printf "%s and %d \n"   
and 0  
3. 
## 9. shell 中条件语句
- if 语句
包括：  
1. if [ 表达式 ] then  语句  fi  
2. if [ 表达式 ] then 语句 else 语句 fi  
3. if [ 表达式] then 语句  elif[ 表达式 ] then 语句 elif[ 表达式 ] then 语句   …… fi

- case …… esac语句    
ase ... esac 与其他语言中的 switch ... case 语句类似，是一种多分枝选择结构。case语句格式如下：
```
case 值 in  
模式1)  
    command1  
    command2  
    command3  
    ;;  
模式2）  
    command1  
    command2  
    command3  
    ;;  
*)  
    command1  
    command2  
    command3  
    ;;  
esac   
```
其中，   
1. 取值后面必须为关键字 in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。;; 与其他语言中的 break 类似，意思是跳到整个 case 语句的最后。  
2. 如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

## 10. shell 中循环控制语句
- 1.for 循环
```
for 变量 in 列表  
do  
    command1  
    command2  
    ...  
    commandN  
done   
```
注意：列表是一组值（数字、字符串等）组成的序列，每个值通过空格分隔。每循环一次，就将列表中的下一个值赋给变量  
- 2.until 循环  
until 循环执行一系列命令直至条件为 true 时停止。until 循环与 while 循环在处理方式上刚好相反。    常用格式为：
```
until command  
do  
   Statement(s) to be executed until command is true  
done  
```
command 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环。  
类似地， 在循环中使用 break 与continue 跳出循环。    另外，break 命令后面还可以跟一个整数，表示跳出第几层循环。

## 11. shell 函数
- 定义
Shell函数必须先定义后使用，定义如下，  
```
function_name () {  
    list of commands  
    [ return value ]  
}  
```
也可以加上function关键字：  
```
function function_name () {  
    list of commands  
    [ return value ]  
}  
```
注意:1.调用函数只需要给出函数名，不需要加括号。    
2.函数返回值，可以显式增加return语句；如果不加，会将最后一条命令运行结果作为返回值。  
3.Shell 函数返回值只能是整数，一般用来表示函数执行成功与否，0表示成功，其他值表示失败。  
4.函数的参数可以通过 $n, $#, $@, $*   
5.像删除变量一样，删除函数也可以使用 unset 命令，不过要加上 .f 选项 :    
unset .f function_name
## 12 shell 文件包含
Shell 也可以包含外部脚本，将外部脚本的内容合并到当前脚本。使用：  
. filename  
#或   
source filename    
1.两种方式的效果相同，简单起见，一般使用点号(.)，但是注意点号(.)和文件名中间有一空格。  
2.被包含脚本不需要有执行权限。 

# sh、./、souce *.sh、. 的区别
- sh、./ 会启动子shell 程序，创建新进程，父进程中的局部变量不会传到子程序
- souce *.sh . 不会启动子进程，可以访问当前shell程序中的局部变量  
# shell 常用命令
## 1.set -e
- 当命令返回非零时，立即退出脚本运行
- 作用范围只限于脚本当前的进程，不作用于其创建的子进程
