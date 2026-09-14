---
title: "KSP 任务不编译业务源码的验证教训"
usage_scenario:
    - "排查编译报错但 KSP 通过时选择正确的验证命令"
    - "新增代码修改后确认是否触发编译错误而非仅代码生成错误"
keywords:
    - "KSP"
    - "compileDebugKotlin"
    - "编译验证"
---

KSP (kspDebugKotlin) 任务仅处理代码生成，不编译业务源码；验证编译错误必须显式运行 compileDebugKotlin 任务，否则无法发现如 Modifier 隐式 receiver 解析等编译期问题。
