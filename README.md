# Linux 一键安装 Clash

![GitHub License](https://img.shields.io/github/license/nelvko/clash-for-linux-install)
![GitHub top language](https://img.shields.io/github/languages/top/nelvko/clash-for-linux-install)
![GitHub Repo stars](https://img.shields.io/github/stars/nelvko/clash-for-linux-install)

![preview](resources/preview.png)

## ✨ 功能特性

- 支持一键安装 `mihomo` 与 `clash` 代理内核。
- 兼容 `root` 与普通用户环境。
- 适配主流 `Linux` 发行版，并兼容 `AutoDL` 等容器化环境。
- 自动检测端口占用情况，在冲突时随机分配可用端口。
- 自动识别系统架构与初始化系统，下载匹配的内核与依赖，并生成对应的服务管理配置。
- 在需要时调用 [subconverter](https://github.com/tindy2013/subconverter) 进行本地订阅转换。

## 🚀 一键安装

在终端中执行以下命令即可完成安装：

```bash
git clone --branch master --depth 1 https://gh-proxy.org/https://github.com/nelvko/clash-for-linux-install.git \
  && cd clash-for-linux-install \
  && bash install.sh
```

- 上述命令使用了[加速前缀](https://gh-proxy.org/)，如失效可更换其他[可用链接](https://ghproxy.link/)。
- 可通过 `.env` 文件或脚本参数自定义安装选项。

## ⌨️ 命令一览

```bash
Usage: 
  clashctl COMMAND [OPTIONS]

Commands:
    on                    开启代理
    off                   关闭代理
    status                内核状况
    proxy                 系统代理
    ui                    Web 面板
    secret                Web 密钥
    sub                   订阅管理
    upgrade               升级内核
    tun                   Tun 模式
    mixin                 Mixin 配置

Global Options:
    -h, --help            显示帮助信息
```

💡`clashon` 同 `clashctl on`，`Tab` 补全更方便！

### 优雅启停

```bash
$ clashon
😼 已开启代理环境

$ clashoff
😼 已关闭代理环境
```
- 在启停代理内核的同时，同步设置系统代理。
- 亦可通过 `clashproxy` 单独控制系统代理。

### Web 控制台

```bash
$ clashui
╔═══════════════════════════════════════════════╗
║                😼 Web 控制台                  ║
║═══════════════════════════════════════════════║
║                                               ║
║     🔓 注意放行端口：9090                      ║
║     🏠 内网：http://192.168.0.1:9090/ui       ║
║     🌏 公网：http://8.8.8.8:9090/ui          ║
║     ☁️ 公共：http://board.zash.run.place      ║
║                                               ║
╚═══════════════════════════════════════════════╝

$ clashsecret mysecret
😼 密钥更新成功，已重启生效

$ clashsecret
😼 当前密钥：mysecret
```

- 可通过浏览器打开 `Web` 控制台进行可视化操作，例如切换节点、查看日志等。
- 默认使用 [zashboard](https://github.com/Zephyruso/zashboard) 作为控制台前端，如需更换可自行配置。
- 若需将控制台暴露到公网，建议定期更换访问密钥，或通过 `SSH` 端口转发方式进行安全访问。

### `Mixin` 配置

```bash
$ clashmixin
😼 查看 Mixin 配置

$ clashmixin -e
😼 编辑 Mixin 配置

$ clashmixin -c
😼 查看原始订阅配置

$ clashmixin -r
😼 查看运行时配置
```

- 通过 `Mixin` 自定义的配置内容会与原始订阅进行深度合并，且 `Mixin` 具有最高优先级，最终生成内核启动时加载的运行时配置。
- `Mixin` 支持以前置、后置或覆盖的方式，对原始订阅中的规则、节点及策略组进行新增或修改。

### 升级内核
```bash
$ clashupgrade
😼 请求内核升级...
{"status":"ok"}
😼 内核升级成功
```
- 升级过程由代理内核自动完成；如需查看详细的升级日志，可添加 `-v` 参数。
- 建议通过 `clashmixin` 为 `github` 配置代理规则，以避免因网络问题导致请求失败。

### 管理订阅

```bash
$ clashsub -h
Usage: 
  clashsub COMMAND [OPTIONS]

Commands:
  add <url>       添加订阅
  ls              查看订阅
  del <id>        删除订阅
  use <id>        使用订阅
  update [id]     更新订阅
  log             订阅日志


Options:
  update:
    --auto        配置自动更新
    --convert     使用订阅转换
```

- 支持添加本地订阅，例如：`file:///root/clashctl/resources/config.yaml`
- 当订阅链接解析失败或包含特殊字符时，请使用引号包裹以避免被错误解析。
- 自动更新任务可通过 `crontab -e` 进行修改和管理。

### `Tun` 模式

```bash
$ clashtun
😾 Tun 状态：关闭

$ clashtun on
😼 Tun 模式已开启
```

- 作用：实现本机及 `Docker` 等容器的所有流量路由到 `clash` 代理、DNS 劫持等。
- 原理：[clash-verge-rev](https://www.clashverge.dev/guide/term.html#tun)、 [clash.wiki](https://clash.wiki/premium/tun-device.html)。
- 注意事项：[#100](https://github.com/nelvko/clash-for-linux-install/issues/100#issuecomment-2782680205)

## 🗑️ 卸载

```bash
bash uninstall.sh
```

## 📖 常见问题


[项目地址](https://github.com/nelvko/clash-for-linux-install)：
👉 https://github.com/nelvko/clash-for-linux-install
# 一、问题背景
`clash-for-linux-install `是一个非常方便的一键脚本项目，可以在 Linux 服务器上快速部署 Clash，并提供 Web UI（如 yacd）进行节点切换与管理。

但在实际使用过程中，这个项目有一个容易卡住新用户的地方：

>安装过程中需要输入一个「订阅链接」
且这个订阅链接的要求是：
👉 通过 HTTP GET 请求直接返回 Clash 可用的 YAML 配置文件

而问题在于：

 - 大多数机场提供的订阅链接是 token 形式的通用订阅
 - 该链接返回的内容通常是 base64 编码的 vmess / ss / trojan 信息
 - 并不是 Clash 原生可直接使用的 YAML

因此，直接把机场订阅链接填入脚本时，往往会提示 订阅无效。
# 二、解决思路概述
核心思路只有一句话：
把机场订阅“预先转换”为 Clash YAML，然后再“伪装成一个订阅链接”提供给脚本使用
整体流程如下：

 1. 在本地电脑使用 Clash for Windows 将机场订阅转换为 YAML 
 2. 将生成的 config.yaml 上传到服务器
 3. 在服务器上通过 Python HTTP 服务，把 config.yaml 暴露成一个本地订阅链接 
 4. 在clash-for-linux-install 安装过程中，输入这个“本地订阅链接”

这样可以 完全绕开订阅格式不兼容的问题，同时也不依赖外部转换服务。

# 三、具体操作步骤

创建一个文件夹clash_cfg,放我的config.yaml
接着用服务器 HTTP 服务把 config.yaml 伪装成订阅链接
```bash
python3 -m http.server 18080 --bind 127.0.0.1
```
接着新开一个shell，一键安装项目
```bash
git clone --branch master --depth 1 https://gh-proxy.org/https://github.com/nelvko/clash-for-linux-install.git \
  && cd clash-for-linux-install \
  && bash install.sh
```
---
## 然后填入订阅地址：http://127.0.0.1:18080/config.yaml



![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/983a758efd0c43fbb4c6c63b6f1a0e0f.png)
然后就可以去web端，`http://ip:9090/ui/` 选择节点了，
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8120fbf538e047388a3741bf392760ae.png)

![在这里插入图片描述](https://iblog.csdnimg.cn/direct/966a32229447462c9e0224224e578798.png)

# docker配置代理
1）给 Docker 服务创建代理配置
```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo nano /etc/systemd/system/docker.service.d/http-proxy.conf
```
##写入下面的内容
```bash
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,::1"
```
==crtl+o 保存，然后enter， ctrl + x退出==
---

2）重载并重启 Docker
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```
3）检查 Docker daemon 是否真的吃到了代理
```bash
systemctl show --property=Environment docker
```
