# VPS 自建服务一键部署

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/clashhub-net/vps-self-host.svg?style=flat-square)](https://github.com/clashhub-net/vps-self-host/stargazers)

> VPS 自建服务一键部署脚本 - Docker、LNMP、Nextcloud、WordPress、私人网盘、密码管理器
> 
> 从零开始搭建自己的私有服务

**中文** | **[English](README_EN.md)**

---

## 目录

- [Docker 一键安装](#docker-一键安装)
- [Web 服务器部署](#web-服务器部署)
- [私人网盘搭建](#私人网盘搭建)
- [密码管理器](#密码管理器)
- [其他实用服务](#其他实用服务)

---

## Docker 一键安装

### 安装 Docker

```bash
# 官方脚本
curl -fsSL https://get.docker.com | bash

# 国内镜像加速
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

# 启动服务
systemctl start docker
systemctl enable docker
```

### 安装 Docker Compose

```bash
# 下载最新版本
curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 添加执行权限
chmod +x /usr/local/bin/docker-compose

# 验证安装
docker-compose --version
```

### Docker 加速配置

```bash
# 创建配置文件
mkdir -p /etc/docker
cat > /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ]
}
EOF

# 重启 Docker
systemctl daemon-reload
systemctl restart docker
```

---

## Web 服务器部署

### LNMP 一键安装

```bash
# LNMP 一键包
wget http://soft.vpser.net/lnmp/lnmp2.0.tar.gz -cO lnmp2.0.tar.gz
tar zxf lnmp2.0.tar.gz
cd lnmp2.0
./install.sh lnmp
```

### 宝塔面板

```bash
# Ubuntu/Debian
wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh

# CentOS
yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh
```

### 1Panel

```bash
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && bash quick_start.sh
```

---

## 私人网盘搭建

### Nextcloud

```bash
# Docker 部署
docker run -d \
  --name nextcloud \
  -p 8080:80 \
  -v nextcloud:/var/www/html \
  --restart=always \
  nextcloud

# 访问 http://your-ip:8080
```

### Docker Compose 部署

```yaml
version: '3'
services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: nextcloud
      MYSQL_DATABASE: nextcloud
    volumes:
      - db:/var/lib/mysql

  app:
    image: nextcloud
    restart: always
    ports:
      - 8080:80
    environment:
      MYSQL_HOST: db
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: root
      MYSQL_PASSWORD: nextcloud
    volumes:
      - nextcloud:/var/www/html
    depends_on:
      - db

volumes:
  db:
  nextcloud:
```

### Alist 网盘列表

```bash
# 一键安装
curl -fsSL "https://alist.nn.ci/v3/install.sh" | bash -s install

# 启动
alist start

# 获取密码
alist admin set NEW_PASSWORD
```

---

## 密码管理器

### Bitwarden

```bash
# Vaultwarden (轻量版)
docker run -d \
  --name vaultwarden \
  -p 80:80 \
  -v vaultwarden:/data \
  --restart=always \
  vaultwarden/server:latest
```

### Docker Compose

```yaml
version: '3'
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    environment:
      - SIGNUPS_ALLOWED=true
    volumes:
      - ./data:/data
    ports:
      - "80:80"
```

---

## 其他实用服务

### RSS 阅读器 - FreshRSS

```bash
docker run -d \
  --name freshrss \
  -p 8080:80 \
  -v freshrss:/var/www/FreshRSS/data \
  --restart=always \
  freshrss/freshrss
```

### 书签管理 - Linkding

```bash
docker run -d \
  --name linkding \
  -p 9090:9090 \
  -v linkding:/etc/linkding/data \
  --restart=always \
  sissbruecker/linkding:latest
```

### 笔记服务 - Memos

```bash
docker run -d \
  --name memos \
  -p 5230:5230 \
  -v ~/.memos:/var/opt/memos \
  --restart=always \
  neosmemo/memos:latest
```

### 短链接服务 - Yourls

```bash
docker run -d \
  --name yourls \
  -p 80:80 \
  -e YOURLS_DB_PASS=password \
  -e YOURLS_SITE=https://your-domain.com \
  -e YOURLS_USER=admin \
  -e YOURLS_PASS=password \
  --restart=always \
  yourls
```

### 图床服务 - Lsky Pro

```bash
docker run -d \
  --name lsky-pro \
  -p 8089:80 \
  -v lsky:/var/www/html \
  --restart=always \
  halcyonazure/lsky-pro:latest
```

---

## 推荐资源

| 名称 | 说明 | 链接 |
|------|------|------|
| **VPSVIP** | VPS 主机测评与推荐 | [官网](https://vpsvip.net) |
| **Docker Hub** | 镜像仓库 | [官网](https://hub.docker.com) |

---

## License

MIT License - 2026

<p align="center">
  <a href="https://vpsvip.net">VPSVIP 主机测评</a>
</p>