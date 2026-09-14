---
title: "Android混合应用播放状态不同步修复技能"
usage_scenario:
    - "Android App中点击播放按钮无声音但UI显示已播放"
    - "原生播放器源失效导致前端resume操作无效"
    - "WebView混合开发中JS与Native状态同步问题"
keywords:
    - "ExoPlayer"
    - "状态同步"
    - "resumeMusic"
    - "混合开发"
---

## 输入
- Android App中点击播放按钮无声音，但UI显示为暂停/播放状态
- 场景特征：刚部署新前端后出现，或歌曲播完/出错后出现

## 步骤
1. **现象确认**：确认按钮是否有视觉反馈（如变为暂停图标）
2. **根因分析**：通常是因为原生播放器源失效（ExoPlayer STATE_IDLE或出错），但前端仍认为`hasSource=true`
3. **原生层兜底**：在`resumeMusic()`等恢复播放的方法中，增加对原生播放器状态的检查。若发现源已失效（`playbackState == STATE_IDLE`或无源），则主动从影子队列重新加载当前歌曲，而非直接调用无效的`play()`
4. **前端层同步**：监听原生的错误回调（如`onMusicError`），一旦收到错误，立即将前端的`hasSource`置为`false`，确保下次点击走完整的重播流程而非简单的resume

## 输出
点击播放按钮后，音乐正常播放，且状态切换与UI一致

## 注意事项
- ExoPlayer在源出错或结束后会进入IDLE状态，此时调用`play()`无效
- 必须同时修改原生和前端代码，单侧修改可能导致状态再次不一致
