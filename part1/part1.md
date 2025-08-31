
# Plugin Basics

1. Default plugins.
2. Lazy Extras are not enabled by default.
3. Third-party plugins that LazyVim has no awareness of.

Any `lua` files inside the `lua/plugins` subdirectory will automatically be loaded.
For example, to disable bufferline.nvim

```lua
return {
  {"akinsho/bufferline.nvim, enabled = false"}brew install markdownlint-cli2
}
```

The first argument in Lua table is a string containing the github repo(with owner).

**File Management**

1. Snacks explorer(Neo-tree)
2. Mini.files
3. Oil.nvim, a vim-like explorer

**Lazy Extras**

1. Hit `x` to install and uninstall plugins.

# Useful commands

1. Lazy Extras mode

```zsh  
: LazyExtras
```

2. Return to the dashboard

```zsh
: lua Snacks.dashboard()
```

# Keybindings

Three places to configure keybindings, depending on how any one plugin is configure:

1. `.config/nvim/lua/config/keymaps.lua`. keybindings that are not specific to plugins.
2. `keys` filed of Lua table(in Lua, a "table" is like a combination of an array and a dict)passed to a plugin.
3. `opts` argument passed into a plugin's configuration.

An example showing how to modify the keybindings to open different dictionaries.

```lua
return {
  "echasnovski/mini.files",
  keys = {
    {
      "<leader>e",
      function()
        require("mini.files").open(vim.api.nvim_buf_get_name(0), true)
      end,
      desc = "Open mini.files (directory of current file)",
    },
    {
      "<leader>E",
      function()
        require("mini.files").open(vim.uv.cwd(), true)
      end,
      desc = "Open mini.files (cwd)",
    },
    {
      "<leader>fm",
      function()
        require("mini.files").open(LazyVim.root(), true)
      end,
      desc = "Open mini.files (root)",
    },
  },
}
```
