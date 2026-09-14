---
title: "AI提示词外部化存储规范"
usage_scenario:
    - "新增或修改AI Agent的任务指令"
    - "维护多轮对话或复杂推理的Prompt模板"
keywords:
    - "Prompt管理"
    - "提示词"
    - "外部化"
    - "Markdown"
---

所有用于驱动AI执行特定任务的提示词（Prompt），必须从业务代码中剥离，独立存储在外部Markdown文件中（如skills目录下的.md文件），由代码动态加载调用。
