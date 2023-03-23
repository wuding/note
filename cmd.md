# CMD 使用指南

总是“用管理员方式运行”：快捷方式 > 属性 > 高级 > 勾选



## 常用命令

#### 查看所有的端口
netstat -ano

#### 查询指定的端口占用
netstat -aon|findstr "80"

#### 查询PID对应的进行进程
tasklist|findstr "2016"
taskkill /f /t /im 程序名

#### 创建一个空文件
type nul>.htaccess

#### 启动停止服务
net start/stop nginx

#### 设置编码terminal为UTF-8
chcp 65001

#### 查看 PATH 环境变量
path

#### 刷新 DNS
ipconfig /flushdns

#### regsvr32
将这个文件复制到C:/Windows/System32下（如果系统是64位的请将32位的dll文件复制到C:\Windows\SysWOW64目录）；

然后打开“开始-运行-输入regsvr32 pthreadgc2.dll”，回车，如果报错，用下面的方法。

新建一个文件“注册.bat“，内容：
```sh
@echo 开始注册
copy pthreadgc2.dll %windir%\system32\
regsvr32 %windir%\system32\pthreadgc2.dll /s
@echo pthreadgc2.dll注册成功
@pause
```

#### regedit
```sh
regedit /s ps7.reg
```

## 参考
- https://ss64.com/nt/  <=> [nt](_assets/html/nt.html)
- http://houliang4285.iteye.com/blog/1687816
- https://en.wikipedia.org/wiki/List_of_DOS_commands
- [Windows 批处理(bat)语法大全](https://blog.csdn.net/qq_36838191/article/details/83046599)
- https://docs.microsoft.com/zh-cn/previous-versions/windows/it-pro/windows-xp/bb490890(v=technet.10)


## 变量
%SystemRoot%

%ALLUSERSPROFILE%

%APPDATA%


## 帮助

### help
```sh
C:\Users\Administrator>help
有关某个命令的详细信息，请键入 HELP 命令名
ASSOC          显示或修改文件扩展名关联。
ATTRIB         显示或更改文件属性。
BREAK          设置或清除扩展式 CTRL+C 检查。
BCDEDIT        设置启动数据库中的属性以控制启动加载。
CACLS          显示或修改文件的访问控制列表(ACL)。
CALL           从另一个批处理程序调用这一个。
CD             显示当前目录的名称或将其更改。
CHCP           显示或设置活动代码页数。
CHDIR          显示当前目录的名称或将其更改。
CHKDSK         检查磁盘并显示状态报告。
CHKNTFS        显示或修改启动时间磁盘检查。
CLS            清除屏幕。
CMD            打开另一个 Windows 命令解释程序窗口。
COLOR          设置默认控制台前景和背景颜色。
COMP           比较两个或两套文件的内容。
COMPACT        显示或更改 NTFS 分区上文件的压缩。
CONVERT        将 FAT 卷转换成 NTFS。你不能转换
               当前驱动器。
COPY           将至少一个文件复制到另一个位置。
DATE           显示或设置日期。
DEL            删除至少一个文件。
DIR            显示一个目录中的文件和子目录。
DISKPART       显示或配置磁盘分区属性。
DOSKEY         编辑命令行、撤回 Windows 命令并
               创建宏。
DRIVERQUERY    显示当前设备驱动程序状态和属性。
ECHO           显示消息，或将命令回显打开或关闭。
ENDLOCAL       结束批文件中环境更改的本地化。
ERASE          删除一个或多个文件。
EXIT           退出 CMD.EXE 程序(命令解释程序)。
FC             比较两个文件或两个文件集并显示
               它们之间的不同。
FIND           在一个或多个文件中搜索一个文本字符串。
FINDSTR        在多个文件中搜索字符串。
FOR            为一组文件中的每个文件运行一个指定的命令。
FORMAT         格式化磁盘，以便用于 Windows。
FSUTIL         显示或配置文件系统属性。
FTYPE          显示或修改在文件扩展名关联中使用的文件
               类型。
GOTO           将 Windows 命令解释程序定向到批处理程序
               中某个带标签的行。
GPRESULT       显示计算机或用户的组策略信息。
GRAFTABL       使 Windows 在图形模式下显示扩展
               字符集。
HELP           提供 Windows 命令的帮助信息。
ICACLS         显示、修改、备份或还原文件和
               目录的 ACL。
IF             在批处理程序中执行有条件的处理操作。
LABEL          创建、更改或删除磁盘的卷标。
MD             创建一个目录。
MKDIR          创建一个目录。
MKLINK         创建符号链接和硬链接
MODE           配置系统设备。
MORE           逐屏显示输出。
MOVE           将一个或多个文件从一个目录移动到另一个
               目录。
OPENFILES      显示远程用户为了文件共享而打开的文件。
PATH           为可执行文件显示或设置搜索路径。
PAUSE          暂停批处理文件的处理并显示消息。
POPD           还原通过 PUSHD 保存的当前目录的上一个
               值。
PRINT          打印一个文本文件。
PROMPT         更改 Windows 命令提示。
PUSHD          保存当前目录，然后对其进行更改。
RD             删除目录。
RECOVER        从损坏的或有缺陷的磁盘中恢复可读信息。
REM            记录批处理文件或 CONFIG.SYS 中的注释(批注)。
REN            重命名文件。
RENAME         重命名文件。
REPLACE        替换文件。
RMDIR          删除目录。
ROBOCOPY       复制文件和目录树的高级实用工具
SET            显示、设置或删除 Windows 环境变量。
SETLOCAL       开始本地化批处理文件中的环境更改。
SC             显示或配置服务(后台进程)。
SCHTASKS       安排在一台计算机上运行命令和程序。
SHIFT          调整批处理文件中可替换参数的位置。
SHUTDOWN       允许通过本地或远程方式正确关闭计算机。
SORT           对输入排序。
START          启动单独的窗口以运行指定的程序或命令。
SUBST          将路径与驱动器号关联。
SYSTEMINFO     显示计算机的特定属性和配置。
TASKLIST       显示包括服务在内的所有当前运行的任务。
TASKKILL       中止或停止正在运行的进程或应用程序。
TIME           显示或设置系统时间。
TITLE          设置 CMD.EXE 会话的窗口标题。
TREE           以图形方式显示驱动程序或路径的目录
               结构。
TYPE           显示文本文件的内容。
VER            显示 Windows 的版本。
VERIFY         告诉 Windows 是否进行验证，以确保文件
               正确写入磁盘。
VOL            显示磁盘卷标和序列号。
XCOPY          复制文件和目录树。
WMIC           在交互式命令 shell 中显示 WMI 信息。

有关工具的详细信息，请参阅联机帮助中的命令行参考。

C:\Users\Administrator>
```

### cmd /?
```sh
L:\Users\Benny>cmd /?
启动 Windows 命令解释器的一个新实例

CMD [/A | /U] [/Q] [/D] [/E:ON | /E:OFF] [/F:ON | /F:OFF] [/V:ON | /V:OFF]
    [[/S] [/C | /K] string]

/C      执行字符串指定的命令然后终止
/K      执行字符串指定的命令但保留
/S      修改 /C 或 /K 之后的字符串处理(见下)
/Q      关闭回显
/D      禁止从注册表执行 AutoRun 命令(见下)
/A      使向管道或文件的内部命令输出成为 ANSI
/U      使向管道或文件的内部命令输出成为
        Unicode
/T:fg   设置前台/背景颜色(详细信息见 COLOR /?)
/E:ON   启用命令扩展(见下)
/E:OFF  禁用命令扩展(见下)
/F:ON   启用文件和目录名完成字符(见下)
/F:OFF  禁用文件和目录名完成字符(见下)
/V:ON   使用 ! 作为分隔符启用延迟的环境变量
        扩展。例如，/V:ON 会允许 !var! 在执行时
        扩展变量 var。var 语法会在输入时
        扩展变量，这与在一个 FOR
        循环内不同。
/V:OFF  禁用延迟的环境扩展。

注意，如果字符串加有引号，可以接受用命令分隔符 "&&"
分隔多个命令。另外，由于兼容性
原因，/X 与 /E:ON 相同，/Y 与 /E:OFF 相同，且 /R 与
/C 相同。任何其他开关都将被忽略。

如果指定了 /C 或 /K，则会将该开关之后的
命令行的剩余部分作为一个命令行处理，其中，会使用下列逻辑
处理引号(")字符:

    1.  如果符合下列所有条件，则会保留
        命令行上的引号字符:

        - 不带 /S 开关
        - 正好两个引号字符
        - 在两个引号字符之间无任何特殊字符，
          特殊字符指下列字符: &<>()@^|
        - 在两个引号字符之间至少有
          一个空格字符
        - 在两个引号字符之间的字符串是某个
          可执行文件的名称。

    2.  否则，老办法是看第一个字符
        是否是引号字符，如果是，则去掉首字符并
        删除命令行上最后一个引号，保留
        最后一个引号之后的所有文本。

如果 /D 未在命令行上被指定，当 CMD.EXE 开始时，它会寻找
以下 REG_SZ/REG_EXPAND_SZ 注册表变量。如果其中一个或
两个都存在，这两个变量会先被执行。

    HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun

        和/或

    HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun

命令扩展是按默认值启用的。您也可以使用 /E:OFF ，为某一
特定调用而停用扩展。您
可以在机器上和/或用户登录会话上
启用或停用 CMD.EXE 所有调用的扩展，这要通过设置使用
REGEDIT.EXE 的注册表中的一个或两个 REG_DWORD 值:

    HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions

        和/或

    HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions

到 0x1 或 0x0。用户特定设置
比机器设置有优先权。命令行
开关比注册表设置有优先权。

在批处理文件中，SETLOCAL ENABLEEXTENSIONS 或 DISABLEEXTENSIONS 参数
比 /E:ON 或 /E:OFF 开关有优先权。请参阅 SETLOCAL /? 获取详细信息。

命令扩展包括对下列命令所做的
更改和/或添加:

    DEL or ERASE
    COLOR
    CD or CHDIR
    MD or MKDIR
    PROMPT
    PUSHD
    POPD
    SET
    SETLOCAL
    ENDLOCAL
    IF
    FOR
    CALL
    SHIFT
    GOTO
    START (同时包括对外部命令调用所做的更改)
    ASSOC
    FTYPE

有关特定详细信息，请键入 commandname /? 查看。

延迟环境变量扩展不按默认值启用。您
可以用/V:ON 或 /V:OFF 开关，为 CMD.EXE 的某个调用而
启用或停用延迟环境变量扩展。您
可以在机器上和/或用户登录会话上启用或停用 CMD.EXE 所有
调用的延迟扩展，这要通过设置使用 REGEDIT.EXE 的注册表中的
一个或两个 REG_DWORD 值:

    HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\DelayedExpansion

        和/或

    HKEY_CURRENT_USER\Software\Microsoft\Command Processor\DelayedExpansion

到 0x1 或 0x0。用户特定设置
比机器设置有优先权。命令行开关
比注册表设置有优先权。

在批处理文件中，SETLOCAL ENABLEDELAYEDEXPANSION 或 DISABLEDELAYEDEXPANSION
参数比 /V:ON 或 /V:OFF 开关有优先权。请参阅 SETLOCAL /?
获取详细信息。

如果延迟环境变量扩展被启用，
惊叹号字符可在执行时间被用来
代替一个环境变量的数值。

您可以用 /F:ON 或 /F:OFF 开关为 CMD.EXE 的某个
调用而启用或禁用文件名完成。您可以在计算上和/或
用户登录会话上启用或禁用 CMD.EXE 所有调用的完成，
这可以通过使用 REGEDIT.EXE 设置注册表中的下列
 REG_DWORD 的全部或其中之一:

    HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar
    HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar

        和/或

    HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar
    HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar

由一个控制字符的十六进制值作为一个特定参数(例如，0x4
是Ctrl-D，0x6 是 Ctrl-F)。用户特定设置优先于机器设置。
命令行开关优先于注册表设置。

如果完成是用 /F:ON 开关启用的，两个要使用的控制符是:
目录名完成用 Ctrl-D，文件名完成用 Ctrl-F。要停用
注册表中的某个字符，请用空格(0x20)的数值，因为此字符
不是控制字符。

如果键入两个控制字符中的一个，完成会被调用。完成功能将
路径字符串带到光标的左边，如果没有通配符，将通配符附加
到左边，并建立相符的路径列表。然后，显示第一个相符的路
径。如果没有相符的路径，则发出嘟嘟声，不影响显示。之后，
重复按同一个控制字符会循环显示相符路径的列表。将 Shift
键跟控制字符同时按下，会倒着显示列表。如果对该行进行了
任何编辑，并再次按下控制字符，保存的相符路径的列表会被
丢弃，新的会被生成。如果在文件和目录名完成之间切换，会
发生同样现象。两个控制字符之间的唯一区别是文件完成字符
符合文件和目录名，而目录完成字符只符合目录名。如果文件
完成被用于内置式目录命令(CD、MD 或 RD)，就会使用目录
完成。
用引号将相符路径括起来，完成代码可以正确处理含有空格
或其他特殊字符的文件名。同时，如果备份，然后从行内调用
文件完成，完成被调用时位于光标右方的文字会被调用。

需要引号的特殊字符是:
     <space>
     ()[]{}^=;!'+,`~(&()

L:\Users\Benny>
```


### mklink /?
```sh
C:\windows\system32>mklink /?
创建符号链接。

MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件
                符号链接。
        /H      创建硬链接而非符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径
                (相对或绝对)。

C:\windows\system32>
```

### sc
```sh
描述:
        SC 是用来与服务控制管理器和服务进行通信
        的命令行程序。
用法:
        sc <server> [command] [service name] <option1> <option2>...


        <server> 选项的格式为 "\\ServerName"
        可通过键入以下命令获取有关命令的更多帮助: "sc [command]"
        命令:
          query-----------查询服务的状态，
                          或枚举服务类型的状态。
          queryex---------查询服务的扩展状态，
                          或枚举服务类型的状态。
          start-----------启动服务。
          pause-----------向服务发送 PAUSE 控制请求。
          interrogate-----向服务发送 INTERROGATE 控制请求。
          continue--------向服务发送 CONTINUE 控制请求。
          stop------------向服务发送 STOP 请求。
          config----------更改服务的配置(永久)。
          description-----更改服务的描述。
          failure---------更改失败时服务执行的操作。
          failureflag-----更改服务的失败操作标志。
          sidtype---------更改服务的服务 SID 类型。
          privs-----------更改服务的所需特权。
          managedaccount--更改服务以将服务帐户密码
                          标记为由 LSA 管理。
          qc--------------查询服务的配置信息。
          qdescription----查询服务的描述。
          qfailure--------查询失败时服务执行的操作。
          qfailureflag----查询服务的失败操作标志。
          qsidtype--------查询服务的服务 SID 类型。
          qprivs----------查询服务的所需特权。
          qtriggerinfo----查询服务的触发器参数。
          qpreferrednode--查询服务的首选 NUMA 节点。
          qmanagedaccount-查询服务是否将帐户
                          与 LSA 管理的密码结合使用。
          qprotection-----查询服务的进程保护级别。
          quserservice----查询用户服务模板的本地实例。
          delete ----------(从注册表中)删除服务。
          create----------创建服务(并将其添加到注册表中)。
          control---------向服务发送控制。
          sdshow----------显示服务的安全描述符。
          sdset-----------设置服务的安全描述符。
          showsid---------显示与任意名称对应的服务 SID 字符串。
          triggerinfo-----配置服务的触发器参数。
          preferrednode---设置服务的首选 NUMA 节点。
          GetDisplayName--获取服务的 DisplayName。
          GetKeyName------获取服务的 ServiceKeyName。
          EnumDepend------枚举服务依赖关系。

        以下命令不需要服务名称:
        sc <server> <command> <option>
          boot------------(ok | bad)指示是否应将上一次启动另存为
                          最近一次已知的正确启动配置
          Lock------------锁定服务数据库
          QueryLock-------查询 SCManager 数据库的 LockStatus
示例:
        sc start MyService


QUERY 和 QUERYEX 选项:
        如果查询命令带服务名称，将返回
        该服务的状态。其他选项不适合这种
        情况。如果查询命令不带参数或
        带下列选项之一，将枚举此服务。
    type=    要枚举的服务的类型(driver, service, userservice, all)
             (默认 = service)
    state=   要枚举的服务的状态 (inactive, all)
             (默认 = active)
    bufsize= 枚举缓冲区的大小(以字节计)
             (默认 = 4096)
    ri=      开始枚举的恢复索引号
             (默认 = 0)
    group=   要枚举的服务组
             (默认 = all groups)

语法示例
sc query                - 枚举活动服务和驱动程序的状态
sc query eventlog       - 显示 eventlog 服务的状态
sc queryex eventlog     - 显示 eventlog 服务的扩展状态
sc query type= driver   - 仅枚举活动驱动程序
sc query type= service  - 仅枚举 Win32 服务
sc query state= all     - 枚举所有服务和驱动程序
sc query bufsize= 50    - 枚举缓冲区为 50 字节
sc query ri= 14         - 枚举时恢复索引 = 14
sc queryex group= ""    - 枚举不在组内的活动服务
sc query type= interact - 枚举所有不活动服务
sc query type= driver group= NDIS     - 枚举所有 NDIS 驱动程序
```

### more

```sh
mysqld --verbose --help | more
```

按下空格键显示下一屏，确定键显示下一行，q 退出
