
# Plugin Basics

1. Default plugins.
2. Lazy Extras are not enabled by default.
3. Third-party plugins that LazyVim has no awareness of.

Any `lua` files inside the `lua/plugins` subdirectory will automatically be loaded.
For example, to disable bufferline.nvim

```lua
return {
  {"akinsho/bufferline.nvim, enabled = false"}
}
```

The first argument in Lua table is a string containing the github repo(with owner).

**Installing Third-party Plugins**

```lua
return {
  "nmac427/guess-indent.nvim",
  opts = {
    auto_cmd = true,
    override_editorconfig = true
  },
}
```

Create a new Lua file in the `plugins` directory. The first entry in the Lua table is the GitHub repo, with other
configuration(`opts` and `keys`, among others)after that name. `opts` table depends on the plugin, read README.

**Setting the plugins**:
Using `require()` to import plugin, then call `setup()` function with a Lua table as parameters.

```lua
-- example in the document of plugin
require("some-plugin").setup({
  -- options
  option1 = true,
  option2 = "value",
})
```

Lazy.nvim simplifies the setting procedure, just copy the parameters in `setup()` function to `opts`. Lazy.vim would execute `require("some-plugin").setup({...})`.

To install `chrisgrieser/nvim-spider`, which changes the `w`, `e`, and `b` commands to support navigating within CamelCase and snake_case words. This is the file named `nvim-spider.lua` in `plugins` directory:

```lua
return {
  "chrisgrieser/nvim-spider",
  opts = {},
  keys = {
    {
      "w",
      "<cmd>lua require('spider').motion('w')<CR>",
      mode = { "n", "o", "x" },
      desc = "Move to start of next of word",
    },
    {
      "e",
      "<cmd>lua require('spider').motion('e')<CR>",
      mode = { "n", "o", "x" },
      desc = "Move to end of word",
    },
    {
      "b",
      "<cmd>lua require('spider').motion('b')<CR>",
      mode = { "n", "o", "x" },
      desc = "Move to start of previous word",
    },
  },
}
```

The

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
2. `keys` filed of Lua table(in Lua, a "table" is like a combination of an array and a dict)passed to a plugin. `keys` would not be send to plugins. It is a filed of `Lazy.nvim` to manage keybindings. `Lazy.vim` would analyse the `keys` table and set the keybindings when starting Neovim. `keymaps.lua` would be treated as default keybindings, every `keys` table in `plugins` would be treated as local keybindings related to the plugin. `keys` has a higher priority than `keymaps.lua`.
3. `opts` argument passed into a plugin's configuration. `Lazy.nvim` would execute `require("plugin").setup(ops)` when importing plugins.

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

A Lua table wrapped in curly braces should be returned to satisfy the contract with Lazy.nvim.

Each item in the `keys` table is anther Lua table with three fields. The first two fields are positional and represent the keybinding name and the Lua callback function that gets called whenever that keybinding is invoked.

`<leader>` is an key used as the prefix for custom keybindings. In LazyVim, the leader is `<space>`. Special keys are
indicated to Vim's keybindings engine using angle brackets, such as, `<Space>`, `<Right>`.
