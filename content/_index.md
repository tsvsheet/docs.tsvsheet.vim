---
title: tsvsheet.vim
---

**tsvsheet.vim** brings [tsvsheet](https://tsvsheet.com) `.tsvt` spreadsheets to Vim and Neovim: filetype detection, buffer settings that keep the TAB-separated grid intact, and [tsvsheet-lsp](https://github.com/tsvsheet/tsvsheet.lsp) wiring for live diagnostics and hover — through Neovim's built-in LSP client on 0.11+, or [vim-lsp](https://github.com/prabirshrestha/vim-lsp) on classic Vim.

- Source: [tsvsheet/tsvsheet.vim](https://github.com/tsvsheet/tsvsheet.vim)
- Server: [tsvsheet/tsvsheet.lsp](https://github.com/tsvsheet/tsvsheet.lsp)
- Language: [tsvsheet/tsvsheet](https://github.com/tsvsheet/tsvsheet)

## Install the plugin

- [lazy.nvim](https://github.com/folke/lazy.nvim): `{ 'tsvsheet/tsvsheet.vim' }`
- [vim-plug](https://github.com/junegunn/vim-plug): `Plug 'tsvsheet/tsvsheet.vim'`
- Native packages: clone into `pack/*/start/` and run `:helptags ALL`

No configuration is needed: the plugin registers the `tsvsheet` filetype for `*.tsvt`, keeps real tabs (`noexpandtab`) in sheet buffers, and enables the language server itself wherever it can run.

## Install the server binary

The plugin launches `tsvsheet-lsp` from your `$PATH`. Install it either:

- **With [mason.nvim](https://github.com/mason-org/mason.nvim)** (Neovim), from the [tsvsheet mason registry](https://github.com/tsvsheet/mason-registry) — add it to mason's `registries`, then `:MasonInstall tsvsheet-lsp`:

  ```lua
  require('mason').setup({
    registries = {
      'github:tsvsheet/mason-registry',
      'github:mason-org/mason-registry',
    },
  })
  ```

- **From a [release](https://github.com/tsvsheet/tsvsheet.lsp/releases)** — download the tarball for your platform and put the binary on `$PATH`.
- **From source** — `go install github.com/tsvsheet/tsvsheet.lsp/cmd/tsvsheet-lsp@latest`.

## What you get

- **Diagnostics** — formula syntax errors and `check` findings, mapped to the offending cells as you edit.
- **Hover** — a cell's computed value, its formula, and resolved inputs.
- **Code actions** — fill a formula from the neighbouring cell (the gesture a GUI spreadsheet spells Ctrl+D / Ctrl+R), with references rebased by the engine.

Capabilities live in the server, so new ones arrive with a `tsvsheet-lsp` upgrade — the plugin needs no change.

## Troubleshooting

- **No diagnostics or hover** — confirm `tsvsheet-lsp` is on the editor's `$PATH`; GUI editors often see a shorter `$PATH` than your shell. The plugin stays silently inert when the binary is missing.
- **Everything flagged on one line** — the file likely uses spaces instead of real TABs between cells; tsvsheet cells are TAB-separated.
- **Server logs** — the server logs to stderr, which the client captures (`:help lsp-log` in Neovim); it accepts `--log-level debug` for verbose traces.

Also in the plugin: `:help tsvsheet` ships the same reference offline.
