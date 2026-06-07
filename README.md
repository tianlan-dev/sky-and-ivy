# sky-and-ivy
(づ￣ 3￣)づ

## 运行

首次运行先安装依赖：

```bash
npm install
```

本地启动配置放在两个 Git 忽略的环境文件里：

- `.env.production.local`: production Docker 启动配置。
- `.env.development.local`: local development 启动配置。

两个文件都需要定义 `APP_HOST` 和 `APP_PORT`。

### Production in Docker

先创建本地 production 环境文件。这个文件会被 Git 忽略：

```bash
cat > .env.production.local <<'EOF'
APP_HOST=0.0.0.0
APP_PORT=<production-port>
EOF
```

然后用 Docker 构建并启动 production container：

```bash
./production-docker.sh
```

### Local Development

先创建本地 development 环境文件。这个文件会被 Git 忽略：

```bash
cat > .env.development.local <<'EOF'
APP_HOST=0.0.0.0
APP_PORT=<local-dev-port>
EOF
```

```bash
./development-server.sh
```

development 启动脚本会读取 `.env.development.local`，停止占用同一端口的旧进程，然后在后台运行 `npm run start`。具体监听地址和端口由本地环境文件提供。

## Package scripts

```bash
npm run start
```

- `npm run start`: 从仓库根目录启动静态文件服务器。需要通过环境变量提供 `APP_PORT`，可选提供 `APP_HOST`。

## Startup scripts

- `./production-docker.sh [start|stop|restart]`: 读取 `.env.production.local`，构建 Docker image，并替换同名 production container。
- `./development-server.sh [start|stop|restart]`: 读取 `.env.development.local`，停止占用 development 端口的旧进程，然后在后台运行 `npm run start`。

脚本日志写入 `.server-logs/`，该目录不会提交到 Git。
