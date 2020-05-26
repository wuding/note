# Runtime



## .NET Framework

### 常见问题

#### 安装时证书错误

下载 [MicrosoftRootCertificateAuthority2011.cer](http://go.microsoft.com/fwlink/?LinkID=747875&clcid=0x409)

1. Win + R 运行 `certmgr.msc`

   或者运行 `mmc` ： 文件 > 添加或删除管理单元：证书 > 添加：计算机帐户

2. 受信任的根证书颁发机构 > 证书 | 右键菜单：所有任务 > 导入

##### 参考：

- [Microsoft .NET Framework 安装未成功（证书方面）](https://www.cnblogs.com/3tai/p/6046711.html)

- [Win7 安装.Net Framework 4.8失败，提示：已处理证书链，但是在不受信任提供程序信任的根证书中终止。或：无法建立到信任根颁发机构的证书链](https://lexsion.com/index.php/archives/183/)