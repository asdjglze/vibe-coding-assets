---
title: "Compose 全屏覆盖页隐藏底部导航栏架构技能"
usage_scenario:
    - "实现免责声明等全屏启动页但底部导航栏仍显示"
    - "需要全屏覆盖层遮挡底部导航栏或状态栏"
    - "Scaffold 布局下需要临时隐藏底部导航"
keywords:
    - "Compose"
    - "Scaffold"
    - "全屏覆盖"
    - "底部导航"
    - "架构设计"
---

## 输入
- 需要实现一个全屏覆盖的启动页或弹窗（如免责声明、加载页）
- 当前应用使用 Scaffold 作为主布局，包含底部导航栏（bottomBar）

## 步骤
1. 检查目标页面是否被嵌套在 Scaffold 的内容区（content）内
2. 若需全屏覆盖且隐藏底部导航栏，必须将页面提升至 Scaffold 外层（如 Activity 顶层）
3. 使用条件分支控制渲染顺序：先渲染全屏覆盖层（此时不组合 Scaffold），再渲染主界面 Scaffold
4. 确保覆盖层使用 fillMaxSize() 并独占整个窗口空间

## 输出
- 全屏页面显示时，底部导航栏完全不可见
- 页面自动切换后，Scaffold 正常渲染并显示底部导航栏

## 注意事项
- Scaffold 的 bottomBar 是其子组件，只要 Scaffold 被组合，bottomBar 就会显示
- 全屏覆盖层不能放在 Scaffold.content 内部，否则无法遮挡 bottomBar
- 推荐使用 if/else 分支在顶层控制渲染逻辑，而非在 content 内叠加
