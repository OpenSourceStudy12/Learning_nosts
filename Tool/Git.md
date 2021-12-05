# git常用命令

## 1. git remote 命令

1. git remote 不带参数，列出已经存在的远程分支
2. git remote -v | --verbose 列出详细信息，在每一个名字后面列出其远程url
   此时， -v 选项(译注:此为 –verbose 的简写,取首字母),显示对应的克隆地址:
   eg:
   $ git remote -v
   origin  http://git.oschina.net/yiibai/sample.git (fetch)
   origin  http://git.oschina.net/yiibai/sample.git (push)
3.

## 2. git format-patch /git am/apply

1. patch 中存储的是对代码的修改(diff)，打patch就是记录代码修改并将其保存到patc文件中。
   对于单个或多个文件，diff和patch比较方便，但对于git project 的修改就不方便，且无法保存commit信息。
   所以采用git format-patch 和 am 命令进行生成patch和打patch。
   eg:[](https://)

### 2.1 git format-patch:

git format-patch HEAD^  #生成最近1次commit的patch
git format-patch HEAD^^  #生成最近2次commit的patch
'''
'''

##### 1. git format-patch <r1>..<r2> #生成另个commit间的patch（不包含r1 commit）

##### 2. git format-patch -n <r1> #生成某个commit之前的n个patch（含该commit）

##### 3. git format-patch <r1> #生成某个commit以来的patch（不包含该commit）

##### 4. git format-patch --root <r1> #生成从根到r1提交的所有patch（不包含root commit）

### 2.2 git am/apply

##### 1. git apply --stat *.patch   #查看patch

##### 2. git apply --check *.patch  #查看patch是否能够打上，没有任何输出，则无冲突，可以打上。

git apply 与 git am 的区别是，git apply 并不会将commit message 打上去,打完patch之后，需要git add,git commit。而git am 会直接将commit message 所有信息打上去，而且不用重新git add,git commit。author 是patch author,而不是打patch的author

##### 3. git am *patch #打上patch

##### 4. git am --signoff *.path #添加 -s 或 --signoff,可以将自己的名字添加为signed off by 信息，添加打patch的author

##### 5. git am ~/patch-set/*.patch #将路径~/patch-set/目录下所有patch 按先后顺序打上

##### 6. git am --abort #当am失败时，结束am 过程

##### 7. git am --resolved #当am失败，解决冲突后，继续am

### 2.3 打patch 产生冲突后

1. 根据git am 失败信息，找到conflict的patch file,用命令git apply -reject <patch-name> 强行打上该patch,发生冲突的部分会保存为.rej文件，未发生冲突的文件会打上patch
2. 根据.rej文件，编辑该patch文件解决冲突
3. 废弃上一条am命令已经打的patch:git am --abort
4. 重新打上patch

## 3. git branch

### 3.1 git branch --set-upstream-to=远程仓库/分支 本地分支

git branch --set-upstream-to=origin/远程分支 本地分支
本地分支不写默认为当前分支
