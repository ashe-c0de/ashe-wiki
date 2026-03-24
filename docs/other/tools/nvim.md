
> [我的neovim的init.lua](https://github.com/ashe-c0de/dotfiles-env/blob/main/neovim/init.lua)

## 常用快捷键

`nvim .` 打开当前文件夹（项目）

`a` 新建文件/文件夹（add）

`d` 删除（delete）

`r` 重命名（rename）

`y` 复制文件（copy to clipboard）

`p` 粘贴（paste）

`o` 打开文件或展开/收起文件夹

`v` 垂直分屏打开该文件

`dd` 删除当前行

`yy` 复制当前行

`yyp` 复制当前行至下一行

`yyP` 复制当前行至上一行

`ggVG` 全选（可加d即为全选删除/剪切）

`p` 粘贴

## move

`hjkl` left down up right

`a` 行首

`$` 行末

`ctrl + d` 向下翻页

`ctrl + u` 向上翻页

`gg` 跳跃文件顶部

`G` 跳跃文件底部

`gd` 跳跃至函数定义位置

`gr` 跳远至函数引用位置

## search

`/keyword` 向下搜索

`?keyword` 向上搜索

`n` next match

`N` previous match

## leader组合键(需配置init.lua)

`Space + e`   → 文件树

`Space + w`   → 保存

`Space + t`   → 唤出terminal，Insert模式下jk回到Normal模式 

## workspace工作区(project.nvim插件)

`Space + fp` 打开最近的项目

`C:\Users\<username>\AppData\Local\nvim-data\project_nvim\project_history` 使用记事本打开，可以选择删除缓存错误的project目录

## 模式切换

| 模式名称 | 进入方式 | 主要用途 |
|---|---|---|
| Normal | Esc 或 Ctrl+[ | 移动、删除、复制、跳转 |
| Insert | i, a, o 等 | 输入文本 |
| Visual | v, V, Ctrl+v | 选中块、批量操作 |
| Command | :, / | 执行命令、查找 |
| Replace | R | 覆盖现有字符 |
