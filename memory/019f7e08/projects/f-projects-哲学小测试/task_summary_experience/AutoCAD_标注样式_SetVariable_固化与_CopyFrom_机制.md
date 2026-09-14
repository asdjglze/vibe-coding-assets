---
title: "AutoCAD 标注样式 SetVariable 固化与 CopyFrom 机制"
usage_scenario:
    - "调试 AutoCAD 二次开发中样式设置不生效的问题"
    - "排查 SetVariable 设置后读取值为空或旧值的情况"
    - "编写 AutoCAD 样式初始化脚本时的验证逻辑"
keywords:
    - "AutoCAD"
    - "SetVariable"
    - "CopyFrom"
    - "DimStyle"
    - "pywin32"
---

## 任务描述
解决 AutoCAD 通过 pywin32 设置 DimStyle 系统变量（如 DIMDEC, DIMTXT）后失效的问题。

## 执行过程
1. **诊断**：发现 `doc.SetVariable` 设置的变量仅为文档级替代（Overrides），若不固化，切换激活样式或重启后值会丢失。
2. **误区排除**：此前误判为'恢复激活样式导致失效'或'天正插件拦截'，经实验证实天正仅拦截 `DIMTXSTY`，其他变量正常。
3. **核心方案**：采用 Autodesk ActiveX 官方标准流程：
   - 激活目标样式对象 (`doc.ActiveDimStyle = st`)
   - 使用 `doc.SetVariable` 设置系统变量（带重试机制防 COM 偶发失效）
   - **关键步骤**：调用 `st.CopyFrom(doc)` 将文档级变量写入样式对象持久化
   - 恢复原激活样式（此时新值已固化，安全无副作用）

## 任务总结
成功定位失效根源为缺少 `CopyFrom` 固化步骤。修正后的 `_ensure_dimstyle` 流程确保样式参数（字体、颜色、箭头、小数位等）在导出 DXF 及前端 Canvas 示意图中完全生效。验证表明，7 个标注对象回读参数正确，切走再切回图纸样式保持无误。
