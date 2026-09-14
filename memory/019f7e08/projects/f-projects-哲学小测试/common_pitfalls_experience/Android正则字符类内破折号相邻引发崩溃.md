---
title: "Android正则字符类内破折号相邻引发崩溃"
usage_scenario:
    - "Android应用启动时抛PatternSyntaxException或ExceptionInInitializerError"
    - "Kotlin/Java正则表达式包含多个破折号符号"
keywords:
    - "ICU正则"
    - "PatternSyntaxException"
    - "破折号"
    - "Android"
---

Android的ICU正则引擎会将字符类中相邻的破折号解析为降序字符范围，导致PatternSyntaxException崩溃。具体场景：em dash (— U+2014) 后紧跟 en dash (– U+2013) 会被当作范围 \u2014-\u2013。修复方法：将en dash移至字符类首位（如 [–·•、—\s-]+），连字符放末尾。（来源：Bash）
