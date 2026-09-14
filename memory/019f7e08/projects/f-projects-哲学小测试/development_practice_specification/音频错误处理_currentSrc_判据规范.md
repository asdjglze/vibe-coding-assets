---
title: "音频错误处理 currentSrc 判据规范"
usage_scenario:
    - "排查音频组件虚假报错或守卫失效问题时"
    - "实现或修改音频播放错误监听逻辑时"
    - "审查播放器代码中 src 判据的健壮性"
keywords:
    - "currentSrc"
    - "src 判据"
    - "虚假错误"
    - "audio 事件"
---

前端音频错误处理需使用 `currentSrc` 而非 `src` 作为判据：浏览器对空 `src=''` 会返回页面 URL 导致守卫恒失效，产生虚假 error（如 code=4 Empty src）。正确做法是检查 `!this.audio.currentSrc`（仅在真正加载过媒体源时才非空），以此区分真实失败与等待阶段的空状态，避免误报错误。
