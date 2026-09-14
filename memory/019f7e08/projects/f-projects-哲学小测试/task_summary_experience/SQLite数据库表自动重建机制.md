---
title: "SQLite数据库表自动重建机制"
usage_scenario:
    - "SQLite数据库文件可能丢失或误删的场景"
    - "需要提高本地数据存储稳定性的应用"
    - "开发阶段快速原型构建时的容错处理"
keywords:
    - "SQLite"
    - "自动建表"
    - "数据库容错"
    - "幂等DDL"
---

## 任务描述
解决 SQLite 数据库文件丢失或损坏后，程序运行时出现 'no such table' 错误的问题。

## 执行过程
```mermaid
graph TD
    A[需求:解决db文件丢失报错] --> B[定位library_db.py中的get_conn]
    B --> C[分析发现init()仅在初始化时调用]
    C --> D[修改get_conn()增加幂等DDL执行]
    D --> E[编写测试脚本模拟db删除场景]
    E --> F[验证删除db后再次写入能自动重建表]
```

## 任务总结
修改 `library_db.py` 的 `get_conn()` 方法，在每次获取连接时执行 `CREATE TABLE IF NOT EXISTS`。这样即使数据库文件被意外删除、移动或损坏，下一次数据库操作时会自动重建表结构，避免了 'no such table' 异常，提高了程序的自愈能力。
