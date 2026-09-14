---
title: "Android资源解压至工作域解决assets读取失败"
usage_scenario:
    - "WebView加载APK内JS/CSS失败或需要动态更新本地资源时"
    - "Android App需要将Assets资源复制到私有目录进行读写操作时"
keywords:
    - "资源解压"
    - "工作域"
    - "assets"
    - "WebView"
    - "ShareTemplateExtractor"
---

## 任务描述
解决 Android WebView 无法读取 APK assets 内 JS/CSS 文件的问题，将所有模板资源解压到应用私有文件目录（filesDir）。

## 执行过程
```mermaid
graph TD
    A[需求:资源全量解压到工作域] --> B[创建 ShareTemplateExtractor]
    B --> C[实现后台线程递归拷贝 assets 到 filesDir]
    C --> D[在 QuoteApplication 启动时调用 extractAllAsync]
    D --> E[修改 ShareTemplateRepository 优先读取文件]
    E --> F[修改 ShareCardWebView 拦截器优先读取文件]
    F --> G[添加路径逃逸防护与 assets 回退机制]
```

## 任务总结
成功实现资源工作域化：
1. 新增 `ShareTemplateExtractor` 对象，负责后台异步解压 `assets/share_templates/` 至 `files/share_templates/`。
2. 采用增量校验（文件大小比对）和半文件防护（写盘后校验）策略。
3. 所有读取逻辑（Repository、WebView 拦截器）均改为「文件优先，assets 兜底」，彻底解决 `openFd` 失败问题。
