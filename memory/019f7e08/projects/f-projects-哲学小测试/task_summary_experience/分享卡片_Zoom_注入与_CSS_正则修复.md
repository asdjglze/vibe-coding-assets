---
title: "分享卡片 Zoom 注入与 CSS 正则修复"
usage_scenario:
    - "修复 WebView 分享卡片被意外缩放或导出尺寸异常的问题"
    - "排查 CSS 样式在导出时丢失或颜色错误的根因"
    - "处理 Android 打包档位导致的字体缺失或加载失败问题"
keywords:
    - "分享卡片"
    - "Zoom 注入"
    - "CSS 正则"
    - "字体回退"
---

## 任务描述
修复分享卡片在 WebView 中显示异常的问题：卡片被缩成一点、导出图片包含多余透明空白区域、Mini 档位下字体与字色错误。

## 执行过程
```mermaid
graph TD
    A[需求:修复分享卡片显示与导出异常] --> B[调查问题现象:缩小成点/导出空白/字体错误]
    B --> C[定位 share_core.js 发现旧版 Zoom 注入逻辑残留]
    C --> D[分析 Zoom 副作用:导致画布失真/分辨率降低模糊]
    D --> E[排查 CSS 正则替换:发现误伤.body 类选择器导致样式失效]
    E --> F[检查 Mini 档打包清单:发现 fzxkjw 字体缺失及 maoti_si.ttf 损坏]
    F --> G[移除 share_core.js 中的 Zoom 注入与相关测量逻辑]
    G --> H[修复 CSS 正则:先保护.body 再替换 html/body 避免误伤]
    H --> I[修正模板字体引用:32_memphis.html 回退 kaiti, p01_shupai.html 回退 maobixing]
```

## 任务总结
成功解决分享卡片三大问题：
1. 移除 share_core.js 中导致卡片缩小的 CSS zoom 注入逻辑，恢复 1:1 自然渲染，消除导出时的透明空白与模糊。
2. 修复 CSS 样式提取正则，采用“先保护 .body 后替换 body”策略，防止 .body 类选择器被破坏导致字色/字号丢失。
3. 修正 Mini 档位下的字体回退方案：将引用缺失字体 fzxkjw 的模板改为楷体，损坏的 maoti_si.ttf 改为毛体行书，确保三档全量可用。
