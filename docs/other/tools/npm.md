## 安装 npm

npm 随 Node.js 一并安装，前往 [Node.js 官网](https://nodejs.org/) 下载 LTS 版本安装包并安装即可。

### 检查安装是否成功

```
node -v
npm -v
```

## 配置网络

设置 npm 使用代理（如本机 Clash/V2Ray 端口）

```
npm config set proxy http://127.0.0.1:7897
npm config set https-proxy http://127.0.0.1:7897
```

设置国内镜像源（可选，提升安装速度）

```
npm config set registry https://registry.npmmirror.com
```

取消代理设置

```
npm config delete proxy
npm config delete https-proxy
```

## 常用管理命令

| 功能 | 命令示例 |
| :--- | :--- |
| 初始化项目 | `npm init` |
| 初始化默认配置 | `npm init -y` |
| 安装依赖 | `npm install <包名>` |
| 安装开发依赖 | `npm install <包名> -D` |
| 安装指定版本 | `npm install <包名>@<版本号>` |
| 卸载依赖 | `npm uninstall <包名>` |
| 查看已安装依赖 | `npm list` |
| 全局安装 | `npm install -g <包名>` |
| 全局卸载 | `npm uninstall -g <包名>` |
| 更新依赖 | `npm update <包名>` |
| 按 lock 文件安装 | `npm ci` |
| 运行脚本 | `npm run <脚本名>` |
| 查看当前配置 | `npm config list` |
| 设置配置项 | `npm config set <键> <值>` |
| 删除配置项 | `npm config delete <键>` |
