---
title: "Compose重型组件复用与实例内重载技能"
usage_scenario:
    - "WebView切换模板卡顿，发现每次切换都销毁重建"
    - "Compose中NativeView频繁重建导致性能问题"
    - "需要动态加载内容但不想重建整个组件"
keywords:
    - "WebView复用"
    - "AndroidView update"
    - "组件重建"
    - "性能优化"
---

## 输入
- 当前页面存在动态内容切换需求（如模板切换、数据刷新）
- 性能瓶颈表现为切换卡顿，且原因为组件频繁销毁重建

## 步骤
1. **识别重建点**：检查 Compose 代码中是否使用 `key()` 包裹了重型组件（如 WebView、CanvasView），导致每次 key 变化时组件被销毁重建。
2. **移除 key 包裹**：去掉外层 `key()` 调用，让组件实例保持生命周期稳定。
3. **引入 update 回调**：在 `AndroidView` 的 `update` 块中处理状态变化：
   - 更新注入数据（如 JS Bridge 对象属性）
   - 检测关键参数（如 HTML 内容、BaseURL）变化
4. **实例内重载**：当检测到参数变化时，调用原生方法重新加载内容（如 `webView.loadDataWithBaseURL()`），而非重建组件。
5. **桥接层适配**：若需动态注入数据，将 Bridge 对象设为可变（@Volatile），在 update 中直接修改属性并触发重载。

## 输出
- 组件实例不再销毁重建，仅内部内容更新
- 切换操作体感流畅，无原生初始化开销
- 编译通过，功能逻辑不变

## 注意事项
- **核心原则**：重型组件（WebView、SurfaceView等）应复用实例，避免频繁创建/销毁；内容变更应在实例内通过重载或重绘实现。
- **适用场景**：Compose 中 `AndroidView` 封装的原生视图，特别是涉及复杂初始化的组件。
- **性能收益**：消除原生层初始化（Native Init）和配置开销，仅保留渲染层开销。
