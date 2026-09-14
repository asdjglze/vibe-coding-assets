---
title: "Android Room 数据库 Schema 不匹配修复"
usage_scenario:
    - "Android App 启动时 Room 数据库初始化崩溃"
    - "数据库迁移或升级时的 Schema 一致性校验"
    - "排查 SQLite 唯一约束冲突问题"
keywords:
    - "Room"
    - "Schema 校验"
    - "UNIQUE constraint"
    - "数据库修复"
---

## 任务描述
修复 Android App 因 Room 数据库 Schema 不匹配导致的 UNIQUE constraint 崩溃。

## 执行过程
```mermaid
graph TD
    A[App 启动崩溃: UNIQUE constraint failed] --> B[分析堆栈: Room 重建表撞约束]
    B --> C[检查 quote.db: 发现无索引且 book 表有重复 source]
    C --> D[编写脚本: 修正 book.source 为书名去重]
    D --> E[对比 App Schema v9: 发现 db 缺 6 个索引]
    E --> F[执行修复: 补齐索引 + 删除 article 多余 author 列]
    F --> G[用户反馈: author 列非多余, 需保留标注数据]
    G --> H[紧急恢复: ADD COLUMN + 从备份恢复 author 数据]
    H --> I[最终验证: Schema 完全匹配, 外键 0 违规]
```

## 任务总结
成功修复数据库崩溃。关键教训：
1. **根因**：DB 缺失唯一索引 `index_book_kind_personId_source` 且存在重复数据，导致 Room 校验失败走重建路径。
2. **避坑**：不要仅凭 App 实体定义就删除 DB 列，需确认工具端是否使用该列（如 `article.author` 用于研究标注）。
3. **结果**：补齐 6 个索引，修正 20 本书的 source，恢复 author 列及数据。
