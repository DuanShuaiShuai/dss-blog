---
title: git
---
### 基本操作
```bash 
git remote -v # 查看远程仓库的数量- 简单信息
git remote show origin #查看摸个远程仓库的具体信息
git remote add <name> xxxx.com  # 关联远程分支  name 是简称 和origin一样 
git remote set-url --add origin <url2> # 向 origin中再添加一个url   git push 操作就会同时push至多个远程
git push # 如果就一个分支  git push 与git push origin master 没有区别
git push origin master  # 如果多个远程关联 那就是不一样
git remote set-url origin xxxxx #当代码库远程迁移后，修改本地代码关联的远程地址
```

- 工作区
    - git clone
    - git init
    - git add 
    - git fetch 
    - git pull/push
    - git checkout
    - git log/reflog  git reflog 会显示被删除的提交 也就是所有的提交/分支切换
```bash
    commit 234823475
    two commit 
    commit 723784572
    first commit
```
    - git config/status 
        - git config --global --list # 查看配置
        - git config --global user.name "xxx"
        - git config --global user.email "xxx.com"

- 暂存区 stage (git add后)
    - git reset 
        - git reset --hard 723784572   # 相当与删除two commit提交
    - git stash & apply
    - git rm 
    - git status 
- commit 
    - git branch 
    - git merge 
    - git diff 
    - git remote 
    - git rebase 
    - git reset 