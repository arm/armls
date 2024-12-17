# Arm Language Server

A language server that provides language support for AArch64 assembly.

## Features

- Hover support for instructions and operands
- Semantic highlighting
- Completion sourced from mnemonics, labels, set directives and macros
- Real-time diagnostics with customizable configuration and support for `clang`
- Document symbols (go to definition, document symbols, etc)

## Usage

### Vscode extension

Follow [vscode-armls](https://github.com/Arm/vscode-armls) readme.

### Neovim using `lspconfig`

<details open>
<summary>Simple setup</summary>

```lua
local lspconfig = require("lspconfig")
local configs = require("lspconfig.configs")

configs.armls = {
    default_config = {
        cmd = { "<path-to-armls-exe>" },
        root_dir = lspconfig.util.root_pattern(".git"),
        filetypes = { "asm" },
    },
}

configs.armls.setup({})
```

</details>

<details>
<summary>(default) Granular internal diagnostics configuration</summary>

```lua
configs.armls.setup({
    settings = {
        armls = {
            diagnostics = {
                enable = true,
                disableCategories = {
                    -- "unrecognisedInstruction",
                    "invalidOperand",
                    "tooManyOperands",
                    "tooFewOperands",
                },
        },
    },
})
```

</details>

<details>
    <summary>(default) External diagnostics configuration</summary>

```lua
configs.armls.setup({
    settings = {
        armls = {
            -- externalDiagnostics = {
            --     clang = {
            --         path = "/path/to/clang-executable",
            --         args = { "--target=aarch64" },
            --     },
            -- },
        },
    },
})
```

</details>
