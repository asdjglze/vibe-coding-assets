---
title: "Compose 视图等比缩放显示与导出不缩水技能"
usage_scenario:
    - "WebView 内容高度超出手机屏幕需整体缩小显示而非滚动"
    - "需要保持 WebView 内部完整渲染同时适配不同屏幕尺寸"
    - "导出 WebView 内容时需保证原始分辨率不因预览缩放而降低"
keywords:
    - "Compose 缩放"
    - "contain 模式"
    - "View 缩放"
    - "导出不缩水"
---

## 输入
- 卡片原始尺寸（宽 Wpx, 高 Hpx）
- 可视区最大尺寸（maxWpx, maxHpx）

## 步骤
1. **计算缩放系数**：`scale = min(1, maxWpx/Wpx, maxHpx/Hpx)`
2. **布局占位调整**：使用 `Modifier.layout` 将子视图的占位尺寸报告为 `(Wpx*scale, Hpx*scale)`，使父容器预留正确空间
3. **视图级缩放绘制**：对 WebView 设置 `view.scaleX = scale` 和 `view.scaleY = scale`，保持内部内容完整渲染，仅改变绘制矩阵
4. **导出恢复原尺寸**：截图前临时将 `scaleX/scaleY` 重置为 1f，执行 draw 后恢复原值，确保导出图片分辨率不缩水

## 输出
- 预览界面：卡片整体等比缩小至可视区内，无滚动或滚动减少
- 导出图片：保持原始分辨率，与未缩放时一致

## 注意事项
- **兼容性**：若 Compose 版本较老不支持 `Placeable.place(x, y, sx, sy)`，必须改用 `View.scaleX/scaleY` 方案
- **性能优势**：缩放是 GPU 合成的一次性变换，无持续重排开销；优于动态高度方案的反复重布局
- **导出一致性**：截图操作必须在恢复 scale=1 后进行，否则导出图片会包含缩放后的低分辨率内容
