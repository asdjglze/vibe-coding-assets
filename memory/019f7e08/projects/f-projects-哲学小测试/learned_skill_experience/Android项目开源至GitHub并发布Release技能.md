---
title: "Android项目开源至GitHub并发布Release技能"
usage_scenario:
    - "将本地 Android/iOS 项目开源到 GitHub"
    - "自动化构建并上传 APK/IPA 到 GitHub Releases"
    - "需要清理敏感信息和 IDE 残留文件的代码发布"
keywords:
    - "GitHub开源"
    - "Release发布"
    - "Android打包"
    - "API调用"
---

## 输入
- 待开源的项目目录路径
- 目标 GitHub 账号信息
- 仓库名称、许可证类型
- 需要发布的构建产物（如 APK）

## 步骤
1. **环境侦查**：检查 git 配置、GitHub 连通性（`git ls-remote`）、凭据可用性（`git credential fill`）。
2. **安全扫描**：使用 Grep 搜索敏感信息（IP、密钥、密码等），确保源码干净。
3. **清理与过滤**：
   - 编写 `.gitignore` 排除构建产物（build/、*.apk、local.properties 等）。
   - 识别并排除 IDE 冲突残留文件（如 `*_Conflict.*`）。
4. **准备开源素材**：生成 `README.md`（功能描述、构建指南）、`LICENSE`（MIT 等）。
5. **初始化仓库**：`git init`，设置 author/committer 为指定账号，`git add -A`，全量复核后提交初始版本。
6. **发布至 GitHub**：
   - 若 `gh` CLI 不可用，使用 Python `requests` 调用 GitHub REST API。
   - 通过 API 创建仓库（POST `/user/repos`）。
   - 使用 token 内嵌 URL 推送代码（`git push https://x-access-token:TOKEN@github.com/...`）。
   - 通过 API 创建 Release 并上传资产（POST `/repos/{owner}/{repo}/releases` + 上传端点）。
7. **终验**：匿名访问仓库确认内容正确，下载链接有效。

## 输出
- GitHub 公开仓库创建成功
- 代码已推送到 main 分支
- Release 中包含构建产物且可下载

## 注意事项
- **权限校验**：API 操作前务必验证 Token 的 scopes（需 repo 权限）及登录身份（login）。
- **沙箱限制**：在受限环境中，Python 子进程管道可能受限，优先使用普通模式或简化命令形态。
- **身份一致性**：Git 提交身份（author/committer）必须与 GitHub 账号一致，否则关联错误。
