# 3X-UI 汉化版

**3X-UI** 是一个开源的 Xray-core 网络面板，用于部署、配置和监控各类代理与 VPN 协议。本仓库是 [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) 的汉化分支：**SSH 下的 `x-ui` 管理菜单全部为中文**，并且安装、更新之后依然保持中文。

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/3x-ui-dark.png">
    <img alt="3x-ui" src="./media/3x-ui-light.png">
  </picture>
</p>

> [!IMPORTANT]
> 本项目仅供个人学习使用，请勿用于非法用途。

## 汉化说明

- `x-ui.sh`：SSH 管理菜单全中文（主菜单 28 项、日志管理、防火墙、SSL 证书、IP 限制、SSH 端口转发、PostgreSQL 等子菜单与全部提示信息）。
- `install.sh`：安装时优先使用本仓库的汉化脚本，装完直接是中文菜单。
- `update.sh`：面板升级后重新安装菜单脚本时，同样拉取汉化版，不会被官方英文脚本覆盖。
- 功能逻辑与官方完全一致，只翻译用户可见文本；grep 匹配串、URL、配置文件内容等保持不变。
- 面板网页界面（前端）未改动，登录后在面板「设置 → 语言」中选择简体中文即可。

## 一键安装

```bash
bash <(curl -Ls https://raw.githubusercontent.com/RebelEcho/3xui-cn/main/install.sh)
```

安装完成后执行 `x-ui` 即可看到中文管理菜单。

安装指定面板版本（例如 v3.7.0）：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/RebelEcho/3xui-cn/main/install.sh) v3.7.0
```

安装滚动更新的开发版（跟随上游 `main` 的最新构建，非稳定版）：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/RebelEcho/3xui-cn/main/install.sh) dev-latest
```

安装过程会自动生成随机的用户名、密码和访问路径。安装完成后运行 `x-ui` 打开管理菜单，可以启停服务、查看或重置登录信息、管理 SSL 证书等。

## 管理菜单能做什么

- **安装 / 更新 / 卸载**：安装面板、更新面板、更新到开发版、更新菜单脚本、安装历史版本、卸载
- **重置**：重置用户名和密码、重置网页根路径、重置全部设置、修改端口、查看当前设置
- **服务**：启动、停止、重启、重启 Xray、查看状态、日志管理
- **自启**：开启 / 关闭开机自启
- **证书**：SSL 证书管理（域名证书、IP 证书、Cloudflare 证书、强制续期、吊销）
- **防护**：IP 限制（Fail2ban）、防火墙管理（ufw）
- **网络**：SSH 端口转发管理、PostgreSQL 管理、开启 BBR、更新 Geo 文件、Ookla 测速

## 支持的平台

**操作系统**：Ubuntu、Debian、Armbian、Fedora、CentOS、RHEL、AlmaLinux、Rocky Linux、Oracle Linux、Amazon Linux、Virtuozzo、Arch、Manjaro、Parch、openSUSE（Tumbleweed / Leap）、Alpine、Windows。

**架构**：`amd64` · `386` · `arm64`（aarch64）· `armv7` · `armv6` · `armv5` · `s390x`。

## 数据库

安装时可选两种后端：

- **SQLite**（默认）：单文件 `/etc/x-ui/x-ui.db`，零配置，适合中小规模使用。
- **PostgreSQL**：适合客户端数量多或多节点部署。安装脚本可以本机安装 PostgreSQL，也可以填写已有服务器的 DSN。

从 SQLite 迁移到 PostgreSQL：

```bash
x-ui migrate-db --dsn "postgres://xui:***@127.0.0.1:5432/xui?sslmode=disable"
# 然后在 /etc/default/x-ui 中设置 XUI_DB_TYPE 和 XUI_DB_DSN，再重启：
systemctl restart x-ui
```

原 SQLite 文件不会被修改，确认新后端正常后可自行删除。

## Docker

默认 `docker compose up -d` 使用 SQLite。要用内置的 PostgreSQL 服务，取消 `docker-compose.yml` 里两行 `XUI_DB_*` 注释并按 profile 启动：

```bash
docker compose --profile postgres up -d
```

镜像内置 Fail2ban（默认启用）用于限制客户端 IP 数量。Fail2ban 用 `iptables` 封禁 IP，需要 `NET_ADMIN` 权限，`docker-compose.yml` 已通过 `cap_add` 授予；如果用 `docker run` 启动，请自行添加：

```bash
docker run -d --cap-add=NET_ADMIN --cap-add=NET_RAW ... ghcr.io/mhsanaei/3x-ui
```

## 常用环境变量

| 变量 | 说明 | 默认值 |
| --- | --- | --- |
| `XUI_DB_TYPE` | 数据库类型：`sqlite` 或 `postgres` | `sqlite` |
| `XUI_DB_DSN` | PostgreSQL 连接串（`XUI_DB_TYPE=postgres` 时使用） | — |
| `XUI_DB_FOLDER` | SQLite 数据库文件目录 | `/etc/x-ui` |
| `XUI_INIT_WEB_BASE_PATH` | 面板初始访问路径 | `/` |
| `XUI_ENABLE_FAIL2BAN` | 是否启用 Fail2ban 限制客户端 IP | `true` |
| `XUI_LOG_LEVEL` | 日志级别（`debug`/`info`/`warning`/`error`） | `info` |
| `XUI_DEBUG` | 调试模式 | `false` |

完整列表参见上游文档：[docs.sanaei.dev](https://docs.sanaei.dev)。

## 注意

- 本仓库的汉化菜单是静态副本，上游后续新增的菜单项不会自动变中文；如需跟进，重新拉取上游 `x-ui.sh` 汉化一次即可。
- 想还原为官方英文版本，直接执行官方安装脚本覆盖即可。
- 上游项目：[MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui)（GPL-3.0），感谢原作者及 [alireza0](https://github.com/alireza0/)。
