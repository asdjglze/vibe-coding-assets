---
title: "SQLite数据库连接时自动重建表结构技能"
usage_scenario:
    - "SQLite报错 no such table 但确认建表语句已执行过"
    - "数据库文件意外丢失或损坏后需要自动恢复表结构"
    - "防止因外部清理导致空数据库文件引发的运行时错误"
keywords:
    - "SQLite"
    - "幂等DDL"
    - "自动建表"
    - "no such table"
---

## 输入
- SQLite 数据库文件路径

## 步骤
1. 在获取数据库连接的函数（如 `get_conn`）中，建立连接后立即执行幂等 DDL 语句（如 `conn.execute('CREATE TABLE IF NOT EXISTS ...')`）
2. 执行 `conn.commit()` 确保建表生效
3. 返回连接对象供后续读写使用

## 输出
无论数据库文件是否存在或是否为空，都能成功获取包含正确表结构的连接，避免 `no such table` 错误。

## 注意事项
- **核心原理**：SQLite 在连接不存在的数据库文件时会静默创建一个空的物理文件（0字节），但不会自动创建表结构。如果程序逻辑假设表已存在而直接查询/插入，就会报 `no such table` 错误。
- **适用场景**：所有基于 SQLite 的应用，特别是那些可能因清理、迁移或异常导致 `.db` 文件丢失的场景。
- **性能影响**：`CREATE TABLE IF NOT EXISTS` 开销极微秒级，适合在每次获取连接时调用。
