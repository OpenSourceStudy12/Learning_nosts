# 介绍  
Repo 是用 Python 写的调用 git 的脚本，主要用来下载管理 Android 项目的软件仓库。 

# 用法  
## 1.Repo下载
1) wget http://android.git.kernel.org/repo > ~/bin/repo  
2) curl http://android.git.kernel.org/repo > ~/bin/repo
  
  修改 repo 可执行权限 chmod u+x ~/bin/repo  
 
## 2.初始化
repo init -u git://android.git.kernel.org/platform/manifest.git    
repo init 之后，在相应目录下会出现一个隐藏  .repom 目录，.repo/manifest/default.xml 文件记录了 Android  版本所需的库文件  

参数:  
 -u 指定 URL  
 -m 指定获取 repository 中 某一个特定 manifest 文件，默认为default.xml  
 -b 指定某个 manifest 分支 默认下载 master 分支
 -mirror 在 repo sync 时会按照源的版本库组织，否则会按照 manifest.xml 的方式重新组织并检出到本地   
 
 此时 .repo/ 下有几个文件：  
 manifests/     
 manifest.git/    
 manifest.xml    
 repo/
 
 manifests.git/ 此为repo配置信息的git库，不同版本包含不同配置信息
 
 manifests/ 此为repo配置信息的工作目录（将配置信息的工作目录和相应的实际git目录分离管理，并且配置信息中的.git目录实际只是指向实际git库的软连接），其中可能包含一个或多个xml文件描述的配置。每个xml文件是独立的一套配置，配置内容包括当前repo工作目录包含哪些git项目、所有git项目所处的默认公共分支、以及远端地址等 。
 
 manifest.xml 该文件就是其软连接,指向 manifests/*.xml
 
 repo/ 此为repo脚本集的git库，用于repo管理所需的各种脚本，repo的所有子命令就是其中的对应脚本实现。该脚本也通过 git 管理。
 
 
 ## 3.下载
 repo sync   
 更新本地工作文件。若第一次 运行 repo sync ，则会把 repository 所有文件复制到本地，若不是第一次运行 repo sync 则会提示上传修改内容
   
 参数 ：[project-list]
 同步指定 project 列表，若不指定参数则同步整个项目
 
 此时 .repo/ 下会多一个 projects 目录，此为repo所管理的所有git项目集，包含repo当前配置所指定的所有git项目对应的git目录。不同的清单文件（即manifest.xml）内容，指定不同的git项目集组合，表征不同的项目版本或者项目，而如上所述，manifest.xml文件的内容又由其指向的manifests中的、具体的分支下的、xml文件来决定。
 
 .repo/../*  此为repo的工作区。在repo目录（即.repo）之外，根据repo配置（即.repo/manifest.xml文件），从.repo/projects/*中提取出指定分支的各个git项目（即.repo/projects中git项目的子集）的工作目录，形成repo工作目录，可供开发使用。其中每个git工作目录中的.git只是指向.repo/projects/*的软连接，在repo工作目录中的某个git工作目录更新相应的git库，其实最终会更新到.repo/projects中对应的git库。刚刚repo sync之后，当前工作目录不处于任何分支，其中的修改只能本地保存无法提交至远端，若想提交工作，需要先创建一个分支保存工作内容。
 
 ## 4. 上传
 repo update
 上传修改的内容
 
 ## 5.repo diff [project-lsit]
 显示提交的内容与本地内容的差异
 
 ## 6.repo download target version
 下载特定的修改版本到本地
 
 ## 7.repo start branchname 
 创建新的分支，“.”表示当前分支  
 参数 ：  
 -all 为所有项目从 manifests.xml 中指定的分支创建新分支  
 project-list  为指定项目从 manifests.xml 中指定的分支创建新分支
 
 ## 8. repo checkout branchname [project-list]
 将指定项目切换到新分支
 
 ## 9. repo branch [project-list]
 查看指定项目分支

 ## 10.repo prune [project-list]
 删除已经 merge 的 project
 
 ## 11.repo abandon branchname [project-list]
 删除指定项目的分支
 
 ## 12.repo foreach [project-list] -C command
 对每个project 运行 command 命令
 
 ## 13.repo forall -C command
 遍历所有 project ,每个目录执行 command 命令，该命令不仅仅是 git 命令，也包括任何被 系统支持的命令(ls cp pwd...)
 
 参数： 
 -c shell 命令  
 -p 在 shell 指令输出之前列出项目名称  
 -v 列出执行 shell 命令时的错误信息
 ### 13.1 合并多个分支
 repo checkout master  
 repo forall -C git merge topic  
 将所有项目切换到 master 分支后，将所有项目的 topic 分支合并到 master 分支
 ### 13.2 tag
 repo forall -C git tag tagname  
 
 
 ## 14.repo status
 显示 project 中每个仓库状态，并打印仓库名称
 
 