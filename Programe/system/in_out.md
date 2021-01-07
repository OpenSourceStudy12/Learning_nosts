# 1. 格式化输入
```
/* 从标准输入stdin获取内容 */
int scanf(const char* format, ...);
/* 从内存获取内容 */
int sscanf(const char* str, const char* format, ...);
/* 从文件获取内容 */
int fscanf(FILE* stream, const char* format, ...);

scanf % 指定格式数据不对应时，函数返回，终止读取后续数据。
---------------------------------------------
int vscanf(const char* format, va_list arg);
int vsscanf(const char* str, const char* format, va_list arg);
int vfscanf(FILE* stream, const char* format, va_list arg);
```
清空输入缓存：  
fflush(stdin);  // windows 使用  
setbuf(stdin,NULL);  // linux 使用

# 2. 格式化输出
```
/* 格式化输出到标准输出stdout */
int printf(const char* format, ...);
/* 格式化输出到内存 */
int sprintf(char* str, const char* format, ...);
int snprintf(char* str, size_t n, const char* format, ...);
n:str 支持最大字符数，最多填充n-1个字符，最后一个'\0' 字符
return：返回实际填入的字符数，不算'\0';

/* 格式化输出到文件 */
int fprintf(FILE* stream, const char* format, ...);

--------------------------------------------
int vprintf(const char* format, va_list arg);
int vsprintf(char* str, const char* format, va_list arg);
int vsnprintf(char* str, size_t n, const char* format, va_list arg);
int vfprint(FILE* stream, const char* format, va_list arg);
```
|格式化符号|output|eg|
|---|---|---|
|d/i| 有符号十进制数|321|
|u|无符号十进制数|2131|
|o|无符号八进制数|340|
|x|无符号十六进制数|1fa|
|X|无符号十六进制数（大写）|7FA|
|f|十进制单精度浮点数|392.65|
|F|十进制单精度浮点数|392.65|
|e|科学计数法|3.9265e+2|
|E|科学计数法|3.9265E+2|
|g|Use the shortest representation: %e or %f|392.65|
|G|Use the shortest representation: %E or %F|392.65|
|a|十六进制单精度浮点|-0xc.90fep-2|
|A|十六进制单精度浮点|-0XC.90FEP-2|
|c|字符|a|
|s|字符串|example|
|P|地址|b8000000|
The format specifier can also contain sub-specifiers: flags, width, .precision and modifiers (in that order), which are optional and follow these specifications:  
|flags|description|
|---|---|
|-|左对齐，默认右对齐|
|+||
|#|o,x,X输出时，数值前加0,0x,0X|
|0|使用0代替空格，当输出字符小于指定width时|

|width|decsription|
|---|---|
|number|输出最小字符，若输出字符小于number,将用空格填充值左边|
|*||

|.precision|description|
|---|---|
|.number|数值精度|
|.*||
