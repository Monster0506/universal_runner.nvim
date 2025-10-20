#  universal_runner.nvim

**Run or debug the current file in Neovim - easily, flexibly, and per filetype.**  

A minimal, extensible plugin for Neovim that allows you to **run or debug the current buffer** using customizable runners. Each filetype can define its own run/debug commands (e.g., Python, Go, Rust), and the plugin automatically provides buffer-local keymaps or commands.

---

##  Features

-  Define per-filetype **run/debug commands**
-  Buffer-local **keymaps and user commands**
-  Optional automatic `makeprg` integration
-  Minimal configuration -- just works out of the box
-  Fully extensible for any language

---

##  Installation

Use your favorite plugin manager.

**Lazy.nvim:**

```lua
{
  "monster0506/universal_runner.nvim",
  config = function()
    require("universal_runner").setup()
  end,
}
```

**Packer:**

```lua
use({
  "monster0506/universal_runner.nvim",
  config = function()
    require("universal_runner").setup()
  end,
})
```

---

##  Setup

Basic setup:

```lua
require("universal_runner").setup()
```

Or customize behavior:

```lua
require("universal_runner").setup({
  split_cmd = "split",        -- "vsplit", "split", or "tabnew"
  use_quickfix = false,       -- reserved for future use
  enable_makeprg = true,      -- sets makeprg to the runner's command

  keymaps = {
    run = "<leader>x",
    debug = "<leader>d",
  },

  runners = {
    python = {
      filetypes = { "python" },
      run = "python3 %file%",
      debug = "python3 -m pdb %file%",
    },
  },
})
```


##  Keybindings

Default keymaps (per buffer):

| Action | Default | Description |
| :------ | :------- | :----------- |
| Run     | `<leader>rr` | Run current file |
| Debug   | `<leader>rd` | Debug current file |

You can override these via `setup()`:

```lua
keymaps = {
  run = "<leader>x",
  debug = "<leader>d",
}
```

---

##  Custom Runners

Define your own runners for any language:

```lua
require("universal_runner").setup({
  runners = {
    go = {
      filetypes = { "go" },
      run = "go run %file%",
      debug = "dlv exec %file%",
      keymaps = {
        run = "<leader>gr",
      },
      run_command = "GoRun",
    },
  },
})
```

**Runner fields:**

| Field | Description |
| :----- | :----------- |
| `filetypes` | Which filetypes trigger this runner |
| `run` | Command to run the file (`%file%` expands to buffer's full path) |
| `debug` | Command for debugging |
| `keymaps` | (Optional) Override global keymaps |
| `run_command` | (Optional) Creates `:GoRun` (or similar) command |

---

##  Behavior

When a buffer's filetype matches a runner:

- Buffer-local keymaps are created
- Optional user commands (like `:GoRun`) are registered
- `makeprg` is set automatically if enabled
- Running the file opens a terminal split (`vsplit` by default)
- The terminal waits after execution (press `<Enter>` to close)

---

##  Default Configuration

```lua
{
  keymaps = {
    run = "<leader>rr",
    debug = "<leader>rd",
  },
  split_cmd = "vsplit",
  use_quickfix = false,
  enable_makeprg = true,
  runners = {},
}
```

---

##  Notes

- `%file%`  expands to the full path of the current buffer
- Works across shells (PowerShell, Bash, etc.)
- Awaiting expansion of quickfix support

---

##  Author

**Monster0506**  
<https://github.com/monster0506>  
License: [MIT](LICENSE)

