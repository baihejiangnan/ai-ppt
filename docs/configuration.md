# 环境变量与模型配置

配置文件：`backend/.env`。模板见 `backend/.env.example`。

## 必要配置

```env
DATABASE_URL=postgresql+asyncpg://aippt:aippt@localhost:39432/aippt
REDIS_URL=redis://localhost:39379/0
LLM_API_KEY=...
LLM_BASE_URL=https://api.deepseek.com
LLM_MODEL=deepseek-flash
```

`LLM_BASE_URL` 使用 OpenAI 兼容接口；模型名称必须是供应商账户实际可用的名称。

## 常用可选配置

- `LLM_THINKING_ENABLED`：是否启用思考模式
- `LLM_TIMEOUT_SECONDS`：模型请求超时
- `SLIDE_CONCURRENCY`：单份 PPT 的并发页数
- `IMAGE_PROVIDER`：`openai` 或 `bailian`
- `IMAGE_API_KEY`、`IMAGE_BASE_URL`、`IMAGE_MODEL`：AI 生图
- `UNSPLASH_ACCESS_KEY`：图库降级来源
- `STORAGE_DRIVER`：`local` 或 `cos`
- `MAX_UPLOAD_MB`、`MAX_IMAGE_MB`：上传限制
- `JWT_SECRET`、`JWT_EXPIRE_MINUTES`：登录令牌配置

修改配置后必须重启 API 和 worker。不要把任何真实密钥放入 `.env.example`、日志或 Git。
