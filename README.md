# AI PPT Generator

一个将主题、文字或文档转换为可编辑 PPTX 的全栈应用。当前主运行版本由 React/Vite 前端、FastAPI 后端和 ARQ worker 组成；`java_backend/` 暂不属于主开发与部署流程，并已在 `.gitignore` 中排除。

## 能做什么

- 从主题、粘贴文本或 PDF/Word/Markdown/TXT 生成演示文稿大纲
- 大纲确认后并发生成结构化幻灯片
- 在浏览器中编辑文字、图片、图表、表格和页面顺序
- 使用固定布局或树形 Flex 布局，并进行溢出与内容质量检查
- AI 局部改写、页面重排、图片替换
- 导出 PowerPoint 可继续编辑的 `.pptx`

## 快速开始

依赖：Windows/macOS/Linux、Python 3.12+、Node.js、Docker Desktop（用于 PostgreSQL 与 Redis）。

```powershell
docker compose up -d
cd backend
uv sync
uv run alembic upgrade head
uv run uvicorn app.main:app --host 127.0.0.1 --port 39800
```

另开两个终端：

```powershell
cd backend
uv run arq app.worker.settings.WorkerSettings
```

```powershell
cd frontend
npm install
npm run dev -- --host 127.0.0.1
```

浏览器访问 <http://127.0.0.1:39173/>。健康检查为 <http://127.0.0.1:39800/api/v1/health>。

## 文档

- [架构与数据流](docs/architecture.md)
- [本地开发指南](docs/development.md)
- [环境变量与模型配置](docs/configuration.md)
- [API 与工作流](docs/api.md)
- [故障排查](docs/troubleshooting.md)
- [贡献指南](CONTRIBUTING.md)

## 测试与构建

```powershell
cd backend; uv run ruff check .; uv run pytest
cd frontend; npm run build
```

完整回归集：`cd backend; uv run python scripts/run_regression.py`。

## 安全提示

不要提交 `backend/.env`、API Key、JWT secret、对象存储密钥或用户上传内容。生产环境必须替换默认 JWT secret 和数据库密码，并通过 HTTPS 暴露服务。
