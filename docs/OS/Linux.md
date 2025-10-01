## Linux

## 常见问题

### 解决 Linux 中无法删除文件的问题

#### 1. 检查文件是否被占用

```sh
lsof filename

# 终止进程
kill -9 <PID>
sudo rm filename
```

#### 2. 检查文件权限

```sh
ls -l filename

# 修改权限
sudo chmod 777 filename
```

#### 3. 移除特殊属性

```sh
lsattr filename

# 移除不可修改属性 immutable
chattr -i filename

# 移除追加属性（如有）
chattr -a filename
```

#### 4. 删除特殊字符文件

如果文件名包含特殊字符或隐藏属性，可以使用引号或转义符

```sh
sudo rm "file with spaces"
sudo rm file\ name\ with\ spaces
```

#### 5. 修复文件系统错误

```sh
sudo fsck /dev/sdX
# 替换为实际分区名
```

#### 6. 目录非空

需要递归删除整个目录及其内容

```sh
rm -rf directory_name
# 命令非常危险，务必确认目标路径，以免误删重要数据
```

- https://www.cnblogs.com/huangjiabobk/p/18088794