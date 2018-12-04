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



参考.md
-------

- [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)

- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)

- SSH key

