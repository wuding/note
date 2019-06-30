# Ubuntu



## 安装 Ubuntu

### WSL

微软应用商店搜索并安装 ubuntu



## 安装 Apache

更新下软件源

```sh
sudo apt-get update
```

安装

```sh
sudo apt-get install apache2
```

网站根目录 /var/www/html

访问 127.0.0.1

重启

```sh
sudo /etc/init.d/apache2 restart
```

### 参考

- [图解Ubuntu下Apache WEB服务器的安装](https://jingyan.baidu.com/article/8275fc865b85c046a13cf673.html)



## 安装 PHP

安装

```sh
sudo apt-get install php7.0
```

建议中看到有 7.2，取消安装  7.0

```sh
sudo apt-get install php7.2
```

### 参考

- [ubuntu搭建php开发环境记录](https://www.cnblogs.com/impy/p/8040684.html)
