---
title: "日志需包含完整Traceback"
usage_scenario:
    - "排查程序运行时异常或崩溃原因时"
    - "审查代码中的异常捕获逻辑是否完善时"
keywords:
    - "traceback"
    - "异常处理"
    - "错误定位"
---

项目使用统一日志模块(src/log.py)，所有报错和异常必须输出完整 traceback（逐行显示具体错误），禁止只输出总结性描述
