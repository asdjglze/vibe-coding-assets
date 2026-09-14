---
title: "Qoder IDE规则加载与生效机制"
usage_scenario:
    - "排查规则未生效问题时检查是否为新会话"
    - "解释为何规则修改后当前对话仍显示旧版"
keywords:
    - "Qoder"
    - "IDE"
    - "规则加载"
    - "会话快照"
---

Qoder Desktop IDE 会加载用户级规则文件（always_on），注入到会话上下文中作为快照；修改规则后需开启新会话才能生效
