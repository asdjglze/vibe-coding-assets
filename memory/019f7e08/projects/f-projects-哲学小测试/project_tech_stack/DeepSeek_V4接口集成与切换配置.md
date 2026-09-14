---
title: "DeepSeek V4接口集成与切换配置"
usage_scenario:
    - "新增或替换大模型服务供应商时"
    - "调试不同 LLM 提供商的兼容性问题时"
keywords:
    - "DeepSeek"
    - "LLM路由"
    - "模型切换"
---

项目使用 DeepSeek API (V4模型) 作为 LLM 服务后端，通过配置文件切换 provider（如 deepseek/ollama），支持文本及视觉模型调用
