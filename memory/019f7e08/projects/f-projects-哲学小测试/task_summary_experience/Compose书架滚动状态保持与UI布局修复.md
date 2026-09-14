---
title: "Compose书架滚动状态保持与UI布局修复"
usage_scenario:
    - "Jetpack Compose中LazyList/LazyGrid的状态持久化"
    - "解决FlowRow或列表项过多导致按钮不可见的问题"
    - "处理复杂文本标题的正则解析逻辑"
keywords:
    - "SaveableStateHolder"
    - "LazyGridState"
    - "Compose UI"
---

## 任务描述
修复书架展开列表溢出、Tab切换丢失滚动位置及书名解析异常等UI问题。

## 执行过程
```mermaid
graph TD
    A[需求:修复书架UI与状态] --> B[AzPillGroup限高滚动+固定按钮]
    B --> C[MainActivity加SaveableStateProvider]
    C --> D[BookshelfScreen用rememberSaveable保存GridState]
    D --> E[优化CoverPeriod_RE正则保护括号内容]
    E --> F[人物改名与简介重写]
```

## 任务总结
1. **AzPillGroup**：展开区限制最大高度比例并启用垂直滚动，收起按钮置于 BoxWithConstraints 外部确保永远可见。
2. **状态保持**：MainActivity 中使用 `SaveableStateProvider(tab)` 包裹页面，配合 `LazyGridState.Saver` 实现切 Tab/阅读页后恢复滚动位置。
3. **正则优化**：`COVER_PERIOD_RE` 排除括号字符，`COVER_PAREN_RE` 整体保护 `(时期)` 块，避免书名被截断。
