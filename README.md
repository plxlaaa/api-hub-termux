# API Hub Termux —— 手机上跑 AI 网关

把 [API Hub](https://github.com/plxlaaa/api-hub)（个人 API 聚合网关）装进 Android 手机的 Termux，
旧手机秒变 7×24 AI 网关服务器。

> 本仓库是**引导仓库**：主代码在 api-hub 主仓库，这里提供一键安装脚本和 Termux 专项文档。
> Termux 支持已进入主仓库 main 分支（`4d068c0` 起）。

## 一键安装

Termux（[F-Droid 版](https://f-droid.org/packages/com.termux/)）内执行：

```bash
pkg update && pkg upgrade
bash <(curl -fsSL https://raw.githubusercontent.com/plxlaaa/api-hub-termux/main/install-termux.sh)
```

脚本会：安装 Node.js → 克隆主仓 → npm install → 生产构建 → 自检，全程约 5-10 分钟。

## 环境要求

| 项 | 要求 |
|---|---|
| Termux | F-Droid 版（Play 版已停更） |
| Node.js | >= 22.5（安装脚本会自动装） |
| Android | arm64 / x86_64 均可 |
| 存储 | ~500MB |

## 原理一句话

Prisma 的 query engine 是 glibc 原生二进制，Android 跑不了；主仓库内置了
`node:sqlite` 纯绑定驱动适配器（零原生编译依赖），Android 上自动切换，
数据访问行为与 Prisma 完全一致（123 项单测双驱动各跑一遍保证）。

详见 [TERMUX.md](./TERMUX.md)（部署 / 后台保活 / 开机自启 / 排障）。

## 快速上手（装完后）

```bash
cd ~/api-hub
npm run start:node          # 启动
# 浏览器访问 http://localhost:3000 → 初始化管理员 → 添加渠道
termux-wake-lock            # 建议加唤醒锁防杀后台
```

## 文件说明

| 文件 | 用途 |
|---|---|
| `install-termux.sh` | 一键安装脚本（与主仓库同步） |
| `TERMUX.md` | 完整 Termux 部署文档（与主仓库同步） |
