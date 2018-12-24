[Git 简明教程](https://www.kancloud.cn/wuding/git-tutorial)
==============

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



# GitHub

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



# GitBook

## 在线版本

绑定仓库：

设置 -> 集成 -> GitHub



## 本地版本

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

#### GitBook Editor

```
C:\Users\Administrator\GitBook\Library\Import\testbook
```



### 参考：

[GitBook 从懵逼到入门](https://blog.csdn.net/lu_embedded/article/details/81100704)

[GitBook文档（中文版）](https://github.com/chrisniael/gitbook-documentation)

[Gitbook 教程](https://yidaofei.com/post/20180920-software-skills-gitbook-guide/)

[GitBook使用教程](https://blog.csdn.net/axi295309066/article/details/61420694)

[GitBook 简明教程](https://github.com/chengweiv5/gitbook)



# 服务器

[CentOS 部署 Git 服务器](https://shenyu.me/2017/01/03/git-server.html)




参考.md
-------

- [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [Git 学习指南](https://www.xiaoyulive.top/books/git/)
- SSH key

