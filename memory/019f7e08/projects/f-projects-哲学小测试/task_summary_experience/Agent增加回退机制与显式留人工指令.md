---
title: "Agent增加回退机制与显式留人工指令"
usage_scenario:
    - "修复LLM Agent流程中的死循环或错误分支无法恢复问题"
    - "规范Agent的人工介入触发条件与退出机制"
keywords:
    - "Agent"
    - "回退机制"
    - "流程控制"
    - "人工介入"
---

## 任务描述
修复图书编目 Agent 的流程控制缺陷：原逻辑中 Agent 走错分类分支无法回退，且将 confidence=low 错误地作为留人工出口，导致流程僵化。

## 执行过程
```mermaid
graph TD
    A[需求:修复Agent流程控制] --> B[分析缺陷:无回退指令/low误用]
    B --> C[新增reset_level动作:支持按层级回退重钻]
    C --> D[新增manual动作:作为唯一显式留人工出口]
    D --> E[修改主循环:low final一律打回自纠]
    E --> F[设定留人工阈值:连续3次low或显式manual才放行]
    F --> G[同步更新agent_system.md提示词协议]
    G --> H[编写Mock测试验证4种关键场景]
    H --> I[清理临时测试文件]
```

## 任务总结
完善了 Agent 的状态机与纠错能力：1. 新增 `reset_level` 允许 Agent 在发现分类路径错误时主动回退并重试；2. 新增 `manual` 动作作为唯一的合法留人工入口；3. 修正 `confidence=low` 语义，将其改为“打回自纠”信号，只有当 Agent 穷尽手段仍无法确定（连续3次low）或主动请求时才留人工，避免了错误的自动归档。
