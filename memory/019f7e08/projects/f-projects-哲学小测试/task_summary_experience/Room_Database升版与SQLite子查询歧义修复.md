---
title: "Room Database升版与SQLite子查询歧义修复"
usage_scenario:
    - "Android Room数据库版本升级与Migration实现"
    - "SQLite跨库ATTACH UPDATE时的列名歧义排查"
keywords:
    - "Room Migration"
    - "SQLite歧义"
    - "Schema升级"
---

## 任务描述
修复因 App Schema 不匹配导致的 UNIQUE 崩溃，并完成 v10 升级及 author 数据恢复。

## 执行过程
```mermaid
graph TD
    A[需求:修复崩溃并升v10] --> B[App端Article.kt加author字段]
    B --> C[AppDatabase.kt版本升至10并注册Migration]
    C --> D[QuoteApplication.kt注册Migration]
    D --> E[尝试SQL ATTACH UPDATE恢复数据]
    E --> F[发现子查询列名解析歧义导致全空]
    F --> G[改用Python逐条UPDATE恢复151条数据]
    G --> H[编译生成v10 schema并验证匹配]
```

## 任务总结
成功消除崩溃根因：
1. **Schema升级**：App端增加 `author` 字段，版本升至 v10，注册 Migration 防止旧库重建。
2. **数据恢复**：修正了 SQL 子查询歧义 Bug，通过 Python 逐行更新恢复了 151 篇作者标注。
3. **打包发布**：v1.32 版本三档打包成功，Schema 全量验证通过。
