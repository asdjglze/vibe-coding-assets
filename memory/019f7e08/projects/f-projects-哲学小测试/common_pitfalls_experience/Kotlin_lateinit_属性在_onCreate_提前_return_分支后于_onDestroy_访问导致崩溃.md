---
title: "Kotlin lateinit 属性在 onCreate 提前 return 分支后于 onDestroy 访问导致崩溃"
usage_scenario:
    - "修复 Activity 启动即闪退问题"
    - "编写含提前跳转分支的 Activity 初始化代码"
    - "排查 lateinit 属性未初始化异常"
keywords:
    - "lateinit"
    - "UninitializedPropertyAccessException"
    - "onDestroy"
    - "MainActivity"
    - "闪退"
---

Activity 中用 lateinit 声明的 View 属性（如 WebView）若在 onCreate 的提前 return 分支（如无配置跳转设置页后 finish）中未赋值，onDestroy 里直接调用会抛 UninitializedPropertyAccessException 导致闪退。修复：onDestroy 中先判断 `if (::webView.isInitialized)` 再销毁；更稳妥的做法是把视图属性声明为可空类型（var webView: WebView? = null），或把跳转分支移到视图初始化之后。
