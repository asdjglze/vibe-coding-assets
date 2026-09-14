---
title: "Python潜伏Bug排查与静态检查实践"
usage_scenario:
    - "Python项目运行时NameError排查"
    - "大型项目重构后的代码一致性检查"
    - "新成员接手遗留代码时的质量评估"
keywords:
    - "NameError"
    - "pyflakes"
    - "静态检查"
    - "import缺失"
---

## 任务描述
排查并修复因缺少 import 导致的运行时 NameError 错误，建立预防机制。

## 执行过程
```mermaid
graph TD
    A[报错:name 'domain_root' is not defined] --> B[定位organizer.py引用处]
    B --> C[发现import语句缺失]
    C --> D[补充domain_root导入]
    D --> E[使用pyflakes全量扫描src目录]
    E --> F[识别出classify_log/re/json等未定义变量]
    F --> G[批量修复recognizer/web_searcher/library_db/opds等文件]
    G --> H[验证编译通过]
```

## 任务总结
修复了 organizer.py 中 domain_root 未导入的潜伏 Bug。通过 pyflakes 静态扫描发现了 recognizer.py (classify_log)、web_searcher.py (re)、library_db.py (json)、opds.py (json/title) 等多处同类未定义变量问题并全部修复。建议在后续开发中，将 pyflakes 加入常规检查流程，避免此类仅在特定执行路径下触发的错误。
