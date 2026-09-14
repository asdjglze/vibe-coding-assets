---
title: "Compose 搜索高亮与程序化选区实现技能"
usage_scenario:
    - "需要实现全文搜索功能并支持正则匹配"
    - "需要区分搜索高亮与用户手动选区的视觉样式"
    - "需要在阅读页实现语录进入时的自动选中并弹出分享按钮"
keywords:
    - "Compose"
    - "SelectionContainer"
    - "正则搜索"
    - "程序化选区"
    - "高亮熄灭"
---

## 输入
- 需要实现全文搜索功能，支持正则匹配
- 需要区分搜索高亮与用户手动选区的视觉样式
- 需要实现语录进入时的自动选中并弹出分享按钮

## 步骤
1. **正则搜索实现**：
   - 使用 Kotlin Regex 进行匹配，结合 `LIKE` 语句的最长字面片段预筛优化性能
   - 在 UI 层添加 `.*` 胶囊开关，切换时重新执行搜索
2. **选区配色区分**：
   - 搜索高亮使用人物强调色（如朱砂红）
   - 用户选区使用独立配色（如深蓝 #3D6B99），通过 `CompositionLocalProvider(LocalTextSelectionColors)` 包裹正文容器设置
3. **程序化选区实现**：
   - 在 `SelectionContainer` 内使用 `SelectionState.select(TextRange)` 进行全局字符空间定位
   - 计算目标块在全局文本中的偏移量（累加前序 selectable 长度），调用 `select` 触发真选区（弹出分享按钮）
4. **交互熄灭机制**：
   - 监听滚动状态 `isScrollInProgress`，一旦用户拖动即熄灭搜索高亮
   - 切篇/点击搜索按钮/输入新词时恢复高亮

## 输出
- 搜索功能支持正则匹配
- 搜索高亮与用户选区颜色分离
- 语录进入自动选中整段并弹出分享按钮
- 搜索高亮随用户交互自动熄灭

## 注意事项
- `SelectionState.select(range: TextRange)` 参数为全局字符偏移，需准确计算各 selectable 块的累计长度
- 系统 SQLite 默认未注册 regexp 函数，需自行实现正则匹配逻辑
- 选区配色需通过 CompositionLocal 传递，避免影响其他组件
