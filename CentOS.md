CentOS
======

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
```
menuentry 'Windows 10' {
set root=(hd0,1)
chainloader +1
}
```



## 修改默认引导

打开终端输入命令
```
grub2-set-default  'Windows 10'
grub2-editenv list
```
