CentOS
======

<!-- version 3.250803 -->

# 安装

## 从U盘安装

### 下载镜像文件
http://mirror.centos.org/centos/7.5.1804/isos/x86_64/

### 制作U盘启动盘
1. UltralISO 打开 CentOS-7-x86_64-DVD-1804.iso
2. 启动 > 写入硬盘映像：格式化，写入方式 USB-HDD+，如果写入失败，换一个 USB 接口试试
3. U盘卷标改为 CENTOS

### 利用U盘引导
1. 电脑开机时按住 F12，然后选择U盘
2. 选中 Install CentOS 7，按下 Tab 键，将 LABAL 设置为 CENTOS（网上有另一种方式较复杂）

### 安装选项
* 先选自动配置分区，确定后再选我要配置分区，手动调整
* 安装引导装载程序



# 引导

## 添加 Windows
文本编辑器打开 /boot/grub2/grub.cfg

在第一个 menuentry 前添加
```ini
menuentry 'Windows 10' {
set root=(hd0,1)
chainloader +1
}
```



## 修改默认引导

打开终端输入命令
```sh
grub2-set-default  'Windows 10'
grub2-editenv list
```



# 配置

### hosts

```sh
sudo yum install vim -y
# debian
sudo apt install vim -y
```

安装使用

```sh
sudo vim /etc/hosts
```

按 i 编辑模式

格式如下：

```
IP地址 主机名/域名 主机名别名
127.0.0.1 localhost local
```

按 Esc

输入 :wq 保存退出

```sh
cat /etc/hosts
# 查看修改结果
```

重启网络服务：

```sh
service network restart
# 或者
/etc/init.d/network restart
# 或者
systemctl restart network
```

- [centos下修改hosts文件以及生效命令](https://www.cnblogs.com/pxblog/p/14838530.html)
- [centos下配置修改hosts文件以及生效命令详解](https://www.cnblogs.com/2zly/p/17227532.html)

