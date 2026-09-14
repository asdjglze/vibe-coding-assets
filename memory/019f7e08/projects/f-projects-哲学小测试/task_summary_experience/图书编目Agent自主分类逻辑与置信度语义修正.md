---
title: "图书编目Agent自主分类逻辑与置信度语义修正"
usage_scenario:
    - "调整AI Agent的分类决策逻辑"
    - "修正置信度(confidence)的业务含义"
    - "优化图书编目流程中的自动归档与人工干预阈值"
keywords:
    - "自主分类"
    - "置信度语义"
    - "Agent逻辑"
    - "CIP修正"
---

## 任务描述
修正图书编目Agent的分类逻辑，解决"无CIP分类号即判定为low并留人工"的错误行为，确立"按内容实质自主分类"的核心原则。

## 执行过程
```mermaid
graph TD
    A[需求:修正分类逻辑] --> B[分析 agent_system.md 规则]
    B --> C[修改 agent.py 校验逻辑]
    C --> D[移除 final 阶段的 low 打回机制]
    D --> E[修改 recognizer.py 拦截条件]
    E --> F[修改 organizer.py 兜底防护]
    F --> G[重写 agent_system.md 分类铁律]
    G --> H[验证编译与逻辑一致性]
```

## 任务总结
1. **语义修正**：明确 `confidence` 仅表示元数据（书名/作者等）的身份确认程度，不再作为分类完成的阻碍。
2. **自主分类**：在 `agent_system.md` 中规定，无CIP/现成证据是常态，AI必须根据正文、目录、书籍结构进行实质归类，禁止因无证据而放弃分类。
3. **逻辑变更**：
   - `agent.py`: 移除 `_LOW_MAX_TRIES` 和 low 状态打回循环；`final` 校验只关注分类路径合法性与深度，low 照常归档。
   - `recognizer.py` & `organizer.py`: 拦截条件从 `confidence==low OR title缺失` 简化为仅 `title缺失`。Low 置信度的书若书名已确认，照常入库归档。
4. **人工介入**：仅当 AI 显式输出 `manual` 动作或书名完全无法确认时，才留给人工处理。
