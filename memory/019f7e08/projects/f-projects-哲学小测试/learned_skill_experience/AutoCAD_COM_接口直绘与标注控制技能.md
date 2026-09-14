---
title: "AutoCAD COM 接口直绘与标注控制技能"
usage_scenario:
    - "需要在 CAD 中绘制工程图纸并添加内部标注组件"
    - "发现标注文字显示错误或比例不对需要排查"
    - "填充功能失败需要定位边界对象格式问题"
keywords:
    - "AutoCAD"
    - "COM 接口"
    - "标注"
    - "ScaleFactor"
    - "填充"
---

## 输入
- AutoCAD 绘图数据（断面尺寸 b1/b2/H/h1/h2/h3）
- 目标比例（如 1:1）

## 步骤
1. 使用 `msp.AddDimAligned(p1, p2, textpos)` 创建对齐标注，**不要设置 TextOverride**，让 CAD 按实际测量距离自动生成文字
2. 标注外观**全部通过标注样式统一控制**（用户硬性要求，不单独改任何标注实体的特性）：`_ensure_dimstyle` 流程 = 激活样式 `doc.ActiveDimStyle=st` → `doc.SetVariable` 设置 DIM* 变量（DIMDEC=2、DIMTXT=0.09、DIMTAD=1、DIMTIH/DIMTOH=0、DIMBLK=_Oblique、DIMSCALE=1 等）→ **`st.CopyFrom(doc)` 把文档级替代固化进样式对象（关键，漏掉则切走激活样式后设置全部丢失）** → 恢复原激活样式
3. AddDimAligned 创建后仅设置 `Layer` 和 `StyleName`（样式名），其余交给样式
4. 几何计算时注意总高 H = h1+h2+h3 已含垫层，垫层底面在 y=H，标注位置应基于 H 而非 H+h3
5. 填充时使用闭合多段线对象数组：先建 `AddLightWeightPolyline` 并设 `Closed=True`，再作为 `VARIANT(VT_ARRAY|VT_DISPATCH, [pline])` 传给 `AppendOuterLoop`
6. 天正 CAD 文档切换后 COM 代理可能失效（<unknown>.ModelSpace），需带重试循环获取 ModelSpace

## 输出
- CAD 模型空间中生成正确的图形和标注，标注文字为自动测量值（如 "0.60"，2 位小数）
- 所有对象比例符合 1:1，文字位于尺寸线上方且与尺寸线对齐

## 注意事项
### 扩展资源
- AutoCAD 2023 ProgID: `AutoCAD.Application.24.2`
- 填充边界必须用对象数组，不能直接用坐标数组
- 天正插件仅拦截 DIMTXSTY 变量（报错"天正软件：设置系统变量时出错"，SendCommand 也绕不开），标注文字为纯数字可接受图纸默认字体；天正可能在样式新建/激活瞬间拒绝 COM 调用（被呼叫方拒绝接收呼叫），SetVariable/CopyFrom 需带重试+延时
