---
title: "Compose图表交互与后台计时修复技能"
usage_scenario:
    - "Compose热力图点击识别错位或显示错误日期"
    - "Popup悬浮窗弹出位置偏移或超出容器范围"
    - "应用后台运行时仍在累计阅读时长或统计数据"
keywords:
    - "Compose"
    - "Popup定位"
    - "LifecycleOwner"
    - "后台计时"
---

## 输入
- Compose图表组件存在交互或统计异常（如点击识别不准、悬浮窗位置偏移、后台持续计时）

## 步骤
1. **点击识别修正**：检查热力图等网格组件的点击坐标计算，确保行号使用`高度/行数`而非列宽，避免行列尺寸不一致导致的错位。
2. **悬浮窗定位修正**：确认`Popup`的`offset`是相对父容器的局部坐标，不要叠加全局窗口位置（`positionInWindow()`），否则会导致气泡弹出到容器外；需在父容器范围内钳制坐标。
3. **后台计时修正**：阅读/统计类时长记录需监听`LifecycleOwner`，在`ON_STOP`时结算并暂停计时，在`ON_START`时恢复，防止App退后台后继续累积时长。

## 输出
- 点击事件准确映射到对应单元格
- 悬浮窗紧贴点击点且完全位于父容器内
- 后台停留时间不计入有效时长

## 注意事项
- `LocalLifecycleOwner`已从`androidx.compose.ui.platform`移至`androidx.lifecycle.compose`包，注意import路径更新。
- `Popup`的坐标系是相对于其直接父布局的，切勿与`positionInWindow`混用。
