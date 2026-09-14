---
title: "metadata.db 高频增量同步规范"
usage_scenario:
    - "设计或修改图书入库同步逻辑时"
    - "优化 OPDS/COPS 前端数据展示延迟问题"
    - "评估归档流程的性能与用户体验"
keywords:
    - "增量同步"
    - "实时发布"
    - "归档落库"
    - "sync_incremental"
---

图书管理系统中 metadata.db 的更新机制需支持高频增量同步，禁止仅在轮末统一发布。要求在归档落库循环中实现即时或批量（如每满 N 本）触发 sync_incremental()，确保读者能尽快看到新书，避免受限于 AI 分析速度导致的长时间滞后。
