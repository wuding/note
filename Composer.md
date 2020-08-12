# Composer 使用指南



## 目录

[安装 Composer](#安装-composer)

- [Windows 安装](#windows-安装)

- [Linux / Mac 安装](#linux--mac-安装)

[配置 Composer](#配置-composer)

- [查看配置](#查看配置)
- [修改配置](#修改配置)
- [Packagist 镜像使用方法](#packagist-镜像使用方法)

[composer.json 配置](#composerjson-配置)

[创建与提交包](#创建与提交包)

- [资源库自动更新包](#资源库自动更新包)
- [Composer 包资源库网站](#composer-包资源库网站)

[常见问题](#常见问题)

[常用命令](#常用命令)

- [帮助](#帮助)
- [创建项目](#创建项目)
- [自更新](#自更新)
- [更新包](#更新包)
- [安装包](#安装包)
- [自动加载](#自动加载)

[参考](#参考-2 )



## 安装 Composer
https://getcomposer.org/

https://www.phpcomposer.com/



### Windows 安装

https://getcomposer.org/Composer-Setup.exe

#### [手动安装](https://getcomposer.org/download/)
- 下载 [composer.phar](https://getcomposer.org/composer.phar) 到 php 目录（设置好环境变量）
```sh
php composer.phar install
```
- 新建 composer.bat，内容如下：
```sh
@php "%~dp0composer.phar" %*
```

```sh
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```



### Linux / Mac 安装

```sh
curl -sS https://getcomposer.org/installer | php
# 全局安装
mv composer.phar /usr/local/bin/composer
```

或者：
```sh
wget https://dl.laravel-china.org/composer.phar -O /usr/local/bin/composer
chmod a+x /usr/local/bin/composer
```
如遇权限不足，可添加 `sudo`



## 配置 Composer

#### 环境变量

系统变量

C:\ProgramData\ComposerSetup\bin

用户变量

C:\Users\86176\AppData\Roaming\Composer\vendor\bin



### 查看配置

配置文件位置：
*L:\Users\Benny\AppData\Roaming\Composer\config.json*

查看镜像地址：
```sh
$ composer config -g repo.packagist
{"type":"composer","url":"https://packagist.org","allow_ssl_downgrade":true}
```

查看所有全局配置：
```sh
composer config -l -g
```

```sh
[repositories.packagist.org.type] composer
[repositories.packagist.org.url] https?://repo.packagist.org
[repositories.packagist.org.allow_ssl_downgrade] true
[process-timeout] 300
[use-include-path] false
[preferred-install] auto
[notify-on-install] true
[github-protocols] [https, ssh]
[vendor-dir] vendor (D:\www\work\couponiang/vendor)
[bin-dir] {$vendor-dir}/bin (D:\www\work\couponiang/vendor/bin)
[cache-dir] C:/Users/Administrator/AppData/Local/Composer
[data-dir] C:/Users/Administrator/AppData/Roaming/Composer
[cache-files-dir] {$cache-dir}/files (C:/Users/Administrator/AppData/Local/Composer/files)
[cache-repo-dir] {$cache-dir}/repo (C:/Users/Administrator/AppData/Local/Composer/repo)
[cache-vcs-dir] {$cache-dir}/vcs (C:/Users/Administrator/AppData/Local/Composer/vcs)
[cache-ttl] 15552000
[cache-files-ttl] 15552000
[cache-files-maxsize] 300MiB (314572800)
[bin-compat] auto
[discard-changes] false
[autoloader-suffix]
[sort-packages] false
[optimize-autoloader] false
[classmap-authoritative] false
[apcu-autoloader] false
[prepend-autoloader] true
[github-domains] [github.com]
[bitbucket-expose-hostname] true
[disable-tls] false
[secure-http] true
[cafile]
[capath]
[github-expose-hostname] true
[gitlab-domains] [gitlab.com]
[store-auths] prompt
[archive-format] tar
[archive-dir] .
[htaccess-protect] true
[home] C:/Users/Administrator/AppData/Roaming/Composer
```



### 修改配置

```sh
composer config --global data-dir /www/.composer
```



### Packagist 镜像使用方法

https://pkg.phpcomposer.com/
```sh
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
带 -g 为全局，不带为当前项目

```json
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```



## composer.json 配置
```json
{
    "name": "monolog/monolog",
    "type": "library",
    "description": "Logging for PHP 5.3",
    "keywords": ["log","logging"],
    "homepage": "https://github.com/Seldaek/monolog",
    "license": "MIT",
    "authors": [
        {
            "name": "Jordi Boggiano",
            "email": "j.boggiano@seld.be",
            "homepage": "http://seld.be",
            "role": "Developer"
        }
    ],
    "require": {
        "php": ">=5.3.0",
        "ext-curl": "*",
        "another-vendor/package": "1.*"
    },
    "autoload": {
        "psr-0": {
            "Monolog": "src"
        }
    },
    "minimum-stability": "dev"
}
```



## 创建与提交包
- 版本库根目录下运行命令 `composer init` 创建配置文件 composer.json
- 自动加载依赖 `require 'vendor/autoload.php';`
- 提交 https://packagist.org/packages/submit



### 资源库自动更新包

#### 添加 Webhook
- 装载网址 https://packagist.org/api/github?username=<用户名>

  Bitbucket 的是 https://packagist.org/api/bitbucket?username=<用户名>&apiToken=API_TOKEN
- 内容类型 application/json
- 密钥 https://packagist.org/profile/ 你的 API Token
- 事件只需要 push

#### 手动钩子
- 网址 https://packagist.org/api/update-package?username=wuding&apiToken=API_TOKEN
- 请求方法 POST
- 请求内容 `{"repository":{"url":"PACKAGIST_PACKAGE_URL"}}`
```sh
curl -XPOST -H'content-type:application/json' 'https://packagist.org/api/update-package?username=USER_NAME&apiToken=API_TOKEN' -d'{"repository":{"url":"PACKAGIST_PACKAGE_URL"}}'
```

### [Composer 包资源库网站](https://github.com/composer/packagist)
- https://packagist.org/
- https://packagist.com/ 私有库
- https://asset-packagist.org/ 前端

[镜像](https://packagist.org/mirrors)：
- https://pkg.phpcomposer.com/ 设置为 https://packagist.phpcomposer.com
- https://developer.aliyun.com/composer 设置为 https://mirrors.aliyun.com/composer/
- https://php.cnpkg.org/
- [Composer 国内加速：可用镜像列表](https://learnku.com/composer/wikis/30594)

#### 参考：

[101- composer [packagist]包制作（入门篇）](https://www.jianshu.com/p/1eeaad7ec31a)

[使用GitHub、Composer、Packagist管理公开的PHP包（Step By Step）](https://rivsen.github.io/post/how-to-publish-package-to-packagist-using-github-and-composer-step-by-step)

[学习开发自己的 Composer 包，并使用 GitHub 实时更新到 Packagist](https://laravel-china.org/articles/6652/learn-to-develop-their-own-composer-package-and-to-use-packagist-github-updates)




## 常见问题

### 异常 [Composer\Exception\NoSslException]
启用 PHP 扩展 openssl 即可

**参考：**
- [composer常见问题之openSSL](https://www.jianshu.com/p/46150555273b)



### exists as ... but these are rejected by your constraint

- `1.0.*`
- `dev-master`
- `dev-master#<hash>`
- `@dev`



### 命名空间使用

要和 composer.json 中 autoload 设置的一样，大小写敏感



## 常用命令

composer install

composer update

composer require wuding/astrology:dev-secret

composer dump-autoload -o



### 帮助

composer
```sh
C:\windows\system32>composer
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.8.4 2019-02-11 10:52:10

Usage:
  command [options] [arguments]

Options:
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  about                Shows the short information about Composer.
  archive              Creates an archive of this composer package.
  browse               Opens the package's repository URL or homepage in your browser.
  check-platform-reqs  Check that platform requirements are satisfied.
  clear-cache          Clears composer's internal package cache.
  clearcache           Clears composer's internal package cache.
  config               Sets config options.
  create-project       Creates new project from a package into given directory.
  depends              Shows which packages cause the given package to be installed.
  diagnose             Diagnoses the system to identify common errors.
  dump-autoload        Dumps the autoloader.
  dumpautoload         Dumps the autoloader.
  exec                 Executes a vendored binary/script.
  global               Allows running commands in the global composer dir ($COMPOSER_HOME).
  help                 Displays help for a command
  home                 Opens the package's repository URL or homepage in your browser.
  i                    Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  info                 Shows information about packages.
  init                 Creates a basic composer.json file in current directory.
  install              Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  licenses             Shows information about licenses of dependencies.
  list                 Lists commands
  outdated             Shows a list of installed packages that have updates available, including their latest version.
  prohibits            Shows which packages prevent the given package from being installed.
  remove               Removes a package from the require or require-dev.
  require              Adds required packages to your composer.json and installs them.
  run-script           Runs the scripts defined in composer.json.
  search               Searches for packages.
  self-update          Updates composer.phar to the latest version.
  selfupdate           Updates composer.phar to the latest version.
  show                 Shows information about packages.
  status               Shows a list of locally modified packages, for packages installed from source.
  suggests             Shows package suggestions.
  u                    Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  update               Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  upgrade              Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  validate             Validates a composer.json and composer.lock.
  why                  Shows which packages cause the given package to be installed.
  why-not              Shows which packages prevent the given package from being installed.

C:\windows\system32>
```



### 创建项目

composer create-project

```sh
composer -vvv create-project laravel/laravel blog --prefer-dist

# 指定版本
composer create-project laravel/laravel=5.2.*
```

```sh
L:\Windows\system32>composer -h create-project
Usage:
  create-project [options] [--] [<package>] [<directory>] [<version>]

Arguments:
  package                              Package name to be installed
  directory                            Directory where the files should be created
  version                              Version, will default to latest

Options:
  -s, --stability=STABILITY            Minimum-stability allowed (unless a version is specified).
      --prefer-source                  Forces installation from package sources when possible, including VCS information.
      --prefer-dist                    Forces installation from package dist even for dev versions.
      --repository=REPOSITORY          Pick a different repository (as url or js on config) to look for the package.
      --repository-url=REPOSITORY-URL  DEPRECATED: Use --repository instead.
      --dev                            Enables installation of require-dev packages (enabled by default, only present for BC).
      --no-dev                         Disables installation of require-dev packages.
      --no-custom-installers           DEPRECATED: Use no-plugins instead.
      --no-scripts                     Whether to prevent execution of all defined scripts in the root package.
      --no-progress                    Do not output download progress.
      --no-secure-http                 Disable the secure-http config option temporarily while installing the root package. Use at your own risk. Using this fla
g is a bad idea.
      --keep-vcs                       Whether to prevent deleting the vcs folder.
      --remove-vcs                     Whether to force deletion of the vcs folder without prompting.
      --no-install                     Whether to skip installation of the package dependencies.
      --ignore-platform-reqs           Ignore platform requirements (php & ext-packages).
  -h, --help                           Display this help message
  -q, --quiet                          Do not output any message
  -V, --version                        Display this application version
      --ansi                           Force ANSI output
      --no-ansi                        Disable ANSI output
  -n, --no-interaction                 Do not ask any interactive question
      --profile                        Display timing and memory usage information
      --no-plugins                     Whether to disable plugins.
  -d, --working-dir=WORKING-DIR        If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose                 Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  The create-project command creates a new project from a given
  package into a new directory. If executed without params and in a directory
  with a composer.json file it installs the packages for the current project.

  You can use this command to bootstrap new projects or setup a clean
  version-controlled installation for developers of your project.

  php composer.phar create-project vendor/project target-directory [version]

  You can also specify the version with the package name using = or : as separator.

  php composer.phar create-project vendor/project:version target-directory

  To install unstable packages, either specify the version you want, or use the
  --stability=dev (where dev can be one of RC, beta, alpha or dev).

  To setup a developer workable version you should create the project using the source
  controlled code by appending the '--prefer-source' flag.

  To install a package from another repository than the default one you
  can pass the '--repository=https://myrepository.org' flag.


L:\Windows\system32>
```

#### 版本

```
dev-master
^1.5
>=5.4

"php": ">=5.3.3 <7.4"
"ext-mbstring": "*",
```
##### 参考：
- [语义化版本](https://github.com/semver/semver.org/tree/gh-pages/lang/zh-CN)



### 自更新

composer selfupdate
```sh
L:\Users\Benny>composer -h selfupdate
Usage:
  self-update [options] [--] [<version>]
  selfupdate

Arguments:
  version                        The version to update to

Options:
  -r, --rollback                 Revert to an older installation of composer
      --clean-backups            Delete old backups during an update. This makes
 the current version of composer the only backup available after the update
      --no-progress              Do not output download progress.
      --update-keys              Prompt user for a key update
      --stable                   Force an update to the stable channel
      --preview                  Force an update to the preview channel
      --snapshot                 Force an update to the snapshot channel
      --set-channel-only         Only store the channel as the default one and t
hen exit
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as workin
g directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for norma
l output, 2 for more verbose output and 3 for debug

Help:
  The self-update command checks getcomposer.org for newer
  versions of composer and if found, installs the latest.

  php composer.phar self-update


L:\Users\Benny>
```



### 更新包

composer update

```sh
D:\www\work\couponiang>composer -h update
Usage:
  update [options] [--] [<packages>]...
  u
  upgrade

Arguments:
  packages                       Packages that should be updated, if not provided all packages are.

Options:
      --prefer-source            Forces installation from package sources when possible, including VCS information.
      --prefer-dist              Forces installation from package dist even for dev versions.
      --dry-run                  Outputs the operations but will not execute anything (implicitly enables --verbose).
      --dev                      Enables installation of require-dev packages (enabled by default, only present for BC).
      --no-dev                   Disables installation of require-dev packages.
      --lock                     Only updates the lock file hash to suppress warning about the lock file being out of date.
      --no-custom-installers     DEPRECATED: Use no-plugins instead.
      --no-autoloader            Skips autoloader generation
      --no-scripts               Skips the execution of all scripts defined in composer.json file.
      --no-progress              Do not output download progress.
      --no-suggest               Do not show package suggestions.
      --with-dependencies        Add also dependencies of whitelisted packages to the whitelist, except those defined in root package.
      --with-all-dependencies    Add also all dependencies of whitelisted packages to the whitelist, including those defined in root package.
  -o, --optimize-autoloader      Optimize autoloader during autoloader dump.
  -a, --classmap-authoritative   Autoload classes from the classmap only. Implicitly enables `--optimize-autoloader`.
      --apcu-autoloader          Use APCu to cache found/not-found classes.
      --ignore-platform-reqs     Ignore platform requirements (php & ext- packages).
      --prefer-stable            Prefer stable versions of dependencies.
      --prefer-lowest            Prefer lowest versions of dependencies.
  -i, --interactive              Interactive interface with autocompletion to select the packages to update.
      --root-reqs                Restricts the update to your first degree dependencies.
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  The update command reads the composer.json file from the
  current directory, processes it, and updates, removes or installs all the
  dependencies.

  php composer.phar update

  To limit the update operation to a few packages, you can list the package(s)
  you want to update as such:

  php composer.phar update vendor/package1 foo/mypackage [...]

  You may also use an asterisk (*) pattern to limit the update operation to package(s)
  from a specific vendor:

  php composer.phar update vendor/package1 foo/* [...]

  To select packages names interactively with auto-completion use -i.
```



### 安装包

composer install

已存在 composer.lock 文件，先删除

```sh
D:\www\work\couponiang>composer -h install
Usage:
  install [options] [--] [<packages>]...
  i

Arguments:
  packages                       Should not be provided, use composer require instead to add a given package to composer.json.

Options:
      --prefer-source            Forces installation from package sources when possible, including VCS information.
      --prefer-dist              Forces installation from package dist even for dev versions.
      --dry-run                  Outputs the operations but will not execute anything (implicitly enables --verbose).
      --dev                      Enables installation of require-dev packages (enabled by default, only present for BC).
      --no-dev                   Disables installation of require-dev packages.
      --no-custom-installers     DEPRECATED: Use no-plugins instead.
      --no-autoloader            Skips autoloader generation
      --no-scripts               Skips the execution of all scripts defined in composer.json file.
      --no-progress              Do not output download progress.
      --no-suggest               Do not show package suggestions.
  -o, --optimize-autoloader      Optimize autoloader during autoloader dump
  -a, --classmap-authoritative   Autoload classes from the classmap only. Implicitly enables `--optimize-autoloader`.
      --apcu-autoloader          Use APCu to cache found/not-found classes.
      --ignore-platform-reqs     Ignore platform requirements (php & ext- packages).
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  The install command reads the composer.lock file from
  the current directory, processes it, and downloads and installs all the
  libraries and dependencies outlined in that file. If the file does not
  exist it will look for composer.json and do the same.

  php composer.phar install
```



### 自动加载

`composer dump-autoload`

如果手动更新了 composer.json



## 参考

- [Composer 中文文档](https://learnku.com/docs/composer/2018)
- https://github.com/jolicode/composer-cheatsheet
