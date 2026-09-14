---
title: "Subsonic API 转码参数与下载接口语义"
usage_scenario:
    - "配置 Subsonic 播放器时遇到音质问题需禁用转码"
    - "调用 download 接口获取文件却收到 JSON 错误响应"
    - "排查音乐服务器数据库记录存在但文件丢失的问题"
keywords:
    - "Subsonic"
    - "format=raw"
    - "maxBitRate"
    - "download 接口"
    - "API 语义"
---

Subsonic API 中 `maxBitRate=0` **仅表示不限制比特率**，**不等于禁用转码**。要获取原始文件流或禁用转码，必须使用 `format=raw` 参数（1.9.0+）。`download.view` 接口用于获取原始文件，若文件在磁盘上不存在，会返回 HTTP 200 + JSON 格式的错误响应（包含 `<subsonic-response>` 标签及错误码 70），而不是直接报错或返回空数据。验证方法：检查返回数据的 Content-Type 和头部字节（正常音频为 `fLaC` 或 `ID3`，错误时为 JSON 结构）。（来源：WebFetch, Read）
