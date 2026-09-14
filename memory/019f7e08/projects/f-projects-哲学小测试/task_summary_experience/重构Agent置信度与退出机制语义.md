---
title: "重构Agent置信度与退出机制语义"
usage_scenario:
    - "重构AI Agent的分类或编目逻辑"
    - "调整Agent的置信度评估与异常处理策略"
    - "定义Agent的动作语义（如重试、放弃、拒绝）"
keywords:
    - "Agent重构"
    - "置信度语义"
    - "拒绝输出"
    - "信息缺失"
---

## 任务描述
重构图书分类 Agent 的执行逻辑，解决原逻辑中"置信度低（low）"、"留人工（manual）"和"拒绝输出（refuse）"语义混淆的问题。明确：low 仅是质量标注，照常归档；只有书名和责任者全缺才是"信息完全缺失"，应触发 refuse；中途发现无法处理才用 manual。

## 执行过程
```mermaid
graph TD
    A[需求:重构Agent置信度与退出机制] --> B[分析现有agent.py/recognizer.py/organizer.py拦截逻辑]
    B --> C[发现旧逻辑: confidence=low 会拦截或打回]
    C --> D[修改agent.py: 移除low打回逻辑，增加refuse动作支持]
    D --> E[修改agent.py: 增加"信息完全缺失"校验（书名+责任者全缺）]
    E --> F[修改recognizer.py/organizer.py: 仅拦截书名缺失，放行low结果]
    F --> G[重写agent_system.md: 明确low/manual/refuse语义及触发场景]
    G --> H[编写mock测试验证新逻辑]
    H --> I[确认4个场景全部通过]
```

## 任务总结
成功实现语义解耦：
1. **confidence=low**：不再触发拦截或打回，视为有效结果正常归档入库。
2. **refuse（拒绝输出）**：新增专用动作。当 AI 完成探索但仍找不到书名和责任者（信息完全缺失）时，必须输出此动作，禁止硬交空结果。
3. **manual（中途退出）**：保留用于中途发现无法处理（如文件损坏）的场景。
4. **拦截逻辑优化**：主流程仅在"书名缺失"时拦截，不再因置信度低而跳过。
