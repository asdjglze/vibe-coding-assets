---
title: "Agent分类地图功能实现与Prompt规范化"
usage_scenario:
    - "为LLM Agent设计分层导航或地图功能"
    - "重构Agent System Prompt以去除冗余元话语"
    - "基于JSON数据动态生成结构化提示词"
keywords:
    - "分类地图"
    - "System Prompt"
    - "元话语清理"
    - "动态生成"
---

## 任务描述
为图书编目Agent增加‘分类地图’功能，优化System Prompt结构，并清理其中的开发者元话语。

## 执行过程
```mermaid
graph TD
    A[需求:增加分类地图] --> B[读取clc_full.json分析B/T类二级结构]
    B --> C[在agent.py实现_map_section动态生成地图文本]
    C --> D[扩展guide工具支持map参数(list/类字母)]
    D --> E[更新agent_system.md加入地图使用说明]
    E --> F[用户反馈Prompt格式混乱且含元话语]
    F --> G[重写agent_system.md恢复标准Markdown排版]
    G --> H[删除'仿开源agent/repo-map'等开发者视角描述]
    H --> I[运行校验脚本确认关键内容完整且无残留]
```

## 任务总结
实现了三层分类架构：1) System Prompt常驻22大类印象层；2) guide map按需获取第二层布局（如B类的地域/分支划分）；3) 现有get_children细节层。同时确立了Prompt编写铁律：严禁写入设计动机、出处比喻（如repo-map）等开发者元话语，只保留AI的操作指令。
