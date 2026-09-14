---
title: "AutoCAD ActiveX 标注样式固化技能"
usage_scenario:
    - "使用pywin32操作AutoCAD创建或修改标注样式"
    - "发现通过SetVariable设置的样式参数在切换样式后丢失"
    - "需要确保标注样式配置在图纸中持久生效"
keywords:
    - "AutoCAD"
    - "ActiveX"
    - "CopyFrom"
    - "标注样式"
    - "SetVariable"
---

## 输入
- AutoCAD ActiveX 文档对象 (doc)
- 目标标注样式对象 (st) 或样式名

## 步骤
1. 保存当前激活样式：`old = doc.ActiveDimStyle`
2. 激活目标样式：`doc.ActiveDimStyle = st`
3. 使用 `doc.SetVariable(k, v)` 设置所有需要的 DIM* 系统变量（如 DIMDEC, DIMTXT 等），**每项设置后立即 GetVariable 读回验证**，失败重试（新样式刚创建时 CAD 忙会拒绝 COM 调用，重试 8 次、间隔 0.8 秒）
4. **关键步骤**：调用 `st.CopyFrom(doc)` 将文档级替代固化到样式对象中
5. 恢复原激活样式：`doc.ActiveDimStyle = old`

## 输出
标注样式配置被持久化保存到样式定义中，切换激活样式后再次切回时，设置依然有效。

## 注意事项
- 必须执行 `CopyFrom`，否则 `SetVariable` 设置的只是临时文档替代，切换样式后会丢失。
- 建筑标记箭头必须用 `DIMBLK="_ARCHTICK"`（配合 DIMTSZ 控制大小），不要用 `_Oblique`（那是倾斜箭头，不是建筑标记）。
- `DIMTXSTY` 可以正常通过 SetVariable 设置（此前误判为被天正拦截——实际失败原因是目标文字样式名不存在）。hztxt.shx 是大字体，文字样式必须 `fontFile="gbenor.shx"` + `bigFontFile="hztxt.shx"` 配合使用，不能把 hztxt.shx 当常规字体。
- 样式名要带版本+随机 GUID 段（如 `DTQ_DIM_V1_9F3C72A5`），避免与用户图纸/模板中任何现有样式撞名。
- 修改代码后必须重启后端服务且**确认旧进程已全部杀死**：用 `netstat -ano | findstr :端口` 检查，同一端口可能被多个旧进程同时监听（Windows 允许多进程绑定），请求会被旧进程处理导致新代码"看起来没生效"。
