---
title: "metadata.db更新机制及同步延迟原因"
usage_scenario:
    - "排查新书未在前端显示的原因"
    - "理解图书管理系统的元数据同步流程"
    - "分析 run.py 脚本的执行逻辑"
keywords:
    - "metadata.db"
    - "同步机制"
    - "sync_queue"
    - "run.py"
---

在图书管理系统中，`metadata.db` 的更新时机并非实时，而是由 `run.py` 脚本控制，仅在两个时刻执行：1. **轮首**：若文件不存在则全量重建，否则增量同步；2. **轮末**：先增量同步，若两域 library.db 总数与 metadata.db 不一致则全量重建校准。归档操作产生的变更仅记录在对应域的 `library.db` 的 `sync_queue` 中，不直接修改 `metadata.db`。因此，若脚本运行中途卡住或未跑完一轮，前端无法看到新书。（来源：Bash, SearchReplace）
