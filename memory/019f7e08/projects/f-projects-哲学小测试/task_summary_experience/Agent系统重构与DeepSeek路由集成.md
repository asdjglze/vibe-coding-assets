---
title: "Agent系统重构与DeepSeek路由集成"
usage_scenario:
    - "为现有Agent系统接入新的LLM提供商（如DeepSeek/Claude）"
    - "重构Agent的上下文管理与错误重试机制"
    - "调整Agent的文件扫描与处理路径配置"
keywords:
    - "DeepSeek集成"
    - "Agent重构"
    - "上下文管理"
    - "LLM路由"
---

## 任务描述
重构 Agent 系统以解决上下文溢出、格式漂移等问题，并新增 DeepSeek API 支持（含视觉模型），同时调整扫描与存放路径。

## 执行过程
```mermaid
graph TD
    A[需求:修复Agent缺陷并集成DeepSeek] --> B[诊断现有问题:上下文全量重发/超时短/重试不分层]
    B --> C[调研业界方案:Trae压缩/InfoQ上下文管理原则]
    C --> D[设计LLM路由层:统一Ollama与DeepSeek接口]
    D --> E[实现deepseek_client.py:支持V4-Flash/Vision/思考模式]
    E --> F[更新config.yaml:添加DeepSeek配置/调整超时参数]
    F --> G[修改scanner.py:更新扫描与存放目录路径]
    G --> H[实施协议三重保险:提示词约束+双读兜底]
```

## 任务总结
1. **架构升级**：新建 `src/deepseek_client.py`，通过 OpenAI 兼容格式调用 DeepSeek V4-Flash 及 Vision 模型，支持自动切换思考模式与图片输入。
2. **上下文优化**：确立程序控制拼接策略，引入上下文压缩机制，避免服务端截断。
3. **稳定性增强**：细化错误分层重试（网络/工具/AI），增加超时时间至 30s/60s。
4. **配置更新**：在 `config.yaml` 中新增 DeepSeek 配置项，并在 `run_config.json` 中更新书籍扫描与存放路径。
