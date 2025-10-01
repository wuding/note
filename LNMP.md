# LNMP

<!-- version 1.251001 -->

https://www.lnmp.org/

https://www.lnmp.com/

https://bbs.lnmp.com/forum.php



## SSL

https://www.lnmp.org/faq/letsencrypt-wildcard-ssl.html

```sh
## 域名DNS服务商API方式生成SSL证书
lnmp dnsssl <idc>
# 或者
lnmp dns <idc>

## 使用域名DNS服务商API方式只生成SSL证书不创建网站配置文件
lnmp onlyssl <idc>

## 创建SSL站点配置文件
lnmp vhost add
# 或
lnmp ssl add
```



### 首次部署

https://acme.sh/

https://www.namesilo.com/account/api-manager

```sh
export Namesilo_Key="<apiKey>"
lnmp dnsssl namesilo
```

Namesilo_Key 会保存在

> /usr/local/acme.sh/account.conf



- https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E
- https://www.moonfly.net/archives/138.html
- https://docs.certcloud.cn/docs/installation/auto/acme/acmesh/



### 检查续期

```sh
acme.sh --cron
```

查看虚拟主机目录 root

> /usr/local/nginx/conf/vhost/<domain>.conf

修改对应的 Le_Webroot

> /usr/local/nginx/conf/ssl/<domain>.conf

或者

```
dns_ali
dns_namesilo
```



### 设置定时任务

https://www.runoob.com/linux/linux-comm-crontab.html

```sh
# 列出当前的时程表
crontab -l

# 使用 vim 编辑
crontab -e
```

内容

```
16 0 * * * "/usr/local/acme.sh"/acme.sh --cron --home "/usr/local/acme.sh" > /dev/null
```

格式

```
i H d m w program
分 时 日 月 周 程序
```

位置

```sh
# 查看 root 用户的 crontab 文件内容
cat /var/spool/cron/root
```



### 单个续期

```sh
acme.sh --renew -d <domain>
```

添加 --force 强制执行
