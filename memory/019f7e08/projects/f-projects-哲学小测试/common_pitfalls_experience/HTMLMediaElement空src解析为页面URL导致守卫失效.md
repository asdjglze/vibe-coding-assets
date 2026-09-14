---
title: "HTMLMediaElement空src解析为页面URL导致守卫失效"
usage_scenario:
    - "HTML5 audio 虚假 error 事件排查"
    - "判断媒体元素是否有真实音源"
    - "播放器错误守卫失效修复"
keywords:
    - "empty src"
    - "currentSrc"
    - "fake error"
    - "media element"
    - "playback guard"
---

浏览器中给 audio/video 元素赋空 src（或 removeAttribute('src')）后，读取 .src 属性返回的是当前页面 URL 而非空字符串，导致 `if (!audio.src)` 之类的空源守卫恒为真失效：清空 src 触发的虚假 error 事件（code=4 Empty src）会被当成真实失败误报。判断"元素是否有真实媒体源"必须用 currentSrc（仅当真正加载过媒体源后才非空），或在赋空 src 前加条件（currentSrc 非空或 hasAttribute('src') 才执行清空）从源头减少虚假 error。真实加载失败（无效 URL/坏数据）的 error 事件发生时 currentSrc 已指向失败 URL（非空），故 currentSrc 判据可准确区分虚假 error 与真实失败。
