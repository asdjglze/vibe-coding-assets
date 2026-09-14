---
title: "全量爬取入库与App资源同步全流程"
usage_scenario:
    - "大型网站数据爬取与本地数据库同步项目"
    - "Android App 批量添加新人物/资源并更新注册代码"
    - "处理爬虫增量抓取与去重逻辑"
keywords:
    - "全量爬取"
    - "数据库重建"
    - "App资源同步"
    - "头像注册"
---

## 任务描述
对马克思在线图书馆进行全量爬取（含新增 90 人），整理书籍目录，更新 SQLite 数据库，并将所有人物头像及注册代码同步至 Android App。

## 执行过程
```mermaid
graph TD
    A[需求:全量爬取并同步App] --> B[扩充人物清单: 148人, 排除非人物/手工库]
    B --> C[增量抓取: crawl_new.py 抓新人主页, fetch_missing.py 补缺旧书]
    C --> D[书内抓取: crawl_books.py 实现 URL 复用优化]
    D --> E[数据库重建: rebuild_catalog.py 处理诗集标记/未覆盖兜底]
    E --> F[App 资源同步: gen_android.py 生成头像, 修复同名资源冲突]
    F --> G[代码合并: 将 90 人注册代码插入 Theme.kt]
```

## 任务总结
1. **数据层**: 扩充 `people.json` 至 148 人；爬取 5410 篇主页文章 + 2553 篇书内文章；DB 最终包含 150 人、709 本书、16446 目录条目。
2. **爬虫优化**: 在 `crawl_books.py` 中增加 URL 复用逻辑，避免重复下载；所有脚本增加 `maozedong` 跳过保护。
3. **App 层**: 解决 Android 资源命名冲突（如 `avatar_engels.png` vs `.gif`）；成功将 90 个新人物注册到 `StylePresets`。
