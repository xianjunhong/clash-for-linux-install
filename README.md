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

## 自建节点接入：xiancn.top（2026-10-15 更新）

以下记录适用于当前服务器安装的 `clash-for-linux-install` / `clashsub` 版本。上游项目已更名为 `clashctl`，新版命令参数可能不同；操作时以本机 `clashsub -h` 为准。

### 当前部署与验证结果

| 项目 | 当前配置 |
| --- | --- |
| Linux 客户端服务器 | `47.101.190.58` |
| 代理内核 | Mihomo `v1.19.17` |
| Web 管理面板 | zashboard：`http://47.101.190.58:9090/ui/` |
| 本机 HTTP 代理 | `http://127.0.0.1:7890` |
| 节点入口 | `xiancn.top:443` |
| 节点协议 | VLESS + WebSocket + TLS |
| TLS SNI / WebSocket Host | `xiancn.top` |
| WebSocket 路径 | `/blog` |
| 对应入站 | 搬瓦工 x-ui 的 `My-CDN-Node` |
| 本地配置源文件 | `/root/clashctl/resources/local/xiancn.yaml` |
| 已启用的订阅编号 | `2` |

当前连接链路：

```text
Linux 上的 Mihomo
  → xiancn.top:443（Cloudflare，TLS + WebSocket /blog）
  → 172.96.194.29:80（My-CDN-Node，WebSocket 回源）
  → 目标网站
```

已通过服务器的 `127.0.0.1:7890` 发起实际 HTTPS 请求，返回 HTTP 200，出口 IP 为 `172.96.194.29`。启用后保留了原有策略组选择、Rule 模式及 46 条运行时分流规则；不代表所有程序的流量都会自动进入代理。

### 节点入口与订阅地址的区别

| 内容 | 如何使用 |
| --- | --- |
| `xiancn.top` 或 `https://xiancn.top/blog` | 节点连接入口；需要与 UUID、TLS、WebSocket 等参数组合，不能直接当订阅下载地址。 |
| `vless://...` | 单节点分享链接；当前 `clashsub` 版本先尝试下载内容，不能把该链接直接当作 HTTP 订阅地址。可转换为 YAML 节点，或通过内核的节点集合接入。 |
| HTTP(S) 地址，返回完整 Clash/Mihomo YAML | 可用 `clashsub add` 添加。 |
| HTTP(S) 通用订阅，返回 URI 列表或 Base64 | 当前安装脚本会在配置验证失败后尝试本地转换，结果取决于转换器对协议和格式的支持。 |
| `file:///root/clashctl/resources/local/xiancn.yaml` | 从 Linux 服务器本地读取完整 YAML；本次采用此方式。 |

Windows 与 Linux 可以复用兼容内核的节点参数。导入 Windows 上生成的 YAML 本身没有问题，但整份配置里的本机路径、端口和 TUN 等选项仍需适合服务器环境。

Mihomo 的 `proxy-providers` 还支持 YAML、URI 列表和 Base64 格式的节点集合，这是内核能力，与安装脚本的整份配置导入流程不同，参见[代理集合内容说明](https://wiki.metacubex.one/config/proxy-providers/content/)。

### 添加并启用本地配置

本次已经将服务器上验证可用的完整配置保存到 `/root/clashctl/resources/local/xiancn.yaml`，保留了原有节点与分流设置。该文件含节点 UUID，不提交到 Git 仓库。

本地 `file:///` 文件不会经过订阅转换，必须已经是有效的 Clash/Mihomo YAML；不能只放一行 `vless://...` 分享链接。

以下命令在 Linux 服务器的 SSH 终端执行。添加操作已经完成，现有服务器无需重复添加：

```bash
clashsub add 'file:///root/clashctl/resources/local/xiancn.yaml'
clashsub ls
```

本次返回的新编号为 `2`，启用命令如下。其他安装环境应使用实际返回的编号：

```bash
clashsub use 2
```

`use` 会切换配置并重启 Mihomo，已有代理连接可能短暂中断。完成后刷新 9090 面板，在“🚀 手动选择”中选择该节点，或保留“♻️ 自动选择”（当前只有这一个候选节点）。

如果在新的服务器上部署，先将完整 YAML 上传并放到上述绝对路径，再运行添加命令；`file:///` 指向的是服务器文件，不是 Windows 文件。已有安装无需重新执行安装脚本。

若终端提示找不到 `clashsub`，在本次安装路径下可先加载命令：

```bash
source /root/clashctl/scripts/cmd/clashctl.sh
```

### 日常切换、更新与检查

切换回已添加的 xiancn 配置：

```bash
clashsub use 2
```

修改节点 UUID、域名或 WebSocket 路径后，先编辑 `/root/clashctl/resources/local/xiancn.yaml`，再执行：

```bash
clashsub update 2
```

该命令会重新读取本地源文件，更新订阅副本；如果编号 `2` 正在使用，还会重新加载配置。单独执行 `use 2` 只会加载已经导入的副本。该本地配置不会从 x-ui 自动获取变化，也不会生成远程订阅链接。

不要直接修改 `runtime.yaml`，它会由原始配置与 Mixin 合并生成。

检查服务状态及实际出口：

```bash
systemctl is-active mihomo
curl --noproxy '' --proxy http://127.0.0.1:7890 --max-time 20 https://www.cloudflare.com/cdn-cgi/trace
```

服务应显示 `active`。在当前分流和节点选择下，测试响应中的 `ip=` 应为 `172.96.194.29`；这里只验证经过本机代理端口的这次请求。

### 原 18080 方式与回退

之前的订阅编号 `1` 来源于 `http://127.0.0.1:18080/config.yaml`，由临时 Python HTTP 服务提供。该服务停止后，已导入的配置副本仍可使用，但更新订阅会连接失败。

现在直接用 `file:///` 读取稳定的源文件，不再需要启动 Python 18080 服务。旧订阅保留，如需回到此前的配置副本，可执行：

```bash
clashsub use 1
```

回退不会恢复 18080 服务，也不会修复旧订阅的更新来源。本次切换前的配置与订阅元数据另已备份到服务器 `/root/clashctl/backups/xiancn-20261015T150802Z/`。

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
