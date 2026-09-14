---
title: "report生成两阶段职责细化架构"
usage_scenario:
    - "设计或重构AI报告生成流程时确定阶段划分"
    - "新增报告模块需复用现有哲学画像结果时参考阶段接口"
    - "调试报告输出异常时按阶段隔离问题"
keywords:
    - "report生成"
    - "两阶段"
    - "哲学画像"
    - "心理学行为分析"
---

report生成流程严格划分为两个阶段：
- 哲学画像分析阶段：仅接收用户姓名与维度数据，输出严格限定为JSON格式，包含且仅包含`title`、`titleExplanation`、`philosophicalProfile`三个字段，不生成任何行为分析相关内容；
- 心理学行为分析阶段：接收哲学画像（`philosophicalProfile`）与原始维度数据，生成称号解释（复用Phase 1的`title`/`titleExplanation`）、五个生活领域分析（家庭/交友/工作/学习/恋爱）及巴纳姆式行为建议，不再重复生成称号。
