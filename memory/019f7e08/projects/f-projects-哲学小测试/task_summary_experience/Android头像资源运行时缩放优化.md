---
title: "Android头像资源运行时缩放优化"
usage_scenario:
    - "Android应用中大图转Base64导致内存溢出或UI卡顿"
    - "Compose项目中WebView数据注入性能优化"
    - "处理高分辨率头像资源的通用方案"
keywords:
    - "头像缩放"
    - "Base64优化"
    - "OOM预防"
    - "Android性能"
---

## 任务描述
解决 Android Compose 应用中切换分享模板时出现的卡顿与闪退问题。

## 执行过程
```mermaid
graph TD
    A[现象:切换模板卡死闪退] --> B[移除 ShareCardScreen 中的 Debug 弹窗]
    B --> C[排查根因:检查头像资源大小]
    C --> D[发现部分头像高达 512x512, base64 体积过大]
    D --> E[修改 ShareCardStore.avatarToBase64]
    E --> F[增加运行时缩放逻辑: >384px 则缩放至 384px]
    F --> G[ShareCardScreen 复用 Store 方法并删除私有重复代码]
    G --> H[编译验证通过]
```

## 任务总结
定位到切换模板卡顿是由于大尺寸头像（如 512x512 PNG）转 Base64 后数据量过大，导致 JSON 注入 WebView 时解析缓慢甚至 OOM。通过在 `ShareCardStore.avatarToBase64` 中引入运行时缩放逻辑（统一限制最大边长 384px），有效控制了内存占用和序列化开销，解决了性能问题。
