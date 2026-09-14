---
title: "App启动崩溃排查与DB Schema修复"
usage_scenario:
    - "App启动报SQLiteConstraintException等数据库异常"
    - "同步修改Room实体与底层SQLite数据库结构"
    - "清理或重构数据库字段时的风险评估"
keywords:
    - "SQLiteConstraintException"
    - "Room Schema"
    - "数据库修复"
---

## 任务描述
解决 App 启动时因 SQLite 唯一约束失败导致的崩溃，并修复数据库 Schema 不一致问题。

## 执行过程
```mermaid
graph TD
    A[崩溃: UNIQUE constraint failed] --> B[定位 BookEntity.kt 唯一索引]
    B --> C[查询 DB 发现 book 表无索引且存在重复 source]
    C --> D[编写脚本修复 book.source 消除重复]
    D --> E[补全 quote.db 缺失的 6 个索引]
    E --> F[对比 Room Schema v9 发现 article 多 author 列]
    F --> G[错误判断: 删除 article.author 列]
    G --> H[用户质疑: author 是重要标注字段]
    H --> I[纠正: 恢复 author 列并从备份恢复数据]
    I --> J[验证 Schema 完全匹配并重新打包]
```

## 任务总结
1. **根因**: quote.db 缺失所有索引，Room 重建表时撞车唯一约束。
2. **修复**: 补齐 6 个索引，修正 20 本冲突书的 source 字段。
3. **教训**: 严禁仅根据 App 端 Entity 定义删除 DB 字段，必须检查工具链（scripts/）是否使用该字段。
