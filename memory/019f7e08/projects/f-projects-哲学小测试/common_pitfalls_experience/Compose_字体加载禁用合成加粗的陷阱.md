---
title: "Compose 字体加载禁用合成加粗的陷阱"
usage_scenario:
    - "Android Compose 应用设置自定义字体后加粗/斜体失效"
    - "字体切换后标题与正文粗细一致无法区分"
    - "排查字体渲染异常时怀疑字重映射问题"
keywords:
    - "Compose"
    - "FontFamily"
    - "Typeface"
    - "合成加粗"
    - "字重"
---

在 Android Compose 中，使用 `FontFamily(Typeface)` 构造函数加载自定义字体时，该 Typeface 会映射所有字重请求（Bold/SemiBold/Italic）到同一实例，**不做合成加粗或合成斜体**。这会导致设置字体后，标题、加粗（**text**）、斜体等依赖粗细区分的格式全部失效，与正文同粗细。

正确做法：优先使用 `FontFamily(Font(file))` 或 `FontFamily(Font(path, assets))` 构造，让 Compose 引擎处理字重合成。

来源：Bash (编译验证), Grep (代码分析)
