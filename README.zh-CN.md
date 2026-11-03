# dsh-jupyterlab

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

**DSH 的 JupyterLab 风格 notebook 插件。** 一个真正持久的内核（`ipykernel`），agent 可以读写、执行、检查；前端按 JupyterLab 的习惯来：代码在左、图在右、Jupyter 快捷键、Markdown cell。

> **本插件是 [`@beihaizb/dsh-notebook`](https://github.com/beihzb/dsh-notebook)（作者 beihzb，MIT 协议）的修改版。**
> 内核 sidecar、`nb_*` 工具、`.ipynb` 读写、cell 界面都来自上游；本包在此之上加了下面这些功能，并把上游源码一起内置，装一个插件即可。

## 相比上游新增

1. **左右分栏**：每个 code cell 左栏是代码、右栏是它的输出（图、stdout、DataFrame、traceback）。中间竖线可拖动调整宽度并记住比例；工具栏 `⇔ 分栏 / ⇕ 单栏` 可切回原来的上下布局。
2. **输出贴底跟随**：右栏输出对齐 cell 底部，并跟随视口底部滚动——长 cell 滚到哪，刚画出来的图都在眼前。
3. **Jupyter 快捷键**：`Esc` 退出编辑进入命令模式后，`a` / `b` 上下插入、`dd` 删除、`Enter` 编辑、`↑` `↓`（或 `k` `j`）切换、`m` / `y` 切换 Markdown / 代码、`Ctrl+Enter` `Shift+Enter` `Alt+Enter` 运行；编辑时 `Tab` 接受补全。`?` 看快捷键表。
4. **cell 类型与一键删除**：左侧 `✂` 删除该 cell；`py` / `md` 小标签显示并可切换类型。Markdown cell 支持 `##` 标题、双击编辑、可折叠。
5. **工具栏 `＋MD`**：在下方插入 Markdown cell（原来的 `+ Cell` 现在会自动选中并进入编辑）。
6. **左侧文件栏（JupyterLab 风格）**：把当前工作目录下已保存的 `.ipynb` 列在左边，点一下就能打开；工具栏 `🗂 文件` 控制显示/隐藏。
7. **右侧看图面板**：点输出的图，或点输出里打印的图片路径（比如 `Saved: ...\fig.tif`），图就在**右侧可调宽度的面板**里打开（适应窗口 / 1:1 / 全屏 / 关闭），不用再盖住整个 notebook；`.tif` / `.tiff` 会用内核的 Pillow 转成 PNG 并缓存，保存的 TIFF 也能直接看。

其余功能（持久内核、agent 工具、`.ipynb` 存取、内核选择、tqdm 进度条、traceback 跳转、长输出折叠、大小核提示、中英双语界面）与上游一致。

## 安装

从 GitHub 仓库装（市场用的是这条）：

```bash
dsh plugin --profile desktop add github:NKUHan123/dsh-jupyterlab
```

或者用最新 GitHub Release 上的预构建 tarball（安装时不用构建）：

```bash
dsh plugin --profile desktop add file:<path-to>/dsh-jupyterlab.tgz  # prebuilt tarball from the latest GitHub Release
```

重启 DSH 后，会话顶部出现 **Notebook** 标签页。包里已带编译好的 `lib/` 和 Python driver，安装时不需要构建（没有 `prepare`/安装脚本，pnpm 不会要求授权 build）。

> **不要和 `@beihaizb/dsh-notebook` 同时安装**：两者注册同一套 `nb_*` 工具。本插件是超集，请先卸载原插件：
> ```bash
> dsh plugin --profile desktop remove @beihaizb/dsh-notebook
> ```

所选内核环境需要 `ipykernel`、`jupyter_client`、`nbformat`。

## 快捷键

| 模式 | 键 | 作用 |
|---|---|---|
| 命令 | `Enter` | 编辑选中 cell |
| 命令 | `Esc` | 回到命令模式 |
| 命令 | `a` / `b` | 上方 / 下方插入 |
| 命令 | `d` `d` | 删除选中 cell |
| 命令 | `↑` `↓` / `k` `j` | 上 / 下一个 cell |
| 命令 | `m` / `y` | 切 Markdown / 代码 |
| 命令 | `Ctrl+Enter` | 运行并停留 |
| 命令 | `Shift+Enter` | 运行并跳到下一个 |
| 命令 | `Alt+Enter` | 运行并在下方插入 |
| 命令 | `?` | 快捷键列表 |
| 编辑 | `Tab` | 接受补全（无补全时缩进） |
| 编辑 | `Esc` | 退出编辑 |

## 兼容性

DSH `0.1.x-rc` / `0.2.x-rc`（`@deepseek-ai/dsh-tools: ^0.1.0-rc.6 || ^0.2.0-rc.1 || ^0.2.1-rc.1`），在 DSH `0.2.1-rc.5` + Windows + conda `ipykernel` 上开发测试。

## 协议与署名

MIT。原始作品 Copyright (c) 2025-present **beihzb** — [`@beihaizb/dsh-notebook`](https://github.com/beihzb/dsh-notebook)；修改部分 Copyright (c) 2026 dsh-jupyterlab contributors。见 [LICENSE](./LICENSE)。
