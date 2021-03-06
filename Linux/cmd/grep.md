# grep 命令
- grep的工作方式是这样的，它在一个或多个文件中搜索字符串模板。如果模板包括空格，则必须被引用，模板后的所有字符串被看作文件名。搜索的结果被送到屏幕，不影响原文件内容。   
- grep可用于shell脚本，因为grep通过返回一个状态值来说明搜索的状态，如果模板搜索成功，则返回0，如果搜索不成功，则返回1，如果搜索的文件不存在，则返回2。我们利用这些返回值就可进行一些自动化的文本处理工作。  

格式：grep [option] 模式 文件名
## option
-E: 开启扩展的正则表达式  
-i: 忽略大小写  
-v: invert, 只打印未匹配的，匹配的不打印  
-n: 显示行号  
-w: 被匹配的只能是单词，不能使单词的一部分  
-c: 显示总共匹配的行数  
-o: 只显示被模式匹配到的字符串  
--color: 将匹配到的内容以高亮显示
-A n: after， 显示匹配到的字符串所在的行及其后n行  
-B n: before, 显示匹配到的字符串所在的行及其前n行  
-C n: contex, 显示匹配到的字符串所在的行及其前后后n行

## 模式
### 1.直接输入要匹配的字符串
### 2.使用基本正则表达式  


- 匹配字符：  
    . : 匹配任何字符  
    [abc] : 匹配a,b,c中任一个字符  
    [a-zA-Z] : 匹配a-z,A-Z中任一字符  
    [^123] : 除了1,2,3 以外的任一字符  
    对于一些常用的字符集，系统做了定义：  
　　[A-Za-z] 等价于 [[:alpha:]]  
　　[0-9] 等价于 [[:digit:]]  
　　[A-Za-z0-9] 等价于 [[:alnum:]]  
　　tab,space 等空白字符 [[:space:]]  
    [A-Z] 等价于 [[:upper:]]  
　　[a-z] 等价于 [[:lower:]]  
　　标点符号 [[:punct:]]  
- 匹配次数：  
    \\{m,n\\} : 匹配其前面出现的字符至少m次，至多n次  
    \? : 匹配其前面出现的字符0或1次，等价于\\{0,1\\}  
    \* : 匹配其前面的内容任意次  
- 位置锚定  
    ^ : 锚定行首  
    $ : 锚定行尾，"^$" 匹配空白行  
    \\b 或\\< : 锚定单词词首  
    \\b 或\\> : 锚定单词词尾  
    \\B : 与 \\b 相反  
- 分组及引用   
    \\(string\\) : 将string 作为一个整体方便后面使用  
    \\n : 引用第n个左括号及其对应的右括号所匹配的内容  
### 3. 使用扩展的正则表达式 

- 匹配字符： 与基本正则表达式一样 
- 匹配次数  
    * : 和基本正则表达式一样   
    ? : 基本正则表达式是\\?,这里没有\\  
    {m,n} : 没有\\  
    \+ : 匹配其前面的字符至少一次，等价{1,n}  
- 位置锚定   
    和基本正则表达式一样 
- 分组及引用  
    (string) : 没有 \\  
    \\n : 和基本正则一样 
- 或者  
    | : 

## 表达符集
- ^ : 行的开始，eg：'^grep' 匹配所有以grep开始的行
- $ : 行的结束，eg: 'grep$' 匹配所有以grep结束的行
- . : 匹配一个非'\n'字符，eg : 'gr.p' 匹配gr后接任意字符，然后是p
- \* :匹配零个或多个先前字符， eg : ' *grep' 匹配所有零个或多个空格后跟grep的行
- [] : 匹配一个在指定范围的字符，eg : '[Gg]rep' 匹配Grep 和 grep
- [^] : 匹配一个不在指定范围的字符，eg : '[^A-FH-Z]rep' 匹配不包含A-F和H-Z的一个字母，后跟rep的行
- \\(..\\) : 标记匹配字符串，eg : '\\(love\\)' love 被标记为1
- \\< : 