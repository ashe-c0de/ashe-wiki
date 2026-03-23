## windows安装scoop

### 1. 设置脚本执行策略
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

### 2. 下载并安装 Scoop
`Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')`

### 3. 检查安装是否成功
`scoop --version`

## 配置网络

设置 Scoop 全局代理

`scoop config proxy 127.0.0.1:7897`

## 常用管理命令

| 功能 | 命令示例 |
| :--- | :--- |
| 搜索软件 | `scoop search <软件名>` |
| 安装软件 | `scoop install <软件名>` |
| 安装指定版本 | `scoop install <软件名>@<版本号>` |
| 卸载软件 | `scoop uninstall <软件名>` |
| 查看已安装 | `scoop list` |
| 更新某软件 | `scoop update <软件名>` |
| 更新所有软件 | `scoop update *` |
| 清理旧版本/缓存 | `scoop cleanup *` |
| 查看当前配置 | `scoop config` |
| 取消代理设置 | `scoop config rm proxy` |

