---
title: "TTS 音色后端权威管理与下架兜底策略"
usage_scenario:
    - "实现或重构 TTS 服务时确定音色来源与校验逻辑"
    - "排查用户反馈音色异常或更换后未生效问题"
    - "设计设置页音色选择功能的交互与数据同步"
keywords:
    - "TTS音色"
    - "tts_speaker"
    - "后端权威"
    - "PUT profile"
    - "立即保存"
    - "设置面板"
---

TTS 音色管理采用后端权威模式：User 表存储 tts_speaker 字段，登录返回及设置页修改均通过 PUT /api/auth/profile 保存（注意是 PUT 不是 POST，POST 会 405）；后端 /api/playlist/tts 接口从数据库读取音色，忽略前端传入值。合成前校验音色有效性：若用户配置的音色已下架，自动使用默认音色生成并记录日志，不修改用户配置。设置页加载音色列表时，若当前音色不存在，弹出提示告知用户并自动切换回默认音色。关键规范：音色选择后必须立即调用 saveAllPending 落库，不能依赖 markDirty 的 3 分钟延迟定时器——用户选完音色在 3 分钟内关闭设置面板/页面会导致配置永久丢失；主页面 closeSettings 关闭面板时必须 postMessage('settings-save-request') 通知设置 iframe 立即保存。
