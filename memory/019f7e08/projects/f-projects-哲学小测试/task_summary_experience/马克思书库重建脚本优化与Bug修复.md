---
title: "马克思书库重建脚本优化与Bug修复"
usage_scenario:
    - "修复网页爬虫解析逻辑错误"
    - "处理数据库并发锁问题"
    - "优化长耗时任务的断点续传机制"
keywords:
    - "爬虫修复"
    - "数据库锁"
    - "断点续传"
    - "译本过滤"
---

## 任务描述
优化马克思书库重建脚本，添加实时过程输出，修复目录解析失败、译本标题误匹配、并发锁死及网络限流等问题。

## 执行过程
```mermaid
graph TD
    A[需求: 添加过程输出并优化重建脚本] --> B[在 rebuild_catalog.py 添加 log 调用]
    B --> C[发现 fetch_tocs.py 解析目录页失败 (0 entries)]
    C --> D[修复 book_prefix 计算逻辑, 排除 index.htm]
    D --> E[发现 rebuild_catalog.py 译本标题误匹配 (垃圾数据)]
    E --> F[实现 extract_translator 白名单过滤]
    F --> G[发现并发运行导致数据库锁死]
    G --> H[停止冲突进程, 统一执行顺序]
    H --> I[发现网站限流导致下载极慢]
    I --> J[开发 fetch_missing.py 实现落盘断点续传]
    J --> K[完成全量数据补全与入库]
```

## 任务总结
1. **日志优化**：将 log 输出改为直接写入 UTF-8 文件，避免 PowerShell 重定向乱码。
2. **Bug 修复**：修正 `fetch_tocs.py` 的书页面 URL 前缀计算；在 `rebuild_catalog.py` 中增加译者名白名单过滤，消除垃圾译本标题。
3. **并发处理**：排查并解决因多进程并发导致的 SQLite 数据库锁死问题。
4. **网络优化**：针对网站限流，开发 `fetch_missing.py` 进行落盘式断点续传下载，确保大数据量下的稳定性。
