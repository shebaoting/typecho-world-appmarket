# Typecho World AppMarket

Typecho World AppMarket 是 Typecho World 的应用中心插件，用于在后台浏览、搜索、查看和安装 Typecho 主题、插件与语言包。

插件默认连接 Typecho World 生态市场：

```text
https://typecho.world/api/v1/app-market/index.json
```

## 功能

- 浏览 Typecho World 生态市场中的主题、插件和语言包。
- 按类型、安装状态和关键词筛选应用。
- 查看作品详情、README、版本、兼容性和仓库信息。
- 在线下载 GitHub tag zip 包并安装到对应目录。
- 安装前展示应用名称、类型、版本、安装位置和兼容性。
- 安装时自动备份已有目录，失败后回滚。
- 安装完成后提供启用插件、预览/启用外观、启用语言包等后续入口。
- 支持本地缓存市场索引，网络异常时可显示缓存数据。

## 安装

下载最新发布包后，将插件目录放到 Typecho 的插件目录中：

```text
usr/plugins/AppMarket
```

目录结构应类似：

```text
usr/plugins/AppMarket/Plugin.php
usr/plugins/AppMarket/panel.php
usr/plugins/AppMarket/src/Market.php
usr/plugins/AppMarket/src/Repository.php
usr/plugins/AppMarket/assets/market.css
usr/plugins/AppMarket/assets/market.js
```

然后进入 Typecho 后台：

```text
应用管理 -> 插件 -> 应用市场 -> 启用
```

启用后，应用管理标题旁会出现“市场”入口。

## 环境要求

- Typecho World 1.3.10 或更高版本。
- PHP 8.3 或更高版本。
- 需要 `curl` 扩展用于连接市场和下载应用包。
- 需要 `ZipArchive` 扩展用于安装远程 zip 包。
- `usr/plugins`、`usr/themes`、`usr/langs` 需要具备对应写入权限。

## 安装位置

应用中心会按类型安装到固定位置：

| 类型 | 安装位置 |
| --- | --- |
| 插件 | `usr/plugins/{slug}` |
| 外观 | `usr/themes/{slug}` |
| 语言包 | `usr/langs/{locale}.mo` |

其中 `{slug}` 或 `{locale}` 来自市场索引、作品 manifest 或仓库信息。

## 安全策略

插件安装远程应用时会执行以下校验：

- 仅允许从 `typecho.world`、`github.com`、`codeload.github.com` 下载。
- 下载包大小限制为 50 MB。
- 解压前检查 zip 内路径，拒绝绝对路径、上级目录跳转和 Windows 盘符路径。
- 复制目录时拒绝符号链接。
- 插件包必须包含 `Plugin.php`。
- 外观包必须包含 `index.php`。
- 语言包必须包含 `.mo` 文件。
- 更新已有应用前会先备份，安装失败时尝试恢复原文件。

## 市场数据

市场索引由 Typecho World 官网生成。公开作品需要先在官网提交 GitHub 仓库，经审核后进入市场。

作品详情页：

```text
https://typecho.world/ecosystem/
```

提交作品：

```text
https://typecho.world/ecosystem/submit/
```

## 开发

本仓库是独立维护的 Typecho 插件仓库。开发时可以将仓库克隆到 Typecho 项目的插件目录：

```bash
cd usr/plugins
git clone git@github.com:shebaoting/typecho-world-appmarket.git AppMarket
```

修改后建议检查 PHP 语法：

```bash
php -l Plugin.php
php -l panel.php
php -l src/Market.php
php -l src/Repository.php
```

## 发布

发布新版本时需要同步更新 `Plugin.php` 中的版本号：

- `@version`
- `Plugin::VERSION`

然后提交并打 tag：

```bash
git add .
git commit -m "发布应用中心插件 vX.Y.Z"
git tag -a vX.Y.Z -m "发布应用中心插件 vX.Y.Z"
git push origin main
git push origin vX.Y.Z
```

Typecho World 官网会使用发布 tag 生成下载链接。

## 许可证

本插件随 Typecho World 项目长期维护。使用前请确认你的站点和二次开发场景符合对应项目的授权要求。
