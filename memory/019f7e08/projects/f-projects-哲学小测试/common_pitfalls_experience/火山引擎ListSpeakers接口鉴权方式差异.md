---
title: "火山引擎ListSpeakers接口鉴权方式差异"
usage_scenario:
    - "调用火山引擎ListSpeakers接口报InvalidCredential错误"
    - "需要定时同步火山引擎音色列表但无法鉴权"
    - "区分TTS合成凭证与OpenAPI管理凭证"
keywords:
    - "火山引擎"
    - "ListSpeakers"
    - "鉴权"
    - "AK/SK"
    - "X-Api-Key"
---

火山引擎豆包语音的 ListSpeakers 接口不支持 TTS 合成使用的 `X-Api-Key` 鉴权，会返回 `400 InvalidCredential: Invalid credential in 'Authorization'`。该接口属于 OpenAPI 体系，必须使用 AccessKey ID + Secret AccessKey 进行 HMAC256 或 Bearer Token 签名鉴权。若需定时拉取音色列表，需在火山引擎控制台 IAM 创建子账号获取 AK/SK，并在服务器环境变量中配置，不能复用现有的 TTS API Key。（来源：Bash, WebSearch）
