# 3x-ui 汉化版（3xui-cn）

基于 [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) 的汉化分支：SSH 下的 `x-ui` 管理菜单全部中文，安装、更新后仍保持中文。

## 一键安装

```bash
bash <(curl -Ls https://raw.githubusercontent.com/RebelEcho/3xui-cn/main/install.sh)
```

装完直接运行 `x-ui` 即为中文菜单。

## 说明

- 菜单脚本 `x-ui.sh` 已汉化（主菜单 28 项、日志/防火墙/SSL/IP 限制/SSH 端口转发/PostgreSQL 等子菜单与全部提示信息）。
- `install.sh`、`x-ui.sh`、`update.sh` 都已改为从本仓库获取菜单脚本，因此**升级面板、更新菜单后依然是中文**。
- 面板本体（Go 程序与网页前端）来自官方发布包，未做修改；网页界面语言可在面板设置里选择简体中文。
- 功能逻辑与官方一致，仅翻译用户可见文本；grep 匹配串、URL、配置写入等内容保持原样。
- 需要还原官方版时，执行官方安装脚本即可覆盖。
