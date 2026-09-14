---
title: "SQLite数据库头像标识符到人物名的映射技能"
usage_scenario:
    - "需要将数据库中的资源ID/Key转换为人类可读的名称"
    - "查询数据库时遇到终端中文乱码问题"
    - "不确定数据库字段具体存储格式时的探索性查询"
keywords:
    - "SQLite"
    - "头像映射"
    - "中文乱码"
    - "PRAGMA"
---

## 输入
- SQLite 数据库路径
- 需要查找的人物头像标识符列表（如 `['bernstein', 'frank']`）

## 步骤
1. **探查表结构**：使用 `PRAGMA table_info(person)` 获取列名，确认头像存储字段（如 `avatarRes`）。
2. **分析数据格式**：查询少量非空记录（`SELECT name, avatarRes FROM person WHERE avatarRes IS NOT NULL LIMIT 5`），确认字段值是否包含前缀或特定格式（例如 `avatar_xxx` 而非纯 `xxx`）。
3. **构建查询逻辑**：根据实际格式调整 SQL 条件（如 `WHERE avatarRes = 'avatar_' || ?`）。
4. **解决编码问题**：若终端输出存在乱码，将结果写入 UTF-8 编码的文件中读取，或直接打印 Unicode 转义序列。

## 输出
一份清晰的“头像标识符 -> 人物中文名”映射列表。

## 注意事项
- 终端环境可能不支持正确显示中文，务必使用文件输出或确保终端编码为 UTF-8。
- 数据库中的资源名可能与文件名不完全一致（如加前缀），需先观察样例数据。
