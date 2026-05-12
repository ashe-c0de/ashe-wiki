# Docker 安装指南

## macOS

推荐方案：

- OrbStack（推荐，轻量、低内存占用）
- Docker CLI

### 安装 OrbStack

```bash
brew install --cask orbstack
```

启动：

```bash
orb start
```

验证：

```bash
docker version
docker ps
```

查看当前 Docker Context：

```bash
docker context ls
```

---

## Windows

推荐方案：

- Docker Desktop
- WSL2 Backend

### 安装 Docker Desktop

下载并安装 Docker Desktop：

https://www.docker.com/products/docker-desktop/

安装过程中勾选：

```text
Use WSL2 instead of Hyper-V
```

安装完成后验证：

```powershell
docker version
docker ps
```

---

## Linux

推荐方案：

- 原生 Docker Engine

### Ubuntu 安装 Docker

```bash
sudo apt update

sudo apt install -y \
    ca-certificates \
    curl \
    gnupg

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin
```

启动 Docker：

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

将当前用户加入 docker 组（避免每次 sudo）：

```bash
sudo usermod -aG docker $USER
```

重新登录终端后验证：

```bash
docker version
docker ps
```

---

# Redis 测试

拉取 Redis：

```bash
docker pull redis:7-alpine
```

启动 Redis：

```bash
docker run -d \
  --name redis \
  -p 6379:6379 \
  redis:7-alpine
```

查看容器：

```bash
docker ps
```
