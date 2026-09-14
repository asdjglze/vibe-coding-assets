---
title: "阅读页Markdown解析性能优化"
usage_scenario:
    - "Android Compose应用页面加载卡顿排查"
    - "Markdown渲染引擎性能优化"
    - "ViewModel状态管理与UI层数据共享"
keywords:
    - "性能优化"
    - "Markdown解析"
    - "CommonMark"
    - "ViewModel缓存"
---

## 任务描述
解决阅读页打开文章时渲染卡顿问题（等待数秒）。排查发现同一篇文章在 UI 层被 CommonMark 全文解析了 5 次（渲染、标题索引、语录匹配、分享副标题等各自调用 MdParser.parse）。

## 执行过程
```mermaid
graph TD
    A[需求:解决阅读页渲染慢] --> B[搜索 MdParser.parse 调用点]
    B --> C[发现 5 处独立解析逻辑]
    C --> D[分析耗时原因:低端机多次全量解析]
    D --> E[重构 ReaderViewModel: loadContent 时解析一次存入 doc]
    E --> F[重构 ReaderScreen: renderModel/headings/shareSubtitle 读取共享 doc]
    F --> G[修复编译错误:参数名不一致]
    G --> H[验证编译通过]
```

## 任务总结
成功将文章解析从 5 次减少到 1 次。在 `ReaderViewModel` 中新增 `doc: MdDoc?` 字段缓存解析结果，UI 层所有依赖 Markdown 结构的组件均改为读取该缓存，消除了冗余计算开销。
