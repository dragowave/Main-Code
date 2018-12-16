# 开源语音助手

## 目录

     + [目录](#table-of-contents)

+ [入门](#getting-started)
+ [运行](#running-mycroft)
+ [使用](#using-mycroft)

  * [*Home* 设备和账户管理](#home-device-and-account-manager)

  * [技能](#skills)
+ [后台](#behind-the-scenes)

  * [配对信息](#pairing-information)
  * [认证](#configuration)
  * [无Home使用](#using-mycroft-without-home)
  * [API 密钥服务](#api-key-services)
  * [代理后面使用](#using-mycroft-behind-a-proxy)
    + [没有认证的代理后面使用](#using-mycroft-behind-a-proxy-without-authentication)
    + [有认证的代理后面使用](#using-mycroft-behind-an-authenticated-proxy)
+ [参与进来](#getting-involved)
+ [链接](#links)

## 入门

- `cd ~/`
- `git clone --depth=1 https://github.com/MycroftAI/mycroft-core.git`
- `cd mycroft-core`
- `bash dev_setup.sh`


此脚本设置依赖项和[virtualenv][about-virtualenv]。 如果在Ubuntu / Debian，Arch或Fedora之外的环境中运行，您可能需要按照说明手动安装软件包dev_setup.sh.

[about-virtualenv]:https://virtualenv.pypa.io/en/stable/

注意：此存储库的默认分支是“dev”，应将其视为正在进行的工作。 如果要克隆更稳定的版本，请切换到“master”分支。
注意：如果您愿意为此项目做出贡献，请通过以下方式克隆整个存储库

- `git clone  https://github.com/MycroftAI/mycroft-core.git`
 或者
- `git clone --depth=1 https://github.com/MycroftAI/mycroft-core.git`

## 运行

提供`start-mycroft.sh`来执行常见任务。 该脚本使用由`dev_setup.sh`创建的virtualenv。 假设您在主目录中安装运行：
- `cd ~/mycroft-core`
- `./start-mycroft.sh debug`

“debug”命令将启动后台服务（麦克风监听器，技能，消息总线和音频子系统）以及启动基于文本的命令行界面（CLI），您可以使用它进行交互并查看内容，各种日志。 或者您可以运行`./start-mycroft.sh all`以在没有命令行界面的情况下开始服务。 稍后您可以使用CLI启动CLI `./start-mycroft.sh cli`.

后台服务可以作为一个组停止:
- `./stop-mycroft.sh`

# 使用

### 硬件

维护一个Home的设备和帐户管理系统。 开发人员可以注册: https://home.mycroft.ai

默认情况下，Main Core配置为使用Home。 通过说“嘿，配对我的设备”（或任何其他请求口头请求），您将被告知您的设备需要配对。 他会说一个6位数的代码，您可以在[Home](https://home.mycroft.ai)中输入配对页面。

配对后，您的设备将使用API密钥进行语音到文本（STT），天气和各种其他技能等服务。

### 技能

如果没有技能，就没用了。 有一些默认技能会自动下载到您的`/ opt / mycroft / skills`目录，但大多数需要自己安装。 请参阅[Skill Repo](https://github.com/MycroftAI/mycroft-skills#welcome)以发现他人的技能。 请分享你自己有趣的工作！

# 后台

### 配对信息

通过注册生成的配对信息存储在：
`〜/ .mycroft / identity / identity2.json` 

### 配置

配置包含4个可能的位置：
 - ``mycroft-core/mycroft/configuration/mycroft.conf``（默认）
 -  [Home](https://home.mycroft.ai) （远程）
 - `/etc/mycroft/mycroft.conf`(机器）
 - `$HOME/.mycroft/mycroft.conf`（用户）

配置加载程序启动时，它会按此顺序查看这些位置，并加载所有配置。最后一个文件将覆盖多个配置文件中存在的键以包含该值。此过程导致为特定设备和用户编写的最小量，而无需修改默认分发文件。

## 在没有wifi的情况下

如果您不想使用Home服务，可以将自己的API密钥插入下面<b>配置</ b>中列出的配置文件中。

插入API密钥的位置如下所示：

`[WeatherSkill]`
`api_key = ""`

将相关密钥放在引号内，立即开始使用密钥。

## API密钥服务

当前使用的密钥：

 -  [STT API，Google STT，Google Cloud Speech](http://www.chromium.org/developers/how-tos/api-keys)
 -  [天气技能API](http://openweathermap.org/api)
 -  [Wolfram-Alpha Skill](http://products.wolframalpha.com/api/)

## 在代理下使用

许多学校，大学和工作场所在他们的网络上运行“代理”。如果您需要输入用户名和密码来访问外部互联网，那么您很可能在“代理”之后。

如果您计划在代理后面使用，则需要执行其他配置步骤。

_注意：要完成此步骤，您需要知道代理服务器的`hostname`和`port`。您的网络管理员将能够提供这些详细信息。您的网络管理员可能需要有关将使用何种类型的流量的信息。我们在端口`443`上使用`https`流量，主要用于访问基于ReST的API。

### 在没有身份验证的情况下在代理后面使用

如果您在没有身份验证的情况下在代理后面使用，请添加以下环境变量，更改`proxy_hostname.com`和`proxy_port`以获取网络的值。这些命令从Linux命令行界面（CLI）执行。

```庆典
$ export http_proxy = http：//proxy_hostname.com：proxy_port
$ export https_port = http：//proxy_hostname.com：proxy_port
$ export no_proxy =“localhost，127.0.0.1，localaddress，.localdomain.com，0.0.0.0，:: 1”
```

### 在经过身份验证的代理后面使用 

如果您位于需要身份验证的代理后面，请添加以下环境变量，更改网络值的`proxy_hostname.com`和`proxy_port`。这些命令从Linux命令行界面（CLI）执行。

``` 庆典
$ export http_proxy = http：// user：password@proxy_hostname.com：proxy_port
$ export https_port = http：// user：password@proxy_hostname.com：proxy_port
$ export no_proxy =“localhost，127.0.0.1，localaddress，.localdomain.com，0.0.0.0，:: 1”
```

# 参与进来

这是一个开源项目，我们很乐意为您提供帮助。我们准备了[贡献](github / CONTRIBUTING.md)指南来帮助您入门。

# 链接
* [文档](https://docs.mycroft.ai)
* [发行说明](https://github.com/MycroftAI/mycroft-core/releases)
* [聊天机器人](https://chat.mycroft.ai)
* [论坛](https://community.mycroft.ai)
* [博客](https://mycroft.ai/blog)
