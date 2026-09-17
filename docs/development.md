# 本地开发指南

## 初始化

```powershell
docker compose up -d
cd backend; uv sync; uv run alembic upgrade head
cd ../frontend; npm install
```

复制 `backend/.env.example` 为 `backend/.env`，填入模型密钥。`.env` 已被忽略，不要提交。

## 启动命令

```powershell
# API
cd backend; uv run uvicorn app.main:app --reload --host 127.0.0.1 --port 39800

# worker
cd backend; uv run arq app.worker.settings.WorkerSettings

# frontend
cd frontend; npm run dev
```

也可以使用根目录 Makefile：`make up`、`make migrate`、`make dev-api`、`make dev-worker`、`make dev-web`。

## 数据库迁移

应用已有迁移时运行 `uv run alembic upgrade head`。创建迁移使用：

```powershell
cd backend
uv run alembic revision --autogenerate -m "describe change"
```

审阅生成文件后再执行升级；不要修改已应用的历史迁移。

## API 类型

前端类型由 FastAPI OpenAPI 生成：

```powershell
make gen-api
```

生成的 `frontend/openapi.json` 已被忽略，类型输出为 `frontend/src/api/schema.d.ts`。

## 停止与清理

开发结束可执行 `docker compose down`。如需连同数据库卷一起清理，请确认不再需要本地数据后再使用 `docker compose down -v`。
