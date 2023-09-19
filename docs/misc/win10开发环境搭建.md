# 我的 Win10 开发环境搭建

!> 在 Windows 系统中，我平时习惯将开发软件安装在`D`盘的`Develop`目录下

## 可能用到的软件

- [Android Studio](https://developer.android.com/studio)
- [宝塔面板](https://www.bt.cn/new/download.html)
- [Dart](http://gekorm.com/dart-windows/)
- fvm
- [git](https://git-scm.com/downloads)
- [TortoiseGit](https://tortoisegit.org/download/)
- [JDK](https://www.oracle.com/hk/java/technologies/downloads/#java17)
- [Microsoft VS Code](https://code.visualstudio.com/Download)
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)
- [nvm](https://github.com/coreybutler/nvm-windows)
- [Notepad++](https://github.com/notepad-plus-plus/notepad-plus-plus)
- Navicat Premium
- [Python](https://www.python.org/downloads/)
- XshellXftpPortable

## WSL 端口映射

在 windows10 中，由于每次重启电脑后，WSL 虚拟机的 ip 地址有可能会发生变化，所以需要重新映射

```sh
netsh interface portproxy add v4tov4 listenport=[win10端口] listenaddress=0.0.0.0 connectport=[虚拟机的端口] connectaddress=[虚拟机的ip]
```

检查是否映射成功`netsh interface portproxy show all`

---

## 宝塔面板

先去下载 windows 版本的[宝塔面板](https://www.bt.cn/new/download.html)，安装到`D:\Develop\BtSoft`目录

安装完成后会自动创建如下几个环境变量

```sh
BT_PANEL => D:\Develop\BtSoft\panel
BT_PYTHON => C:\Program Files\python
BT_SETUP => D:\Develop\BtSoft
```

接着再给用户变量`Path`增加一个值`%BT_PANEL%\script`

打开宝塔面板站点，安装 MySQL，然后给用户变量`Path`再增加一个值`D:\Develop\BtSoft\mysql\MySQL5.7\bin`，这样就能在终端中使用 MySQL 命令了

---

## git 和 TortoiseGit 的配置

先去下载[git](https://git-scm.com/downloads) 和 [TortoiseGit](https://tortoisegit.org/download/)，具体的安装步骤就不说了，百度一搜各种各样的都有，这里主要讲我对 git 的环境变量配置

安装完成后打开 TortoiseGit 设置，并修改其 SSH 客户端为`D:\Develop\Git\usr\bin\ssh.exe`

给用户变量`Path`增加以下两个值`D:\Develop\Git\cmd`和`D:\Develop\TortoiseGit\bin`

接着给 git 设置用户名和邮箱，具体的邮箱地址我就不放出来了，避免泄漏

```sh
git config --global user.name Jandan
git config --global user.email ***@gmail.com
```

生成公钥，输入指令后无脑按回车就行

```sh
ssh-keygen -m PEM -t ed25519 -C "***@gmail.com"
```

打开 github 网站，进入设置，填入刚才生成的`ssh key`，如果是首次和 github 建立连接，则输入`ssh -T git@github.com`创建连接信息

对于使用 http 方式连接远程仓库的，输入以下指令用来保存账号和密码

```sh
git config credential.helper store
```

---

## nvm 和 nodejs 的配置

先去 Github 下载[nvm](https://github.com/coreybutler/nvm-windows)

设置安装目录为`D:\Develop\nvm`

![](./assets/nvm_install_step_1.png)

设置 nodejs 软链接指向的目录为`D:\Develop\nvm\nodejs`，之后就无脑下一步

![](./assets/nvm_install_step_2.png)

安装完成后会自动生成两个系统环境变量`NVM_HOME`和`NVM_SYMLINK`

然后使用 nvm 命令安装所需要的 nodejs 版本，我这里安装的是最新的 LTS 版本

```sh
nvm install 18.17.1
nvm use 18.17.1
```

接着在 nvm 安装目录下新建如下几个文件夹

- `node_global`
  - `node_modules`
- `node_cache`
- `Yarn`
  - `Global`
  - `Cache`

创建好上述几个文件夹后开始接着补环境变量

```sh
Path => %NVM_HOME%\node_global
NODE_PATH => %NVM_HOME%\node_global\node_modules
```

修改 npm 的缓存目录、全局包存放目录和设置淘宝源

```sh
npm config set prefix "D:\Develop\nvm\node_global"
npm config set cache "D:\Develop\nvm\node_cache"
npm config set registry https://registry.npm.taobao.org
```

安装 yarn

```sh
npm i -g yarn
```

修改 yarn 的缓存目录和全局包存放目录

```sh
yarn config set global-folder "D:\Develop\nvm\Yarn\Global"
yarn config set cache-folder "D:\Develop\nvm\Yarn\Cache"
```

我常用的 npm 全局包

- yarn
- ts-node
- whistle
- nodemon
- pm2
- live-server
- prisma
- appium
- appium-doctor
- docsify-cli
- eas-cli
- @vue/cli
- @tarojs/cli
- @nestjs/cli
- @ant-design/pro-cli

?> 至此，nvm 和 nodejs 的环境配置结束

---

## JDK 配置

1. 根据所安装的 JDK 版本，新建一个带有大版本号的用户变量，例如`JAVA17_HOME`，变量值填入安装路径`D:\Develop\Java\jdk-17`
2. 再新建一个用户变量`JAVA_HOME`，变量值填入`%JAVA17_HOME%`。如果安装有多个 JDK 版本时，只需要修改`JAVA_HOME`变量的值即可切换全局的 JDK 版本，例如修改为`%JAVA11_HOME%`
3. 给用户变量`Path`添加一个值`%JAVA_HOME%\bin`

---

## Android Studio 配置

安装 Android Studio 并下载所需要的相关 SDK 后可以选择设置全局环境变量也可以给某个 Flutter 版本设置局部环境变量：

- 全局环境变量
  1. 新建用户变量`ANDROID_HOME`和`ANDROID_SDK_ROOT`，变量值填入`D:\Develop\Android\Sdk`
  2. 新建用户变量`ANDROID_SDK_HOME`，变量值填入`D:\Develop\Android\AVD`，这个是设置虚拟机路径
  3. 给用户变量`Path`添加以下值`%ANDROID_HOME%`、`%ANDROID_SDK_HOME%`、`%ANDROID_HOME%\platform-tools`、`%ANDROID_HOME%\emulator`、`%ANDROID_HOME%\tools`和`%ANDROID_HOME%\tools\bin`
- Flutter 局部环境变量
  1. 给用户变量`Path`添加`%ANDROID_HOME%\platform-tools`
  2. 接着在命令行终端输入`flutter config --android-sdk D:\Develop\Android\Sdk`

由于 gradle 的默认缓存目录是在 C 盘，所以需要更改到其他位置，新建用户变量`GRADLE_USER_HOME`，变量值填入`F:\AndroidStudioCache\.gradle`

!> 新安装 Android Studio 时，由于 jre 目录的文件不完整，需要把整个 jre 目录删除，然后在软件根目录下执行命令 mklink /D "jre" "jbr"创建一个软链接即可

一些常用的安卓模拟器命令

- 查看安装有哪些安卓模拟器：`emulator -list-avds`
- 修改安卓模拟器的 dns 网关地址，实现模拟器联网：`emulator -avd 模拟器名 -dns-server 局域网网关`

---

## Dart、fvm、Flutter 的安装配置

先去下载[Dart](http://gekorm.com/dart-windows/)

设置安装目录为`D:\Develop\Dart`

![](./assets/dart_install_step_1.png)

安装完后会自动生成一个系统环境变量`DART_SDK`，然后给用户变量`Path`添加一个值`%DART_SDK%\bin`

下载 fvm

```sh
dart pub global activate fvm
```

再给用户变量`Path`添加一个值`%LOCALAPPDATA%\Pub\Cache\bin`

创建一个系统变量`FVM_HOME`，填入值`D:\Develop\fvm`，然后使用命令修改 fvm 的缓存路径

```sh
fvm config --cache-path D:\Develop\fvm
```

!> 注意：由于 fvm 是 bat 脚本，需要在 cmd 或 powershell 中才能执行

查看所有可用的 flutter 版本`fvm releases`

下载安装某个 flutter 版本，这里以 3.13.4 为例：`fvm install 3.13.4`

设置已下载的某个 flutter 版本作为全局，`fvm global 3.13.4`

设置了全局版本之后，会在`D:\Develop\fvm`目录下自动生成一个`default`目录，其将作为软链接指向某个具体的版本

然后给用户变量`Path`添加一个值`%FVM_HOME%\default\bin`

最后创建两个用户环境变量`FLUTTER_STORAGE_BASE_URL`和`PUB_HOSTED_URL`

```sh
FLUTTER_STORAGE_BASE_URL => https://storage.flutter-io.cn
PUB_HOSTED_URL => https://pub.flutter-io.cn
```

?> 更多 fvm 命令可以查看[官方文档](https://fvm.app/docs/guides/basic_commands)

!> 在运行`flutter doctor`进行环境检查时，如果提示 `HTTP Host Availability`，则按以下处理：

- 打开`flutter\packages\flutter_tools\lib\src\http_host_validator.dart`文件
- 修改`kPubDevHttpHost`的值为`https://pub.flutter-io.cn/`
- 修改`kgCloudHttpHost`的值为`https://storage.flutter-io.cn/`
- 修改`androidRequiredHttpHosts`的值为`https://maven.aliyun.com/repository/google/`

?> 至此，Dart、fvm 和 flutter 的环境配置结束

---
