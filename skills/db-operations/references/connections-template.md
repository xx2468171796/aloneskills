# 数据库连接信息模板

在项目中使用此技能时，应创建 `connections.md` 文件记录实际连接信息。

## 连接信息模板

### PostgreSQL

```env
# 连接字符串
DATABASE_URL="postgres://[user]:[password]@[host]:[port]/[database]"

# 分解配置
DB_HOST=[host]
DB_PORT=[port]
DB_NAME=[database]
DB_USER=[user]
DB_PASSWORD=[password]
```

### Redis

```env
# 连接字符串
REDIS_URL="redis://:[password]@[host]:[port]"

# 分解配置
REDIS_HOST=[host]
REDIS_PORT=[port]
REDIS_PASSWORD=[password]
```

### MinIO / S3

```env
# 配置
MINIO_ENDPOINT=[host]
MINIO_PORT=[port]
MINIO_BUCKET=[bucket-name]
MINIO_ACCESS_KEY=[access-key]
MINIO_SECRET_KEY=[secret-key]
```

## 使用说明

1. 复制此模板到项目的 `skills/db-operations/references/connections.md`
2. 替换占位符为实际连接信息
3. 将 `connections.md` 添加到 `.gitignore` (包含敏感信息)

## 测试连接

### PostgreSQL

```bash
# 使用 psql 测试
psql "postgres://user:password@host:port/database"

# 或使用 Prisma
npx prisma db pull
```

### Redis

```bash
# 使用 redis-cli 测试
redis-cli -h [host] -p [port] -a [password] ping
```

### MinIO

```bash
# 使用 mc (MinIO Client) 测试
mc alias set myminio http://[host]:[port] [access-key] [secret-key]
mc ls myminio/[bucket]
```
