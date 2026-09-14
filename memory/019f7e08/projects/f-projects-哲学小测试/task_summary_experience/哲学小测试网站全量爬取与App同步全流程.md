---
title: "哲学小测试网站全量爬取与App同步全流程"
usage_scenario:
    - "从类似网站批量爬取人物资料并同步到App"
    - "处理大量图片资源导入Android项目时的命名冲突"
    - "爬虫脚本的增量抓取与限流保护机制设计"
keywords:
    - "爬虫"
    - "全量爬取"
    - "App同步"
    - "资源冲突"
---

## 任务描述
从马克思在线图书馆爬取148位人物的书籍、文章、头像等资源，整理并同步至Android App数据库。

## 执行过程
```mermaid
graph TD
    A[需求:全量爬取148人物数据] --> B[确认人物清单: 148人, 排除手工库maozedong]
    B --> C[抓取主页结构: probe_structure.py]
    C --> D[抓取主页文章: crawl_new.py]
    D --> E[抓取书目录: fetch_tocs.py]
    E --> F[抓取书内文章: crawl_books.py]
    F --> G[补缺下载: fetch_missing.py]
    G --> H[重建DB: rebuild_catalog.py, 含诗集标记逻辑]
    H --> I[同步App DB: sync_app_db.py]
    I --> J[扫描PDF: pdf_scan.py]
    J --> K[生成Android资源: gen_android.py]
    K --> L[解决资源冲突: 删除同名不同扩展名旧png]
```

## 任务总结
成功完成148人数据爬取与入库：
1. 数据量：150人（147爬虫+3手工库），14228篇文章，709本书，16446目录条目。
2. 脚本优化：所有爬虫脚本增加`SKIP={'maozedong'}`保护，避免覆盖手工库；增加限流间隔（1.5s）和并发控制。
3. 资源处理：gen_android.py生成147个头像，解决aapt2同名不同扩展名冲突（删除旧png保留新jpg/gif）。
4. 诗集标记：rebuild_catalog.py实现POETRY_RX正则匹配，全站实际无诗集类型书，逻辑就绪。
