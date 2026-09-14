---
title: "Compose 启动页隐藏底部导航的实现经验"
usage_scenario:
    - "Android Compose 应用开发中需要替换或隐藏启动加载动画时"
    - "排查启动页底部导航栏意外显示的问题"
    - "设计全屏覆盖层（如免责声明、协议弹窗）的层级结构"
keywords:
    - "Compose"
    - "启动页"
    - "底部导航"
    - "Scaffold"
    - "全屏覆盖"
---

## 任务描述
将 Android Compose 应用的冷启动旋转加载动画替换为全屏免责声明页，并确保声明页显示时底部导航栏完全隐藏。

## 执行过程
```mermaid
graph TD
    A[需求:替换启动加载动画为免责声明] --> B[初步方案:在 HomeScreen 内部添加 DisclaimerLoadingPage]
    B --> C[用户反馈:底部导航栏仍可见]
    C --> D[问题分析:HomeScreen 是 Scaffold 内容区，无法覆盖 bottomBar]
    D --> E[修正方案:将声明页逻辑上移至 MainActivity 顶层分支]
    E --> F[实现:if(!disclaimerShown) { 显示声明页 } else { 组合 Scaffold }] 
    F --> G[优化:增加 1.2s 最短展示时长防止一闪而过]
    G --> H[清理:移除 HomeScreen 中冗余的声明页代码]
```

## 任务总结
成功实现全屏免责声明页替代启动转圈。关键经验：**启动类全屏页面必须置于 Activity 顶层分支**，避免作为 Scaffold 的子组件导致底部导航栏泄露。通过 LaunchedEffect 控制最短展示时长，确保用户可读性，同时数据在后台并行加载，体验流畅无卡顿。
