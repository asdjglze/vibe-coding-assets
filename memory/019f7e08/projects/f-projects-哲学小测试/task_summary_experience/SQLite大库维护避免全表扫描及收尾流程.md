---
title: "SQLite大库维护避免全表扫描及收尾流程"
usage_scenario:
    - "SQLite数据库维护与故障恢复"
    - "机械硬盘上的大数据量操作优化"
    - "清理临时文件或残留状态"
keywords:
    - "SQLite"
    - "全表扫描"
    - "性能优化"
    - "数据库维护"
---

## 任务描述
修复因排查误将主库重命名为 quote_hold.db 的问题，并清理相关的 WAL/Journal 残留及诊断脚本。

## 关键教训
**严禁在 SQLite 大库（尤其是机械盘冷缓存）上使用 `SELECT COUNT(*)` 进行数据完整性验证**。全表扫描会导致极高的 I/O 延迟和超时风险。验证应基于业务日志确认（如 COMMIT 记录）或使用轻量级查询（如 `PRAGMA integrity_check` 或特定索引查询）。

## 执行过程
```mermaid
graph TD
    A[需求:恢复主库名并清理] --> B[识别到 COUNT(*) 验证的低效性]
    B --> C[编写 _finalize.py 脚本]
    C --> D[实现 os.rename 恢复 quote.db]
    D --> E[设置 journal_mode=DELETE 并 checkpoint]
    E --> F[清理 -wal/-shm/-journal 残留]
    F --> G[删除诊断用的临时脚本]
```

## 任务总结
成功创建并执行 `_finalize.py`，恢复了数据库文件名，优化了存储模式，并清理了环境垃圾，避免了高耗时的全表扫描操作。
