---
title: "Qoder Rules系统设计规范与配置"
usage_scenario:
    - "确认Qoder是否支持项目级规则约束"
    - "查找Qoder规则文件的存放位置与格式"
    - "排查Qoder AI行为不符合预期时的规则配置"
keywords:
    - "Qoder"
    - "Rules"
    - "规则系统"
    - "AGENTS.md"
---

Qoder IDE 支持项目级专属规则系统，存放于 `.qoder/rules` 目录，仅对当前项目生效。包含四种类型：始终生效、模型决策、指定文件生效、手动引入（@rule）。兼容 AGENTS.md 文件（冲突时规则优先）。所有活跃规则合计上限 100,000 字符，仅支持自然语言。（来源：WebFetch）
