# 架构与数据流

## 组件

```text
浏览器 React/Vite :39173
        │ /api 代理
        ▼
FastAPI :39800 ─── PostgreSQL :39432
        │                    
        └──────── Redis :39379 ◄── ARQ worker
```

生产环境由 Nginx 托管前端静态文件并反代 API；API 与 worker 共享数据库、Redis、`shared/` 资源和环境配置。

## 生成链路

1. `projects` 接收主题、文本或上传来源，并将解析结果保存为来源小节。
2. `outlines` 工作流裁剪上下文后调用结构化 LLM，生成可编辑大纲。
3. 用户确认大纲后，worker 为每一页创建生成任务。
4. `slide` 工作流生成 `SlideDraft`，计算 Flex 宽高，执行结构/溢出/丰富度校验；可修复问题最多自动修复一轮。
5. 结构化 blocks 存入 PostgreSQL JSONB，同时被前端渲染器和 PPTX renderer 使用。
6. SSE 推送大纲和页面进度；导出接口生成可编辑 PPTX。

## 目录职责

- `backend/app/api`：HTTP/SSE 接口
- `backend/app/workflows`：LangGraph 工作流
- `backend/app/domain`：布局、内容、质量和校验等领域逻辑
- `backend/app/llm`：模型客户端和结构化输出
- `backend/app/render`：PPTX、图表、文字渲染
- `backend/app/worker`：ARQ 异步任务
- `frontend/src/features/deck`：编辑工作台
- `shared`：主题、固定布局、Flex 预设和回归 fixture
