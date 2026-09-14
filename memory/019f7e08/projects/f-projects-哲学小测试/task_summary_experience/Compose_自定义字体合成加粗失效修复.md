---
title: "Compose 自定义字体合成加粗失效修复"
usage_scenario:
    - "Android Compose 项目中自定义字体加载后加粗/斜体失效排查"
    - "FontManager 或类似字体加载类重构时避免合成丢失"
    - "解决字体样式层级在自定义字体下无法区分的问题"
keywords:
    - "Compose"
    - "字体加载"
    - "合成加粗"
    - "FontFamily"
    - "Typeface"
---

## 任务描述
修复 Android Compose 中自定义字体加载后，所有加粗、斜体等字重请求全部失效的问题。

## 执行过程
```mermaid
graph TD
    A[现象:设置自定义字体后标题/加粗/斜体全变正文粗细] --> B[定位 FontManager.fontFamily 方法]
    B --> C[发现路径:解压 files/fonts/后调用 FontFamily(Typeface)]
    C --> D[根因:FontFamily(Typeface) 不区分字重，不做合成加粗/斜体]
    D --> E[对比:assets 路径用 FontFamily(Font(...)) 正常合成]
    E --> F[修复:统一改为 FontFamily(Font(file)) 构造]
    F --> G[清理不再使用的 Typeface import]
    G --> H[检查 WidgetCardRenderer 是否受影响(无影响)]
    H --> I[编译验证通过]
```

## 任务总结
确认根因为 `FontFamily(Typeface)` 会将所有字重映射到同一 typeface，导致合成加粗/斜体失效。修复方案是将解压路径的构造方式从 `FontFamily(Typeface.createFromFile(file))` 改为 `FontFamily(Font(file))`，使 Compose 能正确识别 Bold/Italic 请求并执行合成。同时清理了冗余的 Typeface import，确保所有字体加载路径行为一致。
