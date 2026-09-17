# API 与工作流

API 前缀为 `/api/v1`，除注册、登录、健康检查、设计资源和媒体读取外，接口需要 `Authorization: Bearer <token>`。

## 认证

- `POST /auth/register`：注册，密码长度 8–128
- `POST /auth/login`：登录并返回 JWT
- `GET /auth/me`：当前用户

## 项目与大纲

- `POST /projects`：创建项目并提交来源
- `GET/PATCH/DELETE /projects/{project_id}`：项目管理
- `POST /projects/{project_id}/outline/generate`：生成大纲
- `PATCH /projects/{project_id}/outline`：修改大纲
- `POST .../outline/confirm`、`POST .../outline/unconfirm`：确认/取消确认
- `GET .../outline/events`：SSE 大纲进度

## Deck

- `GET /projects/{project_id}/deck`：读取 deck
- `POST .../deck/generate`：提交页面生成任务
- `GET .../deck/events`：SSE 页面进度
- `GET .../deck/quality`：质量检查
- `GET .../deck/export`：导出 PPTX
- `PUT .../slides/order`、`POST .../slides`、`DELETE .../slides/{slide_id}`：页面管理
- `PATCH .../slides/{slide_id}/blocks/{block_id}`：编辑 block
- `POST .../slides/{slide_id}/ai-edit`：生成 AI 编辑建议

完整请求/响应 schema 以运行中的 `/docs`（FastAPI Swagger UI）和自动生成的 `frontend/src/api/schema.d.ts` 为准。
