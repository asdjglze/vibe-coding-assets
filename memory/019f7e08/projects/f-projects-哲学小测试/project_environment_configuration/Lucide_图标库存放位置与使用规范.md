---
title: "Lucide 图标库存放位置与使用规范"
usage_scenario:
    - "项目中需要图标资源时选择 Lucide 库而非自绘或 emoji"
    - "Android 端将 SVG 图标转换为 vector drawable 时参考路径和格式"
keywords:
    - "Lucide 图标库"
    - "SVG 图标"
    - "vector drawable"
---

项目使用 Lucide 开源图标库（lucide-static v1.33.0，ISC 许可）替代 emoji：完整 2034 个 SVG 图标存放于 f:\projects\maoyulu\resources\lucide\icons\（24×24 stroke 风格，path 数据可直接转 Android vector drawable）。App 内需要图标语义时从此库选取对应图标（如 heart/bookmark/share），转为 vector drawable 或在 Compose 中渲染。
