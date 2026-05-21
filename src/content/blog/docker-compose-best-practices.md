---
title: "Docker Compose 微服务编排最佳实践"
description: "使用 Docker Compose 编排多服务应用，涵盖网络配置、数据持久化、健康检查、环境管理等生产级实践。"
pubDate: 2026-05-10
tags: [Docker, 微服务, DevOps, 容器化]
category: DevOps
---

## 为什么选择 Docker Compose？

对于中小规模的项目，Docker Compose 是最简洁的容器编排方案：

- 单文件定义多服务
- 一条命令启动/停止所有服务
- 支持服务依赖和网络隔离
- 开发与生产环境一致性

## 完整的 docker-compose.yml

```yaml
version: '3.8'

services:
  # 前端
  web:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - API_URL=http://api:8000
    depends_on:
      api:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - frontend

  # API 服务
  api:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/app
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: unless-stopped
    networks:
      - frontend
      - backend

  # 数据库
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: app
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - backend

  # 缓存
  cache:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    networks:
      - backend

volumes:
  postgres_data:
  redis_data:

networks:
  frontend:
  backend:
    internal: true  # 后端网络不对外暴露
```

## 关键实践

### 1. 健康检查

使用 `healthcheck` 确保依赖服务真正可用：

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 40s
```

### 2. 网络隔离

- `frontend` 网络：对外暴露的服务
- `backend` 网络（internal: true）：数据库等不对外暴露

### 3. 数据持久化

使用 named volumes 而非 bind mounts：

```yaml
volumes:
  postgres_data:  # Docker 管理，数据不会丢失
```

## 开发技巧

```bash
# 只重建有变更的服务
docker compose up -d --build api

# 查看实时日志
docker compose logs -f api

# 进入容器调试
docker compose exec api sh

# 清理所有资源
docker compose down -v --rmi all
```

> Docker Compose 适合开发和中小规模部署，大规模生产环境考虑 Kubernetes。
