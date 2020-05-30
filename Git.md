[Git 简明教程](https://www.kancloud.cn/wuding/git-tutorial)
==============

https://git-scm.com/book/zh/v2 <=> https://github.com/progit/progit2-zh

参考.md
-------

- [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [Git 学习指南](https://www.xiaoyulive.top/books/git/)
- [git-cheat-sheet](https://github.com/arslanbilal/git-cheat-sheet/blob/master/other-sheets/git-cheat-sheet-zh.md)
- [Git速查表汇总](https://zhuanlan.zhihu.com/p/28768182)




SUMMARY.md
----------

* [安装](安装.md)
* [配置](配置.md)
* [忽略](忽略.md)
* [分支](分支.md)
* [参考](参考.md)



安装.md
-------

Git 官方下载

https://git-scm.com/downloads

Windows XP 可以用 v2.10.0

https://github.com/git-for-windows/git/releases/tag/v2.10.0.windows.1

CentOS 安装
```bash
yum install git
```



配置.md
-------

~~~
git --config user.email ""
git --config user.name ""
~~~



忽略.md
-------

**不生效解决方法**
先 Unstage From Commit

```
git rm -r --cached .
git add .
git commit -m "update .gitignore"
```



分支.md
-------

**查看分支**

```
git branch [-r|-a]
```

- -r 远程

- -a 所有

**创建分支**

```
git branch <branch_name>
```

**切换分支**

```
git checkout [-b] <branch_name>
```

- -b 创建+切换

**推送分支**

```
git push origin <branch_name>
```

**删除本地分支**

```
git branch -d <branch_name>
```

**删除远程分支**

```
git push origin :<branch_name>
```



## 远程.md

**远程仓库的重命名**

```sh
git remote rename old-origin new-origin
```

**远程仓库的删除**

```sh
git remote rm old-origin
```

#### 参考：

%ProgramFiles%/Git/mingw64/share/doc/git-doc/git-remote.html




GitHub.md
---------

#### 参考：

[GitHub 的替代产品有哪些？](https://www.zhihu.com/question/19573222)



### 初始化

命令行创建新仓库

```sh
echo "# mysql-tutorial" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wuding/mysql-tutorial.git
git push -u origin master
```
推送到已有的仓库

```sh
git remote add origin https://github.com/wuding/mysql-tutorial.git
git push -u origin master
```



### 同步提交

2 种方法，可以同时推送，但要单独拉取

.get/config
```ini
[remote "origin"]
	url = https://gitee.com/excai/note.git
	url = https://github.com/wuding/note.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[remote "github"]
	url = https://github.com/wuding/note.git
	fetch = +refs/heads/*:refs/remotes/github/*
```

- **remote 添加 url**
```bash
git remote set-url --add origin https://github.com/wuding/note.git
```

- **添加 remote**
```bash
git remote add github https://github.com/wuding/note.git
git config alias.pushall "!git push origin && git push github"
```

另外还有一种方法用 Webhooks 实现双向同步

#### 参考：

[Git学习总结（16）——开源世界GitHub和开源中国GitOSChina同步提交](https://blog.csdn.net/u012562943/article/details/74638627)

[git push同时推送到两个远程仓库](https://www.jianshu.com/p/edc85a20ada9)

[配置 git 使项目同时推送至多个远程仓库](https://www.jianshu.com/p/4e7932bcf2eb)



GitBook.md
----------

### 在线版本

绑定仓库：

设置 -> 集成 -> GitHub



### 本地版本

安装

```sh
npm install -g gitbook-cli
gitbook -V
```

初始化文件目录

```sh
gitbook init
# 编辑 SUMMARY.md 再次执行
```

目录 SUMMARY.md

```markdown
* [Chapter1](chapter1/README.md)
  * [Section1.1](chapter1/section1.1.md)
  * [Section1.2](chapter1/section1.2.md)
* [Chapter2](chapter2/README.md)
```

预览 http://localhost:4000

```sh
gitbook serve
```

构建静态页 _book

```sh
gitbook build
```

导出 PDF

需要安装 [Calibre](http://calibre-ebook.com/)

```sh
gitbook pdf ./ ./mybook.pdf
```

参考：

[gitbook 转换 pdf](https://blog.csdn.net/sinat_25295611/article/details/78973827)

命令

```sh
gitbook init //初始化目录文件
gitbook help //列出gitbook所有的命令
gitbook --help //输出gitbook-cli的帮助信息
gitbook build //生成静态网页
gitbook serve //生成静态网页并运行服务器
gitbook build --gitbook=2.0.1 //生成时指定gitbook的版本, 本地没有会先下载
gitbook ls //列出本地所有的gitbook版本
gitbook ls-remote //列出远程可用的gitbook版本
gitbook fetch 标签/版本号 //安装对应的gitbook版本
gitbook update //更新到gitbook的最新版本
gitbook uninstall 2.0.1 //卸载对应的gitbook版本
gitbook build --log=debug //指定log的级别
gitbook build --debug //输出错误信息
```

多语言 LANGS.md

```markdown
* [English](en/)
* [French](fr/)
* [Español](es/)
```

配置 book.json

```json
{

    //样式风格配置格式
    "styles": {
        "website": "styles/website.css",
        "ebook": "styles/ebook.css",
        "pdf": "styles/pdf.css",
        "mobi": "styles/mobi.css",
        "epub": "styles/epub.css"
     },

    //插件安装配置格式

    "plugins": ["myplugin"],
    "pluginsConfig": {
        "myPlugin": {
            "message": "Hello World"
        }
     }
}
```

### GitBook Editor

```
C:\Users\Administrator\GitBook\Library\Import\testbook
```



### 插件

https://github.com/GitbookIO/plugin-hints


### 参考：

[GitBook 从懵逼到入门](https://blog.csdn.net/lu_embedded/article/details/81100704)

[GitBook文档（中文版）](https://github.com/chrisniael/gitbook-documentation)

[Gitbook 教程](https://yidaofei.com/post/20180920-software-skills-gitbook-guide/)

[GitBook使用教程](https://blog.csdn.net/axi295309066/article/details/61420694)

[GitBook 简明教程](https://github.com/chengweiv5/gitbook) <=> [GitBook 簡明教程](https://github.com/shihyu/gitbook)

[gitbook-use](https://github.com/zhangjikai/gitbook-use/)

[GitBook Clarity](https://github.com/zhilidali/gitbook)

[GitBook使用入门](https://janicezhw.github.io/gitbook)

[Gitbook模板](https://github.com/crifan/gitbook_template)



服务器.md
--------

[CentOS 部署 Git 服务器](https://shenyu.me/2017/01/03/git-server.html)



工作流程.md
----------

### Git flow

https://github.com/nvie/gitflow



#### 参考：

[Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

[git-flow 备忘清单](https://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)



身份认证.md
----------

### HTTPS 自动存储用户名和密码

用户文件夹 %USERPROFILE% 新建文件  .git-credentials

```
https://{username}:{password}@github.com
```

Bash 命令

```bash
git config --global credential.helper store
```

会在 .gitconfig 添加

```ini
[credential]
	helper = store
```



### SSH 方式

生成 key

```sh
ssh-keygen -t rsa -C your@email.com
# 用户文件夹 .ssh 目录下会生成 id_rsa.pub
```

Git GUI 菜单 help > Show SSH Key



##### 参考：
[Git Push 避免用户名和密码方法](https://www.cnblogs.com/ballwql/p/3462104.html)

[使用git提交到github,每次都要输入用户名和密码的解决方法](https://www.cnblogs.com/sky6862/p/7992736.html)

https://github.com/fork-copy/Notes/blob/master/Tools/Git/Git_push_password_less.md



常见问题.md
----------

[The remote end hung up unexpectedly](https://www.cnblogs.com/wangkun1993/p/8514015.html)



### Unlock index

行尾结束符 Windows 回车换行，Linux Mac 换行

提交时 CRLF 转 LF 签出时 LF 转 CRLF

```bash
git config --get core.autocrlf
git config --global core.autocrlf true
# true 自动转换 input 提交时转换 false 不转换
```

##### 参考：
https://git-scm.com/book/zh/v1/自定义-Git-配置-Git#格式化与空白



### wrong # args: should be "ui_status msg"

```bash
git config --global core.autocrlf false
```



### SSH key

```sh
git config --global user.name "yourname"
git config --global user.email "youremail"

ssh-keygen -t rsa -C "youremail"
```

- [Permission denied (publickey). fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.](https://www.jianshu.com/p/3b56f4e6ac77)



### 访问权限

删除用户目录里的 .ssh 文件夹中的文件 known_hosts，执行上一条问题解决 SSH key 命令，添加 SSH key 到 GitHub

```sh
ssh -T git@github.com
```

- [git遇到的问题之“Please make sure you have the correct access rights and the repository exists.”](https://blog.csdn.net/jingtingfengguo/article/details/51892864)



####  HTTPS 无法访问

- [github clone遇到的问题](https://my.oschina.net/u/149047/blog/673358)
- [git使用教程](http://lanvane.com/cat/72)

- [在Git Bash中，执行github.com上的git pull时，提示Unknown SSL protocol error in connection to github.com:443，弃用连接方式HTTPS，让其支持SSH的解决流程](http://www.shuijingwanwq.com/2016/11/01/1394/)



### fatal: Authentication failed for

HTTPS 方式提示验证错误，可以如下解决：

```sh
git config --system --unset credential.helper
```

重新打开 Git GUI 就会再次提示输入用户名和密码

- [fatal: Authentication failed for又不弹出用户名和密码 解决办法](https://www.jianshu.com/p/8a7f257e07b8)



#### 账号问题

控制面板\所有控制面板项\凭据管理器

Windows 凭据\普通凭据

删除 `git:https://github.com`

- [git在push提示错误403](https://blog.csdn.net/juan083/article/details/78110586)



#### 合并无关历史

```sh
git pull origin master --allow-unrelated-histories
```

- [解决Git refusing to merge unrelated histories](https://www.jianshu.com/p/536080638cc9)




帮助.md
------

```bash
$ git --help
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```


