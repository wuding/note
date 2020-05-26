SourceTree 使用指南
=================

- [SourceTree安装与使用](https://www.cnblogs.com/fisherbook/p/11397168.html)

- [SourceTree安装（小白特别详细教程）](https://www.jianshu.com/p/dce21c4e88fc)



## 安装 SourceTree

须要 .NET Framework 4.7.1 才可安装

安装时会同时安装 Git 和 Mercurial

默认安装路径 C:\Users\Administrator\AppData\Local\SourceTree



## 常见问题

### 安装时无法登录

%LocalAppData%\Atlassian\SourceTree\accounts.json

```json
[
  {
    "$id": "1",
    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",
    "Authenticate": true,
    "HostInstance": {
      "$id": "2",
      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",
      "Host": {
        "$id": "3",
        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",
        "Id": "atlassian account"
      },
      "BaseUrl": "https://id.atlassian.com/"
    },
    "Credentials": {
      "$id": "4",
      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",
      "Username": "",
      "Email": null
    },
    "IsDefault": false
  }
]
```

创建如上文件后安装，3.3.8 试过很多次都不行，2.6.10 可以正常登录