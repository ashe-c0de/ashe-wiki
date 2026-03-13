## 环境安装
Windows 安装 rustup  
Windows 安装 `MSVC v14x - VS 20xx C++ x64/x86 build tools` & `Windows 10/11 SDK` (通过[build tools](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools))  
scoop 安装 node.js、tree-sitter、ripgrep  
Windows 安装 neovim

## 验证环境
`rustc -V`
`cargo -V`

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/rust/verify-env.png)

## neovim 插件

- rustaceanvim
- treesitter
- cmp
- telescope.nvim
- crates.nvim
- neo-tree.nvim

> [我的neovim的init.lua](https://github.com/ashe-c0de/dotfiles-env/blob/main/neovim/init.lua)

## 打开rust项目

在powershell中进入rust项目，键入`nvim .`即可像IDE一样开发rust项目了。
