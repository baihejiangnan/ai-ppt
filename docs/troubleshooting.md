# 故障排查

## 健康检查显示 database/redis 为 down

确认 Docker Desktop 已运行，然后执行：

```powershell
docker compose ps
docker compose up -d
```

等待 PostgreSQL 和 Redis 显示 `healthy`，再重试 `http://127.0.0.1:39800/api/v1/health`。

## 登录失败或无法注册

确认数据库迁移已执行：

```powershell
cd backend; uv run alembic upgrade head
```

邮箱会被规范化为小写；同一邮箱重复注册会返回 409。

## 生成任务一直等待

确认 worker 正在运行且能连接 Redis。worker 启动时应看到 `generate_outline` 和 `generate_deck`。检查 `LLM_API_KEY`、`LLM_BASE_URL`、`LLM_MODEL`，并查看 API/worker 日志。

## 模型返回错误

不同供应商的模型名称和结构化输出能力不同。先用供应商控制台确认模型名，再检查超时、余额和 API 兼容路径。

## 前端端口被占用

前端固定使用 `39173`，API 使用 `39800`。找到占用进程后停止它，或同步修改 `frontend/vite.config.ts`、API 配置和 CORS。

## 导出后文字溢出

先查看 deck 的 quality 接口结果；减少单页内容、降低内容密度或切换布局。不要只修改导出的 PPTX，因为源数据仍以结构化 slide blocks 为准。
