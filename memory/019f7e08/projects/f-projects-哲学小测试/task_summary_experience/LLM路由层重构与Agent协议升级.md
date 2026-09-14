---
title: "LLM路由层重构与Agent协议升级"
usage_scenario:
    - "接入新的 LLM API（如 Claude/GPT）并实现本地/云端切换"
    - "修复 AI Agent 输出格式不稳定导致的解析失败问题"
    - "解决长对话场景下的上下文溢出或截断问题"
keywords:
    - "LLM路由"
    - "DeepSeek V4"
    - "Agent协议"
    - "上下文管理"
---

## 任务描述
重构 LLM 调用层以支持 DeepSeek V4 API 并与 Ollama 无缝切换，同时升级 Agent 协议以解决上下文溢出和格式漂移问题。

## 执行过程
```mermaid
graph TD
    A[需求: LLM 路由与 Agent 协议升级] --> B[调研 DeepSeek V4 API 最新特性]
    B --> C[创建 deepseek_client.py 实现 OpenAI 兼容接口]
    C --> D[创建 llm_client.py 统一入口按配置路由]
    D --> E[修改 config.yaml 添加 llm/deepseek 配置项]
    E --> F[全局替换 ollama_client 引用为 llm_client]
    F --> G[重写 agent.py _SYSTEM 提示词统一 args 格式]
    G --> H[修复工具函数签名适配 ctx 参数传递]
    H --> I[实现上下文分层拼接与自动压缩逻辑]
    I --> J[实施网络/工具/AI 错误分层降级策略]
```

## 任务总结
成功实现 LLM 提供商热切换（Ollama/DeepSeek），默认使用 DeepSeek V4 Flash 模型；Agent 协议升级为严格 JSON 格式（所有参数在 args 内），通过程序控制上下文拼接避免截断，并增强了错误处理的鲁棒性。
