---
title: "Qoder Rules系统设计认知"
usage_scenario:
    - "在Qoder中配置项目级编码规范或提示词时"
    - "迁移其他IDE的规则到Qoder时参考其设计规范"
keywords:
    - "Qoder"
    - "Rules系统"
    - "AGENTS.md"
    - "提示词配置"
---

Qoder IDE支持Rules级别的提示词系统，设计如下：
- **存放位置**：项目根目录 `.qoder/rules/`，可随Git共享。
- **四种规则类型**：始终生效（注入所有会话）、模型决策（Agent自行判断）、指定文件生效（通配符匹配）、手动引入（@rule）。
- **AGENTS.md兼容**：根目录放置 `AGENTS.md` 可被自动识别，冲突时Rules优先。
- **限制与配置**：活跃规则上限10万字符，仅支持自然语言；通过设置界面或命令添加。
