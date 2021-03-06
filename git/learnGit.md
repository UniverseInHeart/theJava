## 学习如何使用Git
![](./pic/branching-illustration@2x.png)



> git init

> git config --global --list

> git config --local --list

> git status

> git add . 

> git commit -m " "


> git log   

> git log --oneline// 一行  

 
> git log -n4 // 最近的log 


> git log --all //全部分支  


> git log 分支名  //指定分支 


> git log --all --graph //图形化  



> git reset --hard  // 回滚  

> git log 分支名  //指定分支  

 
> git log --all --graph  //图形化 
 
> git reset --hard   // 回滚 

> git push

> git mv A B // 重命名  


> git branch -av // 查看分支  
> git branch -d  分支名 // 删除分支  

> git checkout -b name 版本 // 删除分支
 
> git checkout -D name 版本 // 删除分支

> git commit --amend //最近的一次commit的message修改

> git rebase -i 要修改commit的父commit编号 -> r  // 历史commit的message修改

> git rebase -i 要修改commit的父commit编号 -> s  // 历史commit的message合并

> git diff --cached  // 比较暂存区和HEAD 

> git diff  // 比较暂存区和工作区 

> git diff -- 文件名  // 比较部分文件暂存区和工作区 `-- 后面代表文件`

> git diff 49c0216a 58d9c6a4 -- \<file\>... // 比较不同commit



> 恢复暂存区用reset  恢复工作区用checkout,`可以用.代表全部文件`

> git reset HEAD  \<file\>...//放弃暂存，所有文件从暂存区恢复到HEAD

> git checkout -- \<file\>...  //放弃工作区的修改，和暂存区保持一致

>


### .git文件
> HEAD //正在工作的分支

> config  //配置信息

> refs.heads //所有分支名

```
objects // 历史信息
三大对象：commit > tree > blob

git cat-file -t a88dd9c6de8d7b8a9d8  // 查看类型
git cat-file -p d8a7e6c9a0d9d8e8c6d  // 查看内容
```

分离头指针指的是没有基于某个branch做，进行分支切换，可能会被清理掉

HEAD指向某一个commit



## 学习如何使用GitHub

> 开源协议 MIT license

> 高级搜索 : java 面试 in:readme star>100


## GitHub三剑客