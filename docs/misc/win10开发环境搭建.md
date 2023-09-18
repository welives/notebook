# 我的 Win10 开发环境搭建

!> 在 Windows 系统中，我平时习惯将开发软件安装在`D`盘的`Develop`目录下

## 可能用到的软件

- [Android Studio](https://developer.android.com/studio) 或 [Android Studio SDK](https://developer.android.com/tools)
- [宝塔面板](https://www.bt.cn/new/download.html)
- [Dart](http://gekorm.com/dart-windows/)
- fvm
- [git](https://git-scm.com/downloads)
- [TortoiseGit](https://tortoisegit.org/download/)
- [Java SDK](https://www.oracle.com/hk/java/technologies/downloads/#java17)
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

接着再给用户变量`Path`增加一个值

```sh
Path => %BT_PANEL%\script
```

打开宝塔面板站点，安装 MySQL，然后给用户变量`Path`再增加一个值，这样就能在终端中使用 MySQL 命令了

```sh
Path => D:\Develop\BtSoft\mysql\MySQL5.7\bin
```

---

## git 和 TortoiseGit 的配置

先去下载[git](https://git-scm.com/downloads) 和 [TortoiseGit](https://tortoisegit.org/download/)，具体的安装步骤就不说了，百度一搜各种各样的都有，这里主要讲我对 git 的环境变量配置

安装完成后打开 TortoiseGit 设置，并修改其 SSH 客户端为`D:\Develop\Git\usr\bin\ssh.exe`

给用户变量`Path`增加以下值

```sh
Path => D:\Develop\Git\cmd
Path => D:\Develop\TortoiseGit\bin
```

接着给 git 设置用户名和邮箱，具体的邮箱地址我就不放出来了，避免泄漏

```sh
git config --global user.name Jandan
git config --global user.email ***@gmail.com
```

生成公钥，输入指令后无脑按回车就行

```sh
ssh-keygen -m PEM -t ed25519 -C "***@gmail.com"
```

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

## Dart fvm Flutter 的安装配置

先去下载[Dart](http://gekorm.com/dart-windows/)

设置安装目录为`D:\Develop\Dart`

![](./assets/dart_install_step_1.png)

安装完后会自动生成一个系统环境变量`DART_SDK`，然后给用户变量`Path`添加一个值

```sh
Path => %DART_SDK%\bin
```

下载 fvm

```sh
dart pub global activate fvm
```

再给用户变量`Path`添加一个值

```sh
Path => %LOCALAPPDATA%\Pub\Cache\bin
```

创建一个系统环境变量`FVM_HOME`并修改 fvm 的缓存路径

```sh
FVM_HOME => D:\Develop\fvm\versions
fvm config --cache-path D:\Develop\fvm\versions
```

!> 注意：由于 fvm 是 bat 脚本，需要在 cmd 或 powershell 中才能执行

查看所有可用的 flutter 版本`fvm releases`

下载安装某个 flutter 版本，这里以 3.13.4 为例：`fvm install 3.13.4`

设置已下载的某个 flutter 版本作为全局，`fvm global 3.13.4`

设置了全局版本之后，会在`versions`目录下自动生成一个`default`目录，其将作为软链接指向某个具体的版本

然后给用户变量`Path`添加一个值

```sh
Path => %FVM_HOME%\default\bin
```

最后创建两个用户环境变量`FLUTTER_STORAGE_BASE_URL`和`PUB_HOSTED_URL`

```sh
FLUTTER_STORAGE_BASE_URL => https://storage.flutter-io.cn
PUB_HOSTED_URL => https://pub.flutter-io.cn
```

?> 至此，Dart、fvm 和 flutter 的环境配置结束
