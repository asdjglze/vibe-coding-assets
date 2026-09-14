---
title: "lateinit 属性销毁前判空规范"
usage_scenario:
    - "编写 Activity 生命周期方法时检查 lateinit 属性"
    - "排查因未初始化属性导致的崩溃日志"
keywords:
    - "lateinit"
    - "isInitialized"
    - "Activity"
    - "崩溃"
---

Kotlin lateinit 属性在 Activity onDestroy() 中访问前必须检查 isInitialized，否则当 onCreate() 未完成赋值即触发 finish() 时会抛出 UninitializedPropertyAccessException 导致闪退
