---
title: "CAD导出技术选型"
usage_scenario:
    - "新增或修改 CAD/DXF 导出功能时"
    - "排查 CAD 文件打开异常或乱码问题时"
keywords:
    - "ezdxf"
    - "DXF"
    - "CAD"
    - "开源库"
---

项目 CAD 出图支持两条路径：1) ezdxf 开源库生成 DXF 文件下载；2) AutoCAD 官方 COM 接口直绘（pywin32，AutoCAD 2023 ProgID AutoCAD.Application.24.2），在模型空间直接绘制，尺寸标注用 CAD 内部 AddDimAligned 组件（1:1 自动测量，不写固定文字），填充用 AddHatch。
