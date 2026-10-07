# git基本命令



## 创建git仓库

```python
git init 
```



## 创建文件

```
touch file01.txt  
```



## 工作区->本地仓库

```
git add file01.txt   #提交文件file01
git add .            #提交全部工作区
```



## 本地仓库->远程仓库

```
git commit -m "add file01"	   #提交+注释
```



## 查看日志

```
git log
	-all 显示所有分支
	-pretty=oneline 将提交信息显示为一行
	--graph 以图的形式显示
	-
```



## 查看 Git 仓库状态

```
git status
```



## 版本回退

```
git reset --hard 提交id名
```



## 查看删除的记录

```
git reflog
```



# 分支



##  查看分支

```
git branch
```



## 创建分支

```
git branch 分支名
```



## 分支直观展示

```
git-log
```



## 切换分支

```
git checkout  分支名

#切换到一个不存在的分支
git checkout -b 分支名
```



## 合并分支

```
git merge 分支名称
```



## 删除分支

```
git branch -d 分支名
```



```
master分支
develop分支--开发分支
feature分支--develop下的分支用于并行开发
hotfix分支--用于线上bug修复
```



# 密钥

```
#s生成公钥
ssh-keygen -t rsa

#获取公钥
cat-/.ssh/id_rsa.pub

#检验匹配是否成功
ssh -T git@gitee.com

```



## 关联远程仓库

```
git remote add origin 远程仓库地址
git remote add origin https://gitee.com/ymhssg/git_test.git
```

```
#查看远程仓库是否添加
git remote 

=====结果===
origin
```

## 推送

```
git push  -xxx  origin master

-f 强制覆盖
--set-upstream  退送远端并建立远端分支的关联关系
```



## 克隆(从云端拉入本地)

```
#克隆到本地
git clone git@gitee.com:ymhssg/git_test.git 

#克隆到本地,并指定名字
git clone git@gitee.com:ymhssg/git_test.git 名字
```



## 从远程仓库抓取和拉取

```
#将仓库更新抓取本地,但不会进行合并
git fetch

#将仓库更新抓取本地,并自动合并
git pull
```



