---
title: "书架AzPillGroup展开滚动与Tab状态保持"
usage_scenario:
    - "修复Jetpack Compose中下拉菜单/筛选器展开后选项溢出屏幕的问题"
    - "解决Android App中Tab切换导致子页面滚动位置丢失的问题"
keywords:
    - "AzPillGroup"
    - "SaveableStateHolder"
    - "滚动状态"
    - "展开收起"
---

## 任务描述
修复书架页面AzPillGroup展开后收起按钮不可见的问题，以及Tab切换时书架滚动位置丢失的问题。

## 执行过程
```mermaid
graph TD
    A[需求:修复书架UI与状态] --> B[定位AzPillGroup展开溢出问题]
    B --> C[在Az.kt中重构布局:BoxWithConstraints+FlowRow限高滚动]
    C --> D[将展开/收起按钮移出FlowRow并固定]
    D --> E[定位书架滚动状态丢失问题]
    E --> F[在BookshelfScreen.kt中使用rememberSaveable保存LazyGridState]
    F --> G[在MainActivity.kt中引入SaveableStateHolder包裹Tab内容]
    G --> H[补全import并验证编译]
```

## 任务总结
1. **AzPillGroup优化**：在 `Az.kt` 中使用 `BoxWithConstraints` 限制展开区最大高度（40%），内部 `FlowRow` 支持垂直滚动，并将“展开/收起”按钮固定在区域外，确保操作入口始终可见。
2. **状态保持**：在 `BookshelfScreen.kt` 中使用 `rememberSaveable(saver = LazyGridState.Saver)` 保存网格滚动位置；在 `MainActivity.kt` 中使用 `SaveableStateProvider(tab)` 包裹各Tab Composable，确保切回书架时恢复原滚动位置。
