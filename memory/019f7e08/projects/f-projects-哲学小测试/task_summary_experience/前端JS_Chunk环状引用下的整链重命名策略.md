---
title: "前端JS Chunk环状引用下的整链重命名策略"
usage_scenario:
    - "修改前端静态资源需强制刷新缓存"
    - "处理 Vite/Webpack 打包后的多 Chunk 依赖关系"
    - "解决前端页面白屏或功能不生效的缓存问题"
keywords:
    - "前端缓存"
    - "Chunk重命名"
    - "环状引用"
    - "Vite打包"
---

## 任务描述
将前端首页“在库 X 册”的数字格式从西方三位分组改为中国万位分组（如 50479 -> 5,0479）。

## 执行过程
```mermaid
graph TD
    A[需求: 数字格式改为万位分组] --> B[定位 HomeView.js 中的 ae() 函数]
    B --> C[发现仅修改 ae() 会导致缓存未更新]
    C --> D[分析发现 JS Chunk 存在环状引用]
    D --> E[实施整链改名: custom1 -> custom2]
    E --> F[同步修改 index.html 入口及所有关联 Chunk 引用]
    F --> G[上传服务器并验证渲染结果]
```

## 任务总结
实现了数字格式的中国化分组。关键教训是当 JS 构建产物（Chunk）之间存在环状引用时，**必须全量重命名所有相关 Chunk 文件名**（如 index, HomeView, BookCard 等全部改为 -custom2），否则浏览器会加载旧版 Chunk 导致双实例冲突或白屏。
