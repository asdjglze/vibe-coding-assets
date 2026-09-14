---
title: "PDC平台Token失效快跳与并行维护机制"
usage_scenario:
    - "排查PDC检索耗时过长或频繁超时问题时"
    - "实现其他账号制接口的Token失效快速跳过逻辑时"
keywords:
    - "Token失效"
    - "快跳机制"
    - "并行小事务"
    - "快照"
---

PDC Token失效处理机制（2026-09-09 用户定案）：
1. 失效判定：HTTP 200 / keys=[msg,status] / msg="没有用户"，或 code 509 / msg 含"登录"。
2. 失效快跳：首次真实请求判定失效 → 写快照 state/pdc_token_stale.json（token md5 指纹）→ 之后所有查询零等待跳过（不请求、不限速）；token 文件更新（指纹变化）自动恢复。
3. 并行路线：人工过滑块是并行路线，主路线不等它。
