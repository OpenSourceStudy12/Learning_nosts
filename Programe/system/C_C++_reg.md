标准的C和C++都不支持正则表达式，但有一些函数库可以辅助C/C++程序员完成这一功能，其中最著名的当数Philip Hazel的Perl-Compatible Regular Expression库，许多Linux发行版本都带有这个函数库

# 1. regex
```
c 中使用正则表达式分三步：
一. 编译正则表达式
int regcomp(regex_t *compiled, const char *pattern, int cflags)
1. regex_t:结构体类型，存放编译后的正则表达式
2. pattern:指向正则表达式
3. cflags:
REG_EXTENDED 以更加强大的扩展正则表达式进行匹配
REG_ICASE 忽略大小写
REG_NOSUB 不用存储匹配后的结果
REG_NEWLINE 识别换行符，$匹配行尾，^匹配行首

二. 匹配正则表达式
int reggexec(regex_t *compiled, const char *string, size_t nmatch, regmatch_t matchptr[], int eflags);
1. 若 cflags 没指定REG_NEWLINE, 则默认忽略换行符
2. nmatch 存放regmatch_t 结构体数组长度
3. regmatch_t,结构体类型
typedef struct {
    regoff rm_so;
    regoff rm_eo;
}regmatch_t;
rm_so 存放匹配文本串在目标串的开始位置，rm_eo 存放结束位置
4. eflags:
REG_NOTBOL
REG_NOTEOL

三. 重新编译正则时，清空 compiled 内容
void regfree(regex_t *compiled);

四. size_t regerror(int errcode, regex_t *compiled, char *buffer, size_t length);
当执行 regcomp 或 regexec 出错时，调用该函数返回包含错误信息的字符串
1. errcode : regcomp 或 regexec 返回错误码
2. compiled : 已经用regcomp 编译好的正则表达式
3. buffer : 指向用来存放错误信息的字符串
4. length : 指向buffer 长度
```
