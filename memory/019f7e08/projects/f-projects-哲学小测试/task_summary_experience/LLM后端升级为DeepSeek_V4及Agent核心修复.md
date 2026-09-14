---
title: "LLM后端升级为DeepSeek V4及Agent核心修复"
usage_scenario:
    - "为大模型应用增加新的API提供商支持（如接入新模型）"
    - "重构本地服务与云端API的统一路由架构"
    - "修复Agent工具调用签名不匹配或上下文溢出导致的崩溃"
keywords:
    - "DeepSeek V4"
    - "LLM路由"
    - "Agent修复"
    - "上下文压缩"
---

## 任务描述
根据最新 API 文档将 LLM 后端从 Ollama 扩展支持 DeepSeek V4 系列（含视觉模型），并修复 Agent 核心缺陷。

## 执行过程
```mermaid
graph TD
    A[需求:升级至DeepSeek V4并支持多Provider] --> B[WebSearch调研V4/Flash/Vision模型详情]
    B --> C[创建deepseek_client.py实现OpenAI兼容接口]
    C --> D[创建llm_client.py作为统一路由层]
    D --> E[修改config.yaml添加deepseek配置项]
    E --> F[批量替换6个业务模块的import为llm_client]
    F --> G[修复Agent核心Bug:工具签名/协议双读/上下文压缩]
    G --> H[修复Merger/Standard/Scanner目录排除逻辑]
    H --> I[实现网络错误分层与指数退避机制]
    I --> J[语法检查与连通性验证]
```

## 任务总结
成功实现 LLM 路由切换功能，默认使用 deepseek-v4-flash，图片自动切换 vision-exp。修复了 Agent 工具调用 TypeError、JSON 解析兼容性、上下文超限崩溃等严重问题，增强了系统的稳定性和容错能力。
