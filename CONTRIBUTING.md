# 贡献指南

## 开发原则

- Python + React 是当前主线；暂不扩展 `java_backend/`。
- 保持前端渲染、Python 领域逻辑、共享 fixture 和 PPTX 导出的行为一致。
- 新接口先更新后端 schema，再运行 `make gen-api` 更新前端类型。
- 不提交 `.env`、密钥、用户上传文件、虚拟环境或构建产物。

## 提交前检查

```powershell
cd backend; uv run ruff check .; uv run pytest
cd ../frontend; npm run build
```

涉及布局、文本度量或导出的改动，还应运行对应的 Flex parity 和回归脚本。

## 提交说明

提交信息使用简短、明确的动词开头，例如 `fix: prevent slide overflow` 或 `feat: add table block editing`。一个提交尽量只解决一个主题。
