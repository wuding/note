[Git �����̳�](https://www.kancloud.cn/wuding/git-tutorial)
==============

SUMMARY.md
----------

* [��װ](��װ.md)
* [����](����.md)
* [����](����.md)
* [��֧](��֧.md)
* [�ο�](�ο�.md)



��װ.md
-------

Git �ٷ�����

https://git-scm.com/downloads

Windows XP ������ v2.10.0

https://github.com/git-for-windows/git/releases/tag/v2.10.0.windows.1

CentOS ��װ
```bash
yum install git
```



����.md
-------

~~~
git --config user.email ""
git --config user.name ""
~~~



����.md
-------

**����Ч�������**

```
git rm -r --cached .
git add .
git commit -m "update .gitignore"
```



��֧.md
-------

**�鿴��֧**

```
git branch [-r|-a]
```

- -r Զ��

- -a ����

**������֧**

```
git branch <branch_name>
```

**�л���֧**

```
git checkout [-b] <branch_name>
```

- -b ����+�л�

**���ͷ�֧**

```
git push origin <branch_name>
```

**ɾ�����ط�֧**

```
git branch -d <branch_name>
```

**ɾ��Զ�̷�֧**

```
git push origin :<branch_name>
```



�ο�.md
-------

- [Git�̳�](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)

- [git - ����ָ��](http://rogerdudler.github.io/git-guide/index.zh.html)

- SSH key

