---
title: "Agent分类逻辑重构:回退与显式留人工机制"
usage_scenario:
    - "重构基于LLM的决策Agent流程"
    - "修复AI因置信度低而错误终止的任务"
    - "设计支持自我纠错的多轮对话Agent"
keywords:
    - "Agent重构"
    - "reset_level"
    - "manual指令"
    - "confidence_low"
---

## 任务描述
重构图书编目 Agent 的分类决策逻辑，解决原逻辑中 `confidence=low` 被错误地直接触发"留人工"以及无法回退重钻的问题。

## 执行过程
```mermaid
graph TD
    A[需求:修复Agent分类逻辑] --> B[分析agent.py与agent_system.md现状]
    B --> C[发现decide_level只能下钻不能回退]
    C --> D[新增reset_level动作:允许AI走错分支时回退到指定层级]
    D --> E[新增manual动作:作为唯一合法的显式留人工出口]
    E --> F[修改final校验逻辑:low置信度不再直接交卷,而是打回自纠]
    F --> G[更新提示词:明确low的含义及自查路线(回退/取证/manual)]
    G --> H[编写Mock测试验证四种场景:回退/打回/连续low/manual]
    H --> I[清理临时文件并确认无语法错误]
```

## 任务总结
成功实现 Agent 智能纠错机制：
1. **新增 `reset_level`**：AI 可主动回退分类层级重新探索，避免将错就错。
2. **新增 `manual`**：只有当 AI 确认无法处理时才显式请求留人工，取代了原来的隐式 low 交卷。
3. **Low 置信度语义变更**：`confidence=low` 的 final 会被程序打回，要求 AI 自查（回退或补证），连续 3 次 low 才强制留人工。
4. **实测通过**：Mock 测试覆盖了走错回退、打回修正、连续 low 留人工、显式 manual 留人工四种场景。
