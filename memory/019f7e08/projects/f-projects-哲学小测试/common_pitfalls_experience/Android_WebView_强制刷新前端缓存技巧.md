---
title: "Android WebView 强制刷新前端缓存技巧"
usage_scenario:
    - "Android App 修改了前端代码但手机上仍显示旧界面"
    - "WebView 加载远程资源后出现缓存导致的逻辑不一致"
    - "排查\"改了代码没生效\"的问题"
keywords:
    - "Android"
    - "WebView"
    - "HTTP 缓存"
    - "时间戳参数"
---

Android WebView 加载远程前端页面时，若未设置缓存策略，会缓存旧版 index.html，导致后端代码修改后手机端仍显示旧逻辑（"改了还一样"）。解决方案：在 loadUrl 请求 URL 中添加时间戳参数破坏 HTTP 缓存，例如 `val cacheBust = "v=${System.currentTimeMillis()}"`，并将 URL 构造为 `$baseUrl/?token=xxx&$cacheBust`。（来源：WebRadioClient.kt）
