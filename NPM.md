npm
====
https://www.npmjs.com/



### 参考:
[NPM小结](https://www.cnblogs.com/chyingp/p/npm.html)



# 配置

## package.json

https://docs.npmjs.com/files/package.json

```json
{
  "name": "(anonymous)/slack-app",
  "version": "0.0.0",
  "description": "Basic Slack Application",
  "author": "(anonymous)",
  "main": "functions/__main__.js",
  "dependencies": {
    "lib": "^3.0.1",
    "mime": "^1.3.4",
    "request": "^2.79.0",
    "slack": "^8.2.1",
    "async": "^2.1.5",
    "ejs": "^2.5.6"
  },
  "private": true,
  "stdlib": {
    "build": "faaslang",
    "name": "(anonymous)/slack-app",
    "timeout": 10000,
    "publish": true,
    "personalize": {
      "keys": [],
      "user": []
    },
    "source": "@slack/app/0.1.8"
  }
}
```




# 工具
https://runkit.com