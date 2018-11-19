title: Git常见问题解答FQA
date: 2017-06-27 04:00:00
tags: [tool,git]
categories: [git]

---

# Git常见问题解答FQA
1. 设置身份
```shell
git config --global user.name "wnow20"
git config --global user.email "wnow20@gmail.com"

git config --global user.name "wb-linshenglong"
git config --global user.email "wb-linshenglong@cainiao.com"
```
1. 为什么有些时候 `git commit -a -m "this is a comment"` 失效？
答：因为Git中自动追踪文件的改动，要我们手动添加add。`git commit -a -m` 命名会把之前追踪过的文件自动添加追踪，当在要提交的文件中含有之前为追踪过的文件时，git会提示警告，不给成功提交。

1. 提交时怎么才能添加多行信息？

```shell
git commit -a

```

1. 怎么回退reset单个文件

```shell
git checkout xxx.ext
git reset xxx.ext

```

1. clone指定分支

```shell
git clone <remote_repo> -b <branch>
```

1. 只删除仓库指定文件, 而不删除本地文件
```shell
git rm --cached file
git rm -r --cached folder
```

1. 修改本地还未push的某次提交

git config文件位于
GIT_DIR/config
 ~/.gitconfig
/etc/.gitconfig windows中位于安装目标的/etc/.gitconfig


git config --global alias.ci "commit -a -v"
git reset --hard HEAD 回退到最新提交
git reset --hard HEAD^ 向上退一个提交（最新的提交被丢掉）

git reset --soft HEAD^ 这工作空间丢掉最新版本，但是不删除内容


git stash 暂时隐藏当前修改
git stash apply 取出之前隐藏的修改

新建分支并切换到该分支
git checkout -b 'branch_name'

删除分支(-D强制删除, -d安全删除)
git branch -D branch_name

git checkout old_branch -b new_branch

显示缓冲区文件

git ls-files


git add . 可以代替 `git rm` 和 `git mv`

删除当前路径以及缓冲区中的bar.txt，需要commit

git rm bar.txt


删除缓冲区中bar.txt文件，需要commit

git rm --cached bar.txt


重命名

git mv 1.txt 2.txt


.gitignore


克隆本地项目

git clone {root}/git_project/


分组提交
git add -p
git add files（指定文件）
git add -i


git diff --cached 当前问

本地文件与缓冲区的区别


git fetch

git rebase origin/master

// 解决冲突

修改冲突文件

git add .

// 解决冲突后

git rebase --continue



添加远程仓库

git remote add source https://github.com/username/pro-name.git

git pull source master


删除远端分支

git push origin :branch-name
git push --delete origin old_branch

## 疑问
 + 拆分提交，单文件文件拆分提交
 + shell函数命令 怎么入门使用
 + revert 一个版本其中包含三个文件、B、C，A有一个bug，BC还是需要的。
 + 现在很多同事在同一条分支上开发，也就是master，这样大家一直用 merge 命令，会不会很不合理
 + 回退一个指定的文件夹