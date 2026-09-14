---
title: "PDF生成元数据写入规范"
usage_scenario:
    - "使用工具生成PDF时确认元数据字段是否完整"
    - "检查生成的PDF属性信息是否符合归档要求"
    - "排查PDF元数据缺失或显示异常问题"
keywords:
    - "PDF元数据"
    - "作者字段"
    - "时间格式"
    - "归档属性"
---

电子书下载工具生成的PDF需自动写入标准元数据：
- title: 书名
- author: 作者（可选）
- subject: 来源网站
- keywords: 电子书,扫描页图，来源站
- creator: 工具名称
- producer: PyMuPDF
- creationDate/modDate: 本机当前时间（PDF标准D:...格式带时区）
