---
title: "PDC国家版本数据中心接入规范与失效处理"
usage_scenario:
    - "为书库补中图分类或封面时"
    - "维护PDC登录token与检索时"
    - "评估第三方图书数据源时"
keywords:
    - "PDC"
    - "失效快跳"
    - "AES-CBC"
    - "state/pdc_token"
    - "封面下载"
---

PDC（中国国家版本数据中心）已双形态接入：1. 认证：网易易盾滑块人工过一次→localStorage user-token（32位hex）写入 state/pdc_token.txt；API 请求头 userSessionId 携带。2. 失效快跳机制：首次真实请求判定失效（HTTP 200 keys=[msg,status] msg="没有用户" 或 code 509）→ 写快照 state/pdc_token_stale.json → 后续查询零等待跳过；token 文件更新自动恢复。人工过滑块为并行路线，不阻塞主流程。3. 检索：/api/index/searchQuick AES-CBC（key=zg35ws76swnxz679 iv=z66qa18l0w9o521k）。4. 数据源：AI工具pdc_db（中图分类sort参考，13个工具）及封面候选池（image原图3~7MB无凭据可下）。模块：src/pdc_site.py（含限速熔断与失效快照）。
