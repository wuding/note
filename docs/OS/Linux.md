## Linux

<!-- version 2.261112 -->

## 常用软件

### ssh

```sh
systemctl restart sshd
```



#### sshd_config

> /etc/ssh/sshd_config

```sh
Port 22222

# 只某个网卡上提供sshd
ListenAddress 192.168.0.142

# 禁止root账户
PermitRootLogin no

# 失败认证的次数超过这个数值的一半
MaxAuthTries 2

# 显示上次登录信息
PrintLastLog yes

##
Banner /tmp/hi
```

> /tmp/hi

```sh
# 创建横幅文件并写入内容
echo "欢迎登陆s142" > /tmp/hi
```



##### log

https://wangdoc.com/ssh/log

```sh
journalctl -u ssh
```



##### help

```sh
man sshd_config

# 验证配置文件
sshd -t
```



### iptables

```sh
cat  /etc/sysconfig/iptables
```



- https://juejin.cn/post/7515037338939899967
- https://www.cnblogs.com/cangqinglang/p/15438461.html
- https://cloud.tencent.com/developer/ask/sof/116302295
- https://zhuanlan.zhihu.com/p/11624516834

```sh
iptables -L
iptables -t nat -L

# 允许 SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# 屏蔽 IP
iptables -A INPUT -s 192.168.1.100 -j DROP

# 删除第 3 条规则
iptables -D INPUT 3

#设置默认策略
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
```

##### 永久生效

```sh
# 保存iptables规则
service iptables save
#Ubuntu/Debian：使用 iptables-persistent 或 netfilter-persistent

# 重启iptables服务
service iptables stop
service iptables start
```



##### 常见配置示例

```sh
#允许基本服务
iptables -F
iptables -P INPUT DROP
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

```



### ipset

- https://zhuanlan.zhihu.com/p/489103374
- https://www.zzxworld.com/posts/linux-ipset-command
- https://ipset.netfilter.org/ipset.man.html
- https://www.cnblogs.com/jangbo/p/17607391.html
- https://www.cnblogs.com/wn1m/p/10919940.html
- https://www.cnblogs.com/jocongmin/p/9301965.html
- https://zhuanlan.zhihu.com/p/143301176

```sh
ipset create SETNAME TYPENAME
#TYPENAME := method:datatype[,datatype[,datatype]]
#method: bitmap, hash, list
#datatype: ip, net, mac, port, iface

#集合类型
ipset create foo bitmap:ip range 192.168.0.0/16
ipset create foo bitmap:ip,mac range 192.168.0.0/16
ipset create foo bitmap:port range 0-1024
ipset create foo hash:ip netmask 30

##命令示例
ipset create mylist hash:ip hashsize 3
#hashsize 哈希集合大小，默认为 1024
ipset add mylist 127.0.0.1
ipset add mylist 192.168.0.0/32
ipset test mylist 192.168.1.1
#清空
ipset flush mylist
#保存
ipset save mylist -f mylist.txt
#还原
ipset -exist restore -f mylist.txt
#只允许
iptables -I INPUT -m set --match-set mylist src -j ACCEPT

##hash:net
ipset create foo hash:net
ipset add foo 192.168.0.0/24
ipset add foo 10.1.0.0/16
ipset add foo 192.168.0/24
ipset add foo 192.168.0/30 nomatch
```

|               |             |                |       |
| ------------- | ----------- | -------------- | ----- |
| hash:ip       | 单个 IP     | 1.2.3.4        |       |
| hash:net      | 网段        | 1.2.3.0/24     |       |
| hash:ip,port  | IP + 端口   | 1.2.3.4,80     |       |
| bitmap:ip     | 固定范围 IP | 192.168.0.0/16 | 65536 |
| bitmap:ip,mac |             |                | 65536 |
| bitmap:port   |             |                |       |
| list:set      | 集合的集合  | 组合多个 ipset |       |



```sh
#基本命令
ipset create myset hash:ip
ipset add myset 1.2.3.4
ipset del myset 1.2.3.4
ipset list myset
ipset destroy myset


#timeout 自动过期示例
ipset create tempban hash:ip timeout 300
ipset add tempban 1.2.3.4
ipset add tempban 5.6.7.8 timeout 60
```

##### 与 iptables 配合使用

```sh
#黑名单示例
ipset create blacklist hash:ip
iptables -I INPUT -m set --match-set blacklist src -j DROP
ipset add blacklist 176.53.159.0/24
ipset test blacklist 176.53.159.197
ipset save blacklist -f mylist.txt

#白名单
iptables -I INPUT -m set ! --match-set whitelist src -j DROP

```

##### CIDR

- https://free-calculator.online/zh/network-calculators/cidr-calculator



### nftables

- https://nie11kun.github.io/archives/nftables.html
- https://www.renrendaima.com/article/29283.html
- http://ai.bjbook.net/tech/nftables_ip_block.html

##### ASN

https://blog.craftyguy.net/nft-asn-block/

```sh
#!/bin/sh

set -euf

# facebook ASNs
ASNs="AS32934 AS11917"

get_asn_ips() {
        asn="$1"
        whois -h whois.radb.net -- -i origin "$asn" |  awk '/route:/ {printf("\t\t%s,", $2)}'
}

asn_ips=

for a in $ASNs; do
        asn_ips=$(printf "%s%s" $asn_ips $(get_asn_ips $a))
done

cat  <<EOF > /root/etc/nftables.d/50-nft_asn_block.nft
#!/usr/sbin/nft -f
table inet filter {
    set asn_blocked_addresses {
        type ipv4_addr
        flags interval
        elements = {
            $asn_ips
        }
        auto-merge
    }
    chain output {
        meta nfproto ipv4 ip daddr @asn_blocked_addresses log prefix "BLOCKED BY NFT_ASN_BLOCK: " drop;
    }
}
EOF
```



### fail2ban

- https://www.cnblogs.com/hahaha111122222/p/18948141
- https://wangdoc.com/ssh/fail2ban

#### 1. install

##### Enterprise Linux 8 (RHEL 8, Rocky Linux 8, AlmaLinux 8)

https://rhel.pkgs.org/8/epel-x86_64/fail2ban-1.0.2-3.el8.noarch.rpm.html

1. Download latest epel-release rpm from

wget https://dl.fedoraproject.org/pub/epel/8/Everything/x86_64/Packages/e/epel-release-8-22.el8.noarch.rpm

2. Install epel-release rpm

   ```sh
   rpm -Uvh epel-release*rpm
   ```

3. Install fail2ban rpm package

   ```sh
   dnf install fail2ban
   ```

https://mirror.de.leaseweb.net/epel/8/Everything/x86_64/Packages/f/fail2ban-1.0.2-3.el8.noarch.rpm



#### 2 . usage

https://zhuanlan.zhihu.com/p/33546122

```sh
# conf
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

# 对于新版本 新建 jail.local 然后填写
sudo nano /etc/fail2ban/jail.local

# status
fail2ban-client status
fail2ban-client status sshd

# del ip
sudo fail2ban-client set sshd unbanip 192.168.1.100

# ban ip
sudo fail2ban-client set sshd banip 192.168.1.100

##
iptables -L
```

https://www.runoob.com/linux/linux-comm-fail2ban.html

> jail.local

```ini
[DEFAULT]
# 忽略的 IP 地址（白名单）
ignoreip = 127.0.0.1/8 ::1 192.168.1.0/24

# 封禁时间（秒）
bantime = 3600

# 检测时间窗口（秒）
findtime = 600

# 最大尝试次数
maxretry = 3

# 使用的防火墙后端
banaction = iptables-multiport

[sshd]
# 是否启用 SSH 保护
enabled = true

# 日志文件路径
logpath = %(sshd_log)s

# 过滤器名称
filter = sshd

# 端口号
port = ssh
```



#### 3. service

```sh
sudo systemctl status fail2ban

sudo systemctl start fail2ban    # 启动服务
sudo systemctl stop fail2ban     # 停止服务
sudo systemctl restart fail2ban  # 重启服务
sudo systemctl enable fail2ban   # 设置开机自启
```



#### 4. sshd-ddos

https://vpsdeck.com/fail2ban-deployment-guide/

> /etc/ssh/sshd_config

```ini
# 限制未认证连接数，超过后随机拒绝新连接
MaxStartups 10:30:60
# 10 个未认证连接时开始随机拒绝新的连接，拒绝的概率为 30%。当未认证连接达到 60 个时，所有新的连接将被拒绝

# 设置认证失败的最大尝试次数
MaxAuthTries 3

# 登录超时时间，减少资源占用
LoginGraceTime 20
```

restart

```sh
sudo systemctl restart sshd
```

> jail.local

```ini
[sshd-ddos]
enabled = true
port = 9527
filter = sshd-ddos
logpath = %(sshd_log)s
maxretry = 3
bantime = 604800
findtime = 300
```

> filter.d/sshd-ddos.conf

```sh
# Fail2Ban configuration file
#
# Author: Yaroslav Halchenko
#
# $Revision$
#

[Definition]

# Option:  failregex
# Notes.:  regex to match the password failures messages in the logfile. The
#          host must be matched by a group named "host". The tag "<HOST>" can
#          be used for standard IP/hostname matching and is only an alias for
#          (?:::f{4,6}:)?(?P<host>[\w\-.^_]+)
# Values:  TEXT
#
failregex = sshd(?:\[\d+\])?: Did not receive identification string from <HOST>$

# Option:  ignoreregex
# Notes.:  regex to ignore. If this regex matches, the line is ignored.
# Values:  TEXT
#
ignoreregex = 
```

https://github.com/mikechau/fail2ban-configs/blob/master/filter.d/sshd-ddos.conf



#### 5. nginx

https://www.cnblogs.com/fjping0606/p/10102797.html

```sh
log_format  main  '$remote_addr $status $request $body_bytes_sent [$time_local]  $http_user_agent $http_referer  $http_x_forwarded_for $upstream_addr $upstream_status $upstream_cache_status $upstream_response_time';
access_log  logs/access.log  main;
```

> /etc/fail2ban/filter.d/nginx-403.conf



```ini
# cat /etc/fail2ban/filter.d/nginx-403-cc.conf
[Definition]
failregex =403.(GET|POST)*.*HTTP/1.*$
ignoreregex =
```



> /etc/fail2ban/filter.d/nginx-403.conf

```ini
# cat /etc/fail2ban/jail.d/nginx-anti-403.conf
[nginx-anti-403]
enabled = true
port = http
filter = nginx-403-cc
logpath = /opt/nginx/logs/access.log
findtime = 6
bantime = 900
maxretry = 9
```



### whois

- https://cn.linux-console.net/?p=8297
- https://www.cnblogs.com/yinliang/p/4535159.html



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



### screen

#### 1. 关闭多个screen

```sh
# 先看看有多少个screen
screen -ls |awk '/Socket/'|awk '{print $1}'

# 如果有20个screen，关闭所有screen
screen -ls|awk 'NR>=2&&NR<=112{print $1}'|awk '{print "screen -S "$1" -X quit"}'|sh
```

- https://www.cnblogs.com/liaojie970/p/7872588.html
