Composer 使用指南
=================

# 安装 Composer
http://www.phpcomposer.com/



# 配置

配置文件位置
*L:\Users\Benny\AppData\Roaming\Composer\config.json*

### Packagist 镜像使用方法
https://pkg.phpcomposer.com/
```sh
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```



# 创建与提交包

### 参考：

[101- composer [packagist]包制作（入门篇）](https://www.jianshu.com/p/1eeaad7ec31a)

[使用GitHub、Composer、Packagist管理公开的PHP包（Step By Step）](https://rivsen.github.io/post/how-to-publish-package-to-packagist-using-github-and-composer-step-by-step)

[学习开发自己的 Composer 包，并使用 GitHub 实时更新到 Packagist](https://laravel-china.org/articles/6652/learn-to-develop-their-own-composer-package-and-to-use-packagist-github-updates)




# 常见问题

### 异常 [Composer\Exception\NoSslException]
启用 PHP 扩展 openssl 即可

**参考：**
- [composer常见问题之openSSL](https://www.jianshu.com/p/46150555273b)



# 常用命令

composer install

composer update

composer require wuding/astrology:dev-secret

composer dump-autoload -o



# 帮助

composer
```sh
L:\Windows\system32>composer
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.7.1 2018-08-07 09:39:23

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

L:\Windows\system32>
```


## 创建项目

composer -h create-project
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

### 版本

```
dev-master
^1.5
>=5.4

"php": ">=5.3.3 <7.4"
"ext-mbstring": "*",
```



## 自更新

composer -h selfupdate
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

