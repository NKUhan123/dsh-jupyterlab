# dsh-jupyterlab

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

**A JupyterLab-style notebook for DeepSeek Harness.** A real, persistent `ipykernel` the agent can read, edit and run — with a front end that behaves like JupyterLab: code on the left, figures on the right, Jupyter keyboard shortcuts, and Markdown cells.

> **This is a modified fork of [`@beihaizb/dsh-notebook`](https://github.com/beihzb/dsh-notebook) by beihzb (MIT).**
> The kernel sidecar, the `nb_*` agent tools, `.ipynb` save/load and the cell UI are upstream work. This package keeps all of it and adds the features below, as a single self-contained plugin (the upstream sources are vendored, so `dsh plugin add` installs everything).

## What this fork adds

1. **Split view (code | output).** Every code cell renders as two columns: the editor on the left, its outputs — figures, stdout, DataFrames, tracebacks — on the right. A draggable divider resizes the columns and the ratio is remembered. The toolbar button `⇔ Split` / `⇕ Inline` returns to the classic stacked layout.
2. **Outputs stay in sight.** The output pane is bottom-aligned and sticks to the viewport while you scroll a long cell, so a figure you just rendered is already on screen instead of being stranded at the top of the cell.
3. **Jupyter keyboard shortcuts.** Press `Esc` to leave editing (command mode), then `a` / `b` insert a cell above / below, `dd` deletes, `Enter` edits, `↑` `↓` (or `k` `j`) move, `m` / `y` switch the cell to Markdown / Code, and `Ctrl+Enter` / `Shift+Enter` / `Alt+Enter` run. While editing, `Tab` accepts the completion popup (and still indents when none is open). `?` opens the shortcut list.
4. **Cell type and one-click delete.** A `✂` scissors in the gutter deletes the cell, and a `py` / `md` chip shows and switches the cell type. Markdown cells render `##` headings, are edited by double-click, and can be folded like code cells.
5. **`+ MD` in the toolbar** inserts a Markdown cell (the existing `+ Cell` now selects and focuses what it inserts).
6. **Left file bar (JupyterLab-style).** The saved `.ipynb` files in the working directory are listed down the left side; click one to open it. The toolbar `🗂 Files` button shows/hides the bar.
7. **Right figure viewer.** Click any output figure — or any image path printed in the output, such as `Saved: ...\fig.tif` — and it opens in a resizable panel on the right (fit / actual size / fullscreen / close) instead of a modal that covers the notebook. `.tif` / `.tiff` frames are converted to PNG with the kernel's Pillow and cached, so saved TIFFs preview too.

Everything else — persistent kernel, agent tools, `.ipynb` round-trip, kernel picker, tqdm progress bars, clickable tracebacks, long-output folding, hybrid-CPU hint, EN/中文 UI — is upstream and unchanged.

## Install

From the GitHub repository (this is what the plugin market uses):

```bash
dsh plugin --profile desktop add github:NKUHan123/dsh-jupyterlab
```

Or from the prebuilt tarball attached to the latest GitHub Release (no build step):

```bash
dsh plugin --profile desktop add file:<path-to>/dsh-jupyterlab.tgz  # prebuilt tarball from the latest GitHub Release
```

Then restart DeepSeek Harness. A **Notebook** tab appears in the session.

The package ships its compiled `lib/` and the Python driver, so there is nothing to build at install time (no `prepare` / install scripts — pnpm has no build to approve).

> **Don't install this next to `@beihaizb/dsh-notebook`.** Both register the same `nb_*` tools; this fork is a superset, so remove the original first:
> ```bash
> dsh plugin --profile desktop remove @beihaizb/dsh-notebook
> ```

The Python environment you pick as kernel needs `ipykernel`, `jupyter_client` and `nbformat`.

## Keyboard

| Mode | Key | Action |
|---|---|---|
| command | `Enter` | edit the selected cell |
| command | `Esc` | back to command mode |
| command | `a` / `b` | insert a cell above / below |
| command | `d` `d` | delete the selected cell |
| command | `↑` `↓` / `k` `j` | select previous / next cell |
| command | `m` / `y` | switch to Markdown / Code |
| command | `Ctrl+Enter` | run, stay on the cell |
| command | `Shift+Enter` | run, move to the next cell |
| command | `Alt+Enter` | run, insert a cell below |
| command | `?` | shortcut list |
| editing | `Ctrl+Enter` / `Shift+Enter` / `Alt+Enter` | same run modes |
| editing | `Tab` | accept completion (indent when nothing is open) |
| editing | `Esc` | leave editing |

## Tools (unchanged from upstream)

`nb_new` / `nb_open` / `nb_save` / `nb_get` / `nb_context` / `nb_list` / `nb_list_vars` / `nb_inspect_object` /
`nb_add_cell` / `nb_delete_cell` / `nb_move_cell` / `nb_edit_cell` / `nb_edit_and_run_cell` /
`nb_cell_history` / `nb_revert_cell` / `nb_error_context` /
`nb_run_cell` / `nb_run_all` / `nb_apply_suggestion` /
`nb_kernel_restart` / `nb_kernel_restart_and_clear` / `nb_kernel_interrupt` /
`nb_kernel_list` / `nb_kernel_select` / `nb_clear_outputs` / `nb_set_cwd`

`nb_add_cell` also accepts `before` (insert above a given cell) in this fork.

## Compatibility

DSH `0.1.x-rc` and `0.2.x-rc` (`@deepseek-ai/dsh-tools: ^0.1.0-rc.6 || ^0.2.0-rc.1 || ^0.2.1-rc.1`). Developed and tested against DSH `0.2.1-rc.5` on Windows with a conda `ipykernel`.

## License & attribution

MIT. Original work Copyright (c) 2025-present **beihzb** — [`@beihaizb/dsh-notebook`](https://github.com/beihzb/dsh-notebook); modifications Copyright (c) 2026 dsh-jupyterlab contributors. See [LICENSE](./LICENSE).
