# ArmLS (preview)

ArmLS is a language server for Arm Assembly that offers a collection of modern code editor features such as in-editor diagnostics and on-hover documentation. It has builtin knowledge of the latest version of the architecture (v9.6) and implements the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/), making it compatible with many [code editors](https://langserver.org/#implementations-client).

## Key features

ArmLS currently implements the following LSP functionality.


### Hover

ArmLS can provide information about instruction mnemonics and their operands on hover.



### Diagnostics

ArmLS offers real-time diagnostics support, either through its own internal diagnostics engine or through error mapping from a specified `clang` binary.


### Completion

ArmLS provides completion suggestions for items like mnemonics, macros, and labels based on the cursor position.


### Document symbols and goto definition

ArmLS detects and provides a list of useful symbols to your editor, and supports jumping to definitions.


### Semantic highlighting

You can configure ArmLS to provide semantic highlighting to clients that support it,
providing batteries-included syntax highlighting that can intelligently differentiate between symbol types.


## Usage

ArmLS should be compatible with any LSP-compliant client, but has been explicitly tested in the following use cases.


### VS Code

ArmLS has a VS Code extension ([vscode-armls](https://github.com/arm/vscode-armls)) that you can download from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/vscode) or the [Open VSX Registry](https://open-vsx.org/).


### Neovim

#### Configuration

<details open>
<summary>Simple setup (default config)</summary>

```lua
local config = {
    cmd = { "<path-to-armls-binary>" },
    filetypes = { "asm" },
    settings = {
        armls = {
            diagnostics = {
                enable = true,
                disableCategories = {
                    "invalidOperand",
                    "tooManyOperands",
                    "tooFewOperands",
                },
            },
        },
    },
}
vim.lsp.config("armls", config)
vim.lsp.enable("armls")
```

</details>

<details>
<summary>Granular internal diagnostics configuration</summary>

```lua
local config = {
    settings = {
        armls = {
            diagnostics = {
                enable = true,
                disableCategories = {
                    -- "unrecognisedInstruction",
                    -- "invalidOperand",
                    -- "tooManyOperands",
                    -- "tooFewOperands",
                },
            },
        },
    },
}
```

</details>

<details>
<summary>External diagnostics configuration</summary>

```lua
local config = {
    settings = {
        armls = {
            externalDiagnostics = {
                clang = {
                    path = "/path/to/clang-executable",
                    args = { "--target=aarch64" },
                    -- Or "--target=arm" if using A32.
                },
            },
        },
    },
}
```

</details>

<details>
<summary>A32 support</summary>

```lua
local config = {
    settings = {
        armls = {
        	defaultArchitecture = "A32",
        },
    },
}
```

</details>

#### Semantic highlighting

To disable semantic highlighting in Neovim, [delete the `semanticTokensProvider` property in the `armls` language server configuration](https://lsp-zero.netlify.app/docs/language-server-configuration.html#disable-semantic-highlights).

## Compatibility

ArmLS has been tested against the following versions of each major operating system:

- MacOS Sequoia
- Ubuntu 24.04
- Windows 11


## Current limitations

ArmLS is in preview and has the current limitations:

- ArmLS does not support cross-file references
- ArmLS does not support C/C++ pre-processor syntax
- The internal diagnostics engine is alpha quality and might display false positives. We recommend that you use `clang`-powered diagnostics at this time.
- A32 only supports [Unified Assembler Language][ual] ([`.syntax unified`][gcc_ual])


## Contact

To report bugs or request enhancements, use GitHub [issues](https://github.com/arm/armls/issues).

## Related resources

- [Learn more about AArch64](https://developer.arm.com/documentation/102374/0102/Overview)

[ual]: https://developer.arm.com/documentation/101754/0624/armasm-Legacy-Assembler-Reference/Writing-A32-T32-Instructions-in-armasm-Syntax-Assembly-Language/About-the-Unified-Assembler-Language
[gcc_ual]: https://sourceware.org/binutils/docs-2.45/as/ARM_002dInstruction_002dSet.html
