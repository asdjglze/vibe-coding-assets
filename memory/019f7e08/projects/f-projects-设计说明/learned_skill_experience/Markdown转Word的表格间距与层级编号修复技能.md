---
title: "Markdown转Word的表格间距与层级编号修复技能"
usage_scenario:
    - "Markdown转Word后表格内段落间距过大"
    - "Word文档标题编号出现跨级跳跃（如1→2.3）"
    - "需要按水利/市政等工程规范生成符合层级编号要求的正式设计文档"
keywords:
    - "md_to_docx"
    - "OOXML编号"
    - "表格段落间距"
    - "w:lvlRestart"
---

## 输入
- Markdown源文件路径
- 明确的Word格式要求（如表格无段后距、编号需按层级真实重启）

## 步骤
1. 分析现有转换脚本（如md_to_docx.py）的实现逻辑
2. 定位表格段落格式设置位置，为所有单元格段落添加`space_before=Pt(0)`、`space_after=Pt(0)`、`line_spacing=1.0`
3. 定位编号定义部分，将`w:lvlRestart`值从0-based ilvl改为one-based index（lvl1→'1'，lvl2→'2'，lvl3→'3'，lvl4→'4'）
4. 运行修改后的脚本生成docx

## 输出
生成的Word文档中：①表格内段落无额外段前/段后间距；②标题编号严格按层级重启（如'2'后为'2.1'而非'1.3'）

## 注意事项
- OOXML规范中`w:lvlRestart`使用one-based index，`'0'`表示永不重启，必须修正为对应层级的one-based值
- 表格单元格段落需逐个设置，不能仅设置表格样式
