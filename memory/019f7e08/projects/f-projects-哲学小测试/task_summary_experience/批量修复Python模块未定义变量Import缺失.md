---
title: "批量修复Python模块未定义变量/Import缺失"
usage_scenario:
    - "项目重构或新增功能后排查潜在运行时错误"
    - "新模块集成时的依赖完整性检查"
keywords:
    - "NameError"
    - "Import缺失"
    - "pyflakes"
    - "代码维护"
---

## 任务描述
排查并修复项目中多个模块因缺少 `import` 语句导致的 `NameError`（潜伏Bug）。

## 执行过程
```mermaid
graph TD
    A[需求:修复NameError] --> B[使用pyflakes扫描所有src模块]
    B --> C[定位organizer/recoginzer/web_searcher等文件的未定义变量]
    C --> D[逐一补充缺失的import语句]
    D --> E[验证编译通过及功能正常]
```

## 任务总结
修复了以下文件的导入缺失问题：
- `organizer.py`: 补全 `domain_root`
- `recognizer.py`: 补全 `classify_log`
- `web_searcher.py`: 补全 `re`
- `library_db.py`: 补全 `json`
- `opds.py`: 补全 `json` 并修复函数签名
此类问题通常表现为‘第一次执行某功能时崩溃’，建议在新功能上线前进行全量静态检查。
