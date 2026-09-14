---
title: "report两阶段职责细化"
usage_scenario:
    - "重构report生成逻辑时明确各阶段输入输出边界"
    - "调试两阶段数据传递异常时对照职责定义"
    - "新增report衍生功能时判断应归属哪个阶段"
keywords:
    - "两阶段职责"
    - "哲学画像"
    - "行为分析"
    - "title生成"
---

report生成流程严格划分为两个阶段：
- 哲学画像分析阶段：仅接收维度数据与用户名，输出包含`title`、`titleExplanation`、`philosophicalProfile`三字段的JSON，不生成任何行为分析内容；
- 心理学行为分析阶段：接收哲学画像（`philosophicalProfile`）与原始维度数据，生成称号解释（复用Phase 1的`title`/`titleExplanation`）、生活领域分析及巴纳姆建议，不再重复生成称号。
两阶段通过`report-generator.js`串联，结果合并为完整报告。
