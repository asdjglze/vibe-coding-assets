---
title: "Kotlin @JavascriptInterface属性与JS方法同名冲突"
usage_scenario:
    - "WebView JS桥接调用返回异常或JSON解析失败"
    - "Kotlin桥接类中定义了与JS方法同名的属性"
keywords:
    - "Kotlin"
    - "@JavascriptInterface"
    - "WebView"
    - "桥接冲突"
---

在Android WebView的`@JavascriptInterface`桥接类中，如果定义了属性（如`data`），Kotlin会自动生成对应的getter方法（如`getData()`）。当JS侧也调用同名方法（如`window.ShareBridge.getData()`）时，会命中Kotlin的属性getter而非预期的方法实现，导致返回对象而非数据，引发JSON解析失败。解决方案：将Kotlin属性重命名（如改为`cardData`），确保JS调用的方法名不与自动生成的getter冲突。（来源：Bash/Readtool）
