# 发布指南（**不需要 npm 账号**）：GitHub 仓库 + Release tarball + 市场投稿

这个目录已经是一个完整、可直接发布的插件包（`dsh-jupyterlab@0.1.0`），本地 git 仓库也初始化并提交好了。
**插件市场不要求发 npm**（npm 只影响市场里是否显示下载量），所以整条路线只有 GitHub。

> 你的 GitHub：用户名 **NKUhan123**（`zhanghan2813` 是 Name 显示名，不是 username）。
> 包里所有地址已按 `NKUhan123` 填好。

---

## 1. 本地先试装一遍（强烈建议，1 分钟）

先卸掉原来那个（两者注册同一套 `nb_*` 工具）：

```powershell
dsh plugin --profile desktop remove @beihaizb/dsh-notebook
```

再从本目录装：

```powershell
dsh plugin --profile desktop add file:E:\Deepseek\dsh-jupyterlab
```

重启 DSH → 会话顶部出现 Notebook 标签页，确认分栏 / 快捷键 / 剪刀 / py-md 都正常。
出了任何问题卸载即可，不影响别的：

```powershell
dsh plugin --profile desktop remove dsh-jupyterlab
```

> 如果 CLI 不接受目录形式，用预构建 tarball：`dsh plugin --profile desktop add file:E:\Deepseek\dsh-jupyterlab\dsh-jupyterlab.tgz`

## 2. 在 GitHub 上建仓库并推上去

网页上 **New repository**：

- 名字：`dsh-jupyterlab`
- 可见性：**Public**
- **不要**勾 “Add a README / .gitignore / license”（勾了会和本地提交冲突，push 时还得 rebase）

建完后：

```powershell
cd E:\Deepseek\dsh-jupyterlab
git remote add origin https://github.com/NKUhan123/dsh-jupyterlab.git
git push -u origin main
```

然后在仓库页面 **About → Topics** 里加 **`dsh-plugin`**（收录硬性要求）。

> 如果 git 报 `detected dubious ownership`（E: 盘不记录所有权）：
> `git config --global --add safe.directory E:/Deepseek/dsh-jupyterlab`

## 3. 发一个 Release，把预构建 tarball 挂上去（代替 npm）

Releases → **Draft a new release**：

- Tag：`v0.1.0`（新建 tag）
- 附件：上传 `E:\Deepseek\dsh-jupyterlab\dsh-jupyterlab.tgz`
- 正文随便写两句功能即可

⚠️ **资产文件名必须是 `dsh-jupyterlab.tgz`，不要带版本号。**
条目里写的是 `releases/latest/download/dsh-jupyterlab.tgz`：`latest/download` 只在请求时解析 `latest`，文件名是照字面取的——带版本号的话，你下次发版这个链接就 404。

市场会给用户提供这个预构建 tarball（而不是让他们从源码装），体验和 npm 包一样，且本包没有任何构建脚本，装的时候不会弹 pnpm 的 build 授权。

## 4. 投稿到插件市场（一个文件就是一个投稿）

1. Fork <https://github.com/awesome-dsh-plugin/awesome-dsh-plugin>
2. 把 `market/NKUhan123__dsh-jupyterlab.yml` 复制到那边的 `data/plugins/` 下
3. 提 PR，标题 `Add NKUhan123/dsh-jupyterlab`

条目内容（已校验为合法 YAML）：

```yaml
url: https://github.com/NKUHan123/dsh-jupyterlab
name: NKUHan123/dsh-jupyterlab
category: ui
tarball: https://github.com/NKUHan123/dsh-jupyterlab/releases/latest/download/dsh-jupyterlab.tgz
description:
  en: 'JupyterLab-style notebook for DSH: code and output side by side with a draggable divider, outputs that stick to the viewport while a long cell scrolls, Jupyter keyboard shortcuts (a/b/dd, m/y, Shift+Enter vs Ctrl+Enter), a scissors button to delete a cell and a chip to switch it between code and Markdown. Modified fork of @beihaizb/dsh-notebook.'
  zh: 'DSH 的 JupyterLab 风格 notebook：代码与输出左右分栏并可拖动，长 cell 滚动时输出贴住视口，支持 Jupyter 快捷键（a/b/dd、m/y、Shift+Enter 与 Ctrl+Enter 区分）、左侧剪刀删除 cell、py/md 标签切换代码与 Markdown。基于 @beihaizb/dsh-notebook 的修改版。'
```

审核要求（官方 contributing.md）对照：

| 要求 | 状态 |
|---|---|
| `package.json` 声明 `dsh.bundle`（只声明 `dsh.client` 会被拒） | ✅ `dsh.bundle.patch: ./cordis.patch.yml` |
| 仓库里有 `cordis.patch.yml` | ✅ |
| 仓库创建满 1 天 | ⏳ **刚建的仓库当天会被 CI 挡住，第二天自动通过，不用重新提交** |
| 仓库加 `dsh-plugin` topic | ⏳ 你来加 |
| 描述与代码一致、不夸大 | ✅ 每条功能都真实存在 |
| 一个 PR 最多 3 条 | ✅ 只加 1 条 |
| 不是纯聚合包 | ✅ 自带全部实现，不是依赖清单 |

合并后网站自动重建，dsh-market 里就能搜到并一键安装。

## 5. 以后更新版本

```powershell
# 改代码 → 改 package.json 的 version（例如 0.1.1）
cd E:\Deepseek\dsh-jupyterlab
git commit -am "0.1.1: ..."
git tag v0.1.1
git push --tags
# 再发一个 Release，资产名继续用 dsh-jupyterlab.tgz
```

市场条目不用改（它指向仓库和 latest tarball，不指向具体版本）。

## 关于 npm（以后想发再说）

- 市场**不要求** npm：不发只是市场卡片上没有下载量数字。
- 哪天真想发：注册 npm 后 `npm login && npm publish`（命令在包里就是这一条，包已预构建，不会再编译）。
- 仓库 `package.json` 的 `repository` 已指向本仓库，将来发 npm 时市场会自动把两者关联（无需在条目里写 `npm:`，写了反而会被校验拒绝）。
