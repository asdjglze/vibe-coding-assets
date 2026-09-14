---
title: "CAD导出技术栈变更"
usage_scenario:
    - "新增或修改CAD绘图功能时选择COM接口而非DXF生成"
    - "排查CAD自动化脚本兼容性问题时"
keywords:
    - "AutoCAD COM接口"
    - "pywin32"
    - "CAD自动化"
---

项目出图功能从使用ezdxf库改为调用AutoCAD官方COM接口，实现直接在CAD软件中绘制图形和使用内部标注组件
