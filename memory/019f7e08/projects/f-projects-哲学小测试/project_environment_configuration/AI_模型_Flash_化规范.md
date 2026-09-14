---
title: "AI 模型 Flash 化规范"
usage_scenario:
    - "配置或修改后端 AI 服务时指定 flash 模型"
    - "审查代码中是否存在过时的 Pro 模型引用"
keywords:
    - "AI 模型"
    - "deepseek-v4-flash"
    - "禁用 Pro"
---

后端 AI 模型统一使用 deepseek-v4-flash，禁止使用 Pro 系列模型（如 deepseek-v4-pro），所有相关代码需替换为 flash 版本
