---
title: "PDC失效响应特征为\"没有用户\""
usage_scenario:
    - "排查PDC接口token失效但未被正确拦截的问题"
    - "调试PDC接口失效判定逻辑"
keywords:
    - "PDC"
    - "token失效"
    - "没有用户"
    - "响应特征"
---

PDC接口token失效时，服务器返回HTTP 200且msg="没有用户"（无code字段），原判定条件（code==509或含"登录"）无法命中，导致失效快照机制不触发。修复后判定逻辑应包含：`if code == "509" or "登录" in msg or "没有用户" in msg:`。（来源：pdc_site.py）
