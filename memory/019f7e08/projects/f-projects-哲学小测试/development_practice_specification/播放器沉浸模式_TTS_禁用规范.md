---
title: "播放器沉浸模式 TTS 禁用规范"
usage_scenario:
    - "实现或优化播放器串词功能时确认是否跳过 TTS"
    - "排查 TTS 异常播放问题是否因沉浸模式未生效"
    - "新增播放控制逻辑时需考虑沉浸模式的状态同步"
keywords:
    - "沉浸模式"
    - "TTS 拦截"
    - "纯音乐播放"
    - "串词禁用"
---

播放器沉浸模式（immersion_mode）开启时，必须彻底禁用 AI 朗读串词：
1. **源头拦截**：TTS 任务管理器（ttsTaskManager.js）在 ensureImmediate/ensurePreload 入口检查开关，开启时直接 return，不调用后端 AI 朗读接口；
2. **播放决策**：播放器（player.js）在_playSong/playSongDirectly 入口跳过串词流程，直接纯音乐播放；
3. **动态响应**：监听 storage 事件，播放途中开启沉浸模式需立即中止当前 TTS 任务。
4. **状态同步**：设置页保存开关后需同步 localStorage.user，确保主页面播放器跨页面立即生效。
5. **串词数据始终生成**：后端 music_recommend_service.py 生成歌单时无论沉浸模式开关都生成串词（_generate_scripts），沉浸模式仅由前端拦截播放。原因：若后端跳过串词生成，用户关闭沉浸模式后当前歌单全部无 script，TTS 永远不播，需重新生成歌单才能恢复。
