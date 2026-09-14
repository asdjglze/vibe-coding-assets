---
title: "KSP 任务不编译业务源码，编译验证须用 compileDebugKotlin"
usage_scenario:
    - "修改Android源码后验证编译是否通过"
    - "排查打包阶段才暴露的Kotlin编译错误"
    - "KSP项目选择正确的验证任务"
keywords:
    - "KSP"
    - "compileDebugKotlin"
    - "编译验证"
    - "隐式receiver"
    - "BoxWithConstraints"
---

在 KSP 项目中，`:app:kspDebugKotlin`（即使加 --rerun-tasks）只执行注解处理任务，不编译业务 Kotlin 源码。用它"验证编译"会产生假象：源码中的编译错误（如 BoxWithConstraintsScope.maxHeight 在嵌套 Column/FlowRow 的 Modifier 表达式里隐式 receiver 解析失败）直到 assembleDebug 打包时才暴露。验证代码编译必须运行 `:app:compileDebugKotlin`，否则可能把编译错误留到打包阶段才发现。
