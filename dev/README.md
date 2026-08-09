# Contributor documentation

How [`tsvsheet/tsvsheet.vim`](https://github.com/tsvsheet/tsvsheet.vim) works inside and how to verify a change. User-facing documentation lives in [`content/`](../content/).

## Layout

The plugin is a thin LSP-client shim over standard Vim runtime directories: `ftdetect/` maps `*.tsvt` to the `tsvsheet` filetype, `ftplugin/` keeps real tabs in sheet buffers (with a complete `b:undo_ftplugin`), `lsp/tsvsheet.lua` is the Neovim 0.11+ native server config, and `plugin/tsvsheet.vim` enables it — `vim.lsp.enable` on Neovim, a `User lsp_setup` registration (guarded by `executable()`) for vim-lsp everywhere else. No language semantics live here; they belong to the server.

## Build and test

There is nothing to build. `make check` runs the whole gate:

- `make fmt-check` — stylua formatting for the Lua sources.
- `make test-nvim` — the headless Neovim harness (`test/nvim.lua`): filetype, tab settings and their undo, the LSP config contract, inertness without a binary, and — when `tsvsheet-lsp` is on `$PATH`, enforced in CI via `TSVSHEET_VIM_E2E=required` — a live attach and hover round trip.
- `make test-vim` — the headless classic-vim harness (`test/vim.vim`): the portable core plus the vim-lsp registration contract, executed both ways against a stub `autoload/lsp.vim`.

CI (`verify.yml`) runs all of it on every push against the latest released server, and weekly against the unchanged tree so upstream regressions surface.
