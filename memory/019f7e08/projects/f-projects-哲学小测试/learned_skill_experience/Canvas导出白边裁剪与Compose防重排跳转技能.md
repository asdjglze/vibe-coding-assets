---
title: "Canvas导出白边裁剪与Compose防重排跳转技能"
usage_scenario:
    - "WebView截图导出时出现底部或侧边白边"
    - "阅读页/列表页需要等数据渲染完成后精准滚动到指定位置"
    - "防止字体替换或异步加载导致的布局重排引发UI抖动"
keywords:
    - "Canvas裁剪"
    - "白边修复"
    - "Compose防重排"
    - "LaunchedEffect门闩"
    - "文本匹配定位"
---

## 输入
- WebView/Canvas导出的图片存在底部或右侧白边（透明区域）
- 需要等待页面彻底加载后跳转到指定内容位置

## 步骤
1. **Canvas边缘裁剪**：在 `canvas.toDataURL` 前调用 `trimCanvas`，从底部向上、右侧向左扫描像素的 Alpha 通道，找到连续全透明行/列并新建画布裁掉。
2. **Compose防重排门闩**：使用 `LaunchedEffect` 监听目标块坐标变化；记录 `jumpExecuted` 计数和 `lastJumpTarget`；若已执行两次（首跳+校准）则放弃；若检测到用户手动滚动偏离目标则尊重用户位置不再拉回。
3. **文本匹配定位**：在 ViewModel 层清洗注码与空白，采用三级匹配策略（完整语录→首句→前6字符指纹）定位正文块下标，再交由 UI 层读取坐标滚动。

## 输出
- 导出图片无多余透明白边
- 页面加载完成后精准跳转至目标段落，且不受字体重排影响导致反复跳动

## 注意事项
- `trimCanvas` 需处理全透明异常（返回 null 保持原画布）
- 竖排模板高度由 JS 显式驱动，重置内联样式时需跳过 height/maxHeight
