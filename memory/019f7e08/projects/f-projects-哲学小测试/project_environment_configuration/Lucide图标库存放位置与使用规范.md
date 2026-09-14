---
title: "Lucide图标库存放位置与使用规范"
usage_scenario:
    - "App内需要图标时从Lucide库选取图标"
    - "移植分享卡片时替换emoji为矢量图标"
keywords:
    - "Lucide图标库"
    - "SVG转vector"
    - "emoji替代"
    - "图标资源位置"
---

maoyulu 项目使用 Lucide 开源图标库（lucide-static v1.33.0，ISC 许可）替代 emoji：完整 2034 个 SVG 图标存放于 f:\projects\maoyulu\resources\lucide\icons\（24×24 stroke 风格，path 数据可直接转 Android vector drawable）。App 内需要图标语义时从此库选取对应图标（如 heart/bookmark/share），转为 vector drawable 或在 Compose 中渲染。
