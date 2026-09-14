---
title: "SWF 文件被 WAF 拦截导致解析失败的处理"
usage_scenario:
    - "SWF 转 PDF 过程中大量页面渲染失败并报尺寸错误"
    - "解析 SWF 文件时出现负数宽高或协议错误"
    - "怀疑下载的文件并非真正的 SWF 格式"
keywords:
    - "SWF"
    - "WAF"
    - "HTML 拦截"
    - "文件头校验"
    - "CWS"
---

SWF 转 PDF 时，部分 .swf 文件实际是网站 WAF（如华为云）拦截脚本请求后返回的 HTML 验证页（内容包含 `<html><script>` 等），导致解析出的舞台宽高为负数或异常值，引发浏览器协议错误（`Emulation.setDeviceMetricsOverride: Screen width and height values must be positive`）。解决方案：在读取 SWF 前校验文件头是否为 `CWS` 或 `FWS`，若不符合则视为坏文件跳过并记录警告。（来源：Bash, Python）
