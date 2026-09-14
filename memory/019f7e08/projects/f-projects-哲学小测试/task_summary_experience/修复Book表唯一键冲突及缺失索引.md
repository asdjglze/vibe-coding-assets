---
title: "修复Book表唯一键冲突及缺失索引"
usage_scenario:
    - "Android Room 应用启动报 UNIQUE constraint failed 崩溃"
    - "SQLite 数据库迁移后出现唯一键冲突"
    - "排查 App 端 Schema 校验失败导致的重建崩溃"
keywords:
    - "Room"
    - "唯一约束"
    - "SQLite索引"
    - "Book表"
---

## 任务描述
解决 Android App 启动崩溃 `UNIQUE constraint failed: book.kind, book.personId, book.source`。

## 执行过程
```mermaid
graph TD
    A[崩溃: UNIQUE constraint failed] --> B[定位 BookEntity.kt 唯一索引定义]
    B --> C[查询 quote.db 发现 6 组 (kind,personId,source) 重复数据]
    C --> D[分析原因: 私人汇编类书籍 source 均为通用词'著作']
    D --> E[编写 _fix_book_key.py 将 source 改为书名去书名号]
    E --> F[发现 quote.db 缺失全部 6 个 Schema 索引]
    F --> G[编写 _fix_index.py 补齐所有索引及外键检查]
    G --> H[验证重复消除且无外键违规]
```

## 任务总结
1. **数据修复**：20 本私人汇编书籍的 `source` 字段由通用词（如'著作'）修正为具体书名，消除了唯一键冲突。
2. **Schema 修复**：补齐了 `quote.db` 中缺失的 6 个索引（含 `index_book_kind_personId_source`），确保与 App Room v9 定义一致。
3. **经验教训**：当 App 端 Room 报错唯一约束失败时，需同时检查**数据重复**和**数据库索引是否存在**。
