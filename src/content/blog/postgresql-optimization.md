---
title: "PostgreSQL 性能优化：从慢查询到飞一般"
description: "系统梳理 PostgreSQL 数据库性能优化策略，涵盖索引设计、查询优化、连接池配置、分区表等实战经验。"
pubDate: 2026-05-15
tags: [PostgreSQL, 数据库, 性能优化, 后端]
category: 后端开发
---

## 性能优化的正确姿势

数据库性能优化不是盲目的"加索引"，而是系统性工程。

## 1. 诊断慢查询

首先，开启慢查询日志：

```sql
-- 记录超过 100ms 的查询
ALTER SYSTEM SET log_min_duration_statement = 100;
SELECT pg_reload_conf();
```

使用 `EXPLAIN ANALYZE` 分析执行计划：

```sql
EXPLAIN ANALYZE
SELECT u.name, COUNT(o.id) as order_count
FROM users u
JOIN orders o ON o.user_id = u.id
WHERE u.created_at > '2026-01-01'
GROUP BY u.name
ORDER BY order_count DESC;
```

重点关注：
- **Seq Scan**：全表扫描，通常需要优化
- **Nested Loop**：大数据量时性能差
- **Sort**：是否可以利用索引避免排序

## 2. 索引策略

### B-tree 索引（默认）

```sql
-- 单列索引
CREATE INDEX idx_users_email ON users(email);

-- 复合索引（注意列顺序）
CREATE INDEX idx_orders_user_date ON orders(user_id, created_at DESC);
```

### 部分索引

只索引需要的行，减少索引体积：

```sql
CREATE INDEX idx_active_users ON users(email)
WHERE is_active = true;
```

### 表达式索引

```sql
CREATE INDEX idx_users_lower_email ON users(LOWER(email));
```

## 3. 连接池配置

使用 PgBouncer 管理连接池：

```ini
; pgbouncer.ini
[databases]
mydb = host=127.0.0.1 port=5432 dbname=mydb

[pgbouncer]
pool_mode = transaction
max_client_conn = 1000
default_pool_size = 25
reserve_pool_size = 5
```

## 4. 分区表

大表按时间分区：

```sql
CREATE TABLE measurements (
  id BIGSERIAL,
  recorded_at TIMESTAMP NOT NULL,
  value FLOAT
) PARTITION BY RANGE (recorded_at);

-- 按月创建分区
CREATE TABLE measurements_2026_01 PARTITION OF measurements
  FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');

CREATE TABLE measurements_2026_02 PARTITION OF measurements
  FOR VALUES FROM ('2026-02-01') TO ('2026-03-01');
```

## 优化清单

- [ ] 开启慢查询日志
- [ ] 分析 TOP 10 慢查询
- [ ] 检查缺失索引
- [ ] 配置连接池
- [ ] 考虑分区策略
- [ ] 定期 VACUUM 和 ANALYZE

> 记住：优化的前提是测量，不要凭直觉优化。
