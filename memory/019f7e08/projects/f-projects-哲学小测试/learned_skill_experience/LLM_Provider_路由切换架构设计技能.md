---
title: "LLM Provider 路由切换架构设计技能"
usage_scenario:
    - "需要更换本地或云端 LLM 服务且不想改动业务逻辑"
    - "集成支持 OpenAI 兼容格式的第三方 API（如 DeepSeek/Claude）"
keywords:
    - "LLM路由"
    - "Provider切换"
    - "OpenAI兼容"
    - "配置化"
---

## 输入
- 现有 Ollama 客户端代码
- 目标 LLM API 文档（如 DeepSeek OpenAI 兼容格式）

## 步骤
1. **新建 LLM 客户端**：参照 ollama_client.py 的 chat() 签名创建新客户端（如 deepseek_client.py），处理鉴权、超时、重试及异常统一抛出。
2. **创建路由层**：新建 llm_client.py，根据 config.yaml 的 `llm.provider` 字段动态导入并调用对应客户端，统一导出 chat/OllamaError。
3. **全局 Import 替换**：扫描项目所有引用 ollama_client 的文件，将 import 语句替换为 from src.llm_client import chat, OllamaError。
4. **配置文件扩展**：在 config.yaml 中新增 provider 开关及对应 LLM 的配置节（如 api_key/model/vision_model）。

## 输出
- 业务代码零改动即可通过修改配置文件切换底层 LLM 服务。

## 注意事项
- 确保新客户端的 chat() 参数签名与旧客户端完全一致（prompt, images, expect_json, system, history）。
- 异常类型需统一（如均使用 OllamaError），避免各模块 except 块失效。
