---
title: "article表新增需处理全局sortOrder字段"
usage_scenario:
    - "向article表插入新记录时报NOT NULL constraint failed"
    - "需要给article表新增数据并保证排序正确"
keywords:
    - "article"
    - "sortOrder"
    - "NOT NULL"
    - "SQLite"
---

`article`表包含全局递增的`sortOrder`字段且为NOT NULL约束，新增文章时若未赋值会报 `NOT NULL constraint failed: article.sortOrder`。修复方案：插入前查询 `SELECT COALESCE(MAX(sortOrder), -1) FROM article` 获取最大值，插入时赋值为 `max + 1`。（来源：Bash）
