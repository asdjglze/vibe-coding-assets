---
title: "Android ICU 正则字符类 em/en dash 相邻触发崩溃"
usage_scenario:
    - "编写包含中文标点分隔符的正则字符类时"
    - "应用启动崩溃并报 ExceptionInInitializerError 时"
    - "排查 PatternSyntaxException 降序范围错误时"
keywords:
    - "ICU regex"
    - "dash character class"
    - "PatternSyntaxException"
    - "ExceptionInInitializerError"
    - "em dash en dash"
---

Android ICU 正则引擎（java.util.regex 底层）会把字符类中相邻的 em dash U+2014 后跟 en dash U+2013 的组合（如 [·•、—–—\s-]+）解释为降序字符范围，Regex 初始化即抛 PatternSyntaxException，若在静态初始化（companion/顶层 val）处会连带 ExceptionInInitializerError 崩溃。规避：en dash 放字符类首位、连字符 - 放末尾（[–·•、—\s-]+），首位与末位字符不会被当作范围端点。排查该问题时全项目搜索 —– 或 –— 相邻模式。
