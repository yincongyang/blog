# git 常用命令

## 系统配置: git config
### 查看git配置: git config --list
同一个配置项有可能出现多次，是因为git从不同的文件中读取到了相同的关键字，这种情况，git会使用最后的那个值。如：
```
core.symlinks=false
core.autocrlf=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
pack.packsizelimit=2g
help.format=html
http.sslcainfo=/bin/curl-ca-bundle.crt
sendemail.smtpserver=/bin/msmtp.exe
diff.astextplain.textconv=astextplain
rebase.autosquash=true
user.email=yiibai.com@gmail.com
user.name=minsu
user.author=author-maxsu
core.autocrlf=false

其中core.autocrlf出现了两次，最后一个值有效
```

### 常用git配置: git config [location] [opt] [key] [value]
设置文件默认换行符
```
1： 提交时转换为LF，检出时转换为CRLF（适用于全是windows平台编辑，且大家的编辑器全部设置为CRLF）
git config --global core.autocrlf true   

2：提交时转换为LF，检出时不转换（适合多平台，Linux默认该设置，windows下建议IDE设置成LF，可避免冲突）
git config --global core.autocrlf input   

3：提交检出均不转换（不推荐使用，会造成同一项目存在多种换行符的场景）
git config --global core.autocrlf false
```

设置文件换行符提醒策略
```
1: 拒绝提交包含混合换行符的文件（推荐）
git config --global core.safecrlf true   

2: 允许提交包含混合换行符的文件
git config --global core.safecrlf false   

3: 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

## 克隆代码：git clone
```
git clone {远程地址}
```

## 设置代理
1： 设置全局代理
```
git config --global https.proxy http://10.166.17.101:8080
git config --global https.proxy http://10.166.17.101:1080
```
2: 取消全局代理
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
3: 设置本地代理
```
git config --local https.proxy http://127.0.0.1:1080
git config --local https.proxy https://127.0.0.1:1080
```
4: 取消本地代理
```
git config --local --unset http.proxy
git config --local --unset https.proxy
```
5：仅为github设置代理
```
git config --global http.https://github.com.proxy http://10.166.17.101:8080
```

## 分支管理：git branch|checkout

### 查看分支
1 仅查看当前本地分支：
```
$ git branch
* dev
  dev01
  master
```
2 仅查看远程分支
```
$ git branch -r
  origin/HEAD -> origin/master
  origin/master
```
3 查看所有分支
```
$ git branch -a
* dev
  dev01
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```
4 查看本地分支所关联的远程分支【推荐】
```
$ git branch -vv
  dev        1b7b826 test crlf
* dev-0002-m 0b31ca9 [origin/dev-0002: ahead 1] 
  dev-0003   1b7b826 [origin/dev-0003] test crlf
  dev01      1b7b826 test crlf
  master     1b7b826 [origin/master] test crlf

```

### 创建分支


> 本地分支和远程分支的名称需要保持一致
1 仅创建本地分支
```
$ git branch dev-0001

$ git branch
* dev
  dev-0001
  dev01
  master
```

2 创建并切换至新的分支
```
$ git checkout -b dev-0002
Switched to a new branch 'dev-0002'

$ git branch
  dev
  dev-0001
* dev-0002
  dev01
  master
```

3 从远程拉取指定分支到本地（推荐）
```
$ git pull

$ git checkout -b dev-0003 origin/dev-0003
Switched to a new branch 'dev-0003'
Branch 'dev-0003' set up to track remote branch 'dev-0003' from 'origin'.

$ git branch -a
  dev
  dev-0001
  dev-0002
* dev-0003
  dev01
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev-0003
  remotes/origin/master
```

4 将本地分支上传至远程
```
第一步：切换到需要上传至远程的分支
$ git checkout dev-0001
Switched to branch 'dev-0001'

第二步：push到远程
$ git push --set-upstream origin dev-0001

第三步：push代码（自动push到对应分支）
$ git push
```
>注意，也可以使用git push origin dev-0001:dev-0001 命令创建远程分支，但是不推荐使用，因为该命令执行后，还是需要--set-upstream来指定push默认远程分支。

### 删除分支
1 删除本地分支
```
$ git branch -d dev-0001
```

2 删除远程分支
```
$ git push origin --delete dev-0001
```

### 修改分支名称
1 修改本地分支名称
```
$ git branch -m dev-0002 dev-0002-m
```
>注意，修改本地分支名称后，对应的远程分支名称并没有改变，建议将原来的远程分支删除，重新push更名后的分支。


### 切换分支
1 切换本地分支
```
$ git checkout dev-0001
```

### 合并代码
1 拉取对应远程分支代码
```
没有冲突
$ git pull

若有冲突 
$ git fetch
$ git merge

$ git pull --rebase(推荐)
```

### 合并分支
1 将其它分支合并至当前分支
```
使用git merge直接合并，会多出一个提交信息
$ git merge dev-0003

使用git rebase合并
$ git rebase dev-0003

```
> git rebase合并分支时会改变提交历史，此时若之前的代码已经push至远程分支，需要用git rebase -f强制提交去更新。强制更新操作非常危险，因此feature分支最好是只有一个人去开发，

### 修改提交历史
1 修改本地的提交历史
```
$ git rebase -i master
```





