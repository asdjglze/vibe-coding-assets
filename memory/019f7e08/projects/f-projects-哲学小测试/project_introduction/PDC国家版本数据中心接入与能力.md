---
title: "PDC国家版本数据中心接入与能力"
usage_scenario:
    - "需要获取权威中图分类时调用PDC接口"
    - "需要下载图书高清封面时从PDC获取"
    - "补充图书CIP编目元数据时"
keywords:
    - "PDC"
    - "中图分类"
    - "封面下载"
    - "AES加密接口"
---

PDC（中国国家版本数据中心）已双形态接入，token 失效为并行小事务（不阻塞主路线）：
1. 认证：网易易盾滑块人工过一次→localStorage user-token（32位hex）写入 state/pdc_token.txt；API 请求头 userSessionId 携带。
2. 失效快跳机制（2026-09-09 用户定案）：首次真实请求判定失效（特征：HTTP 200 / keys=[msg,status] / msg="没有用户"，或 code 509 / msg 含"登录"）→ 写快照 state/pdc_token_stale.json（token md5 指纹）→ 之后所有查询零等待跳过（不请求、不限速）；token 文件更新（指纹变化）自动恢复。人工过滑块是并行路线，主路线不等它。
3. 检索：/api/index/searchQuick AES-CBC（key=zg35ws76swnxz679 iv=z66qa18l0w9o521k）。
4. 接入形态：① AI 工具 pdc_db（中图分类 sort 参考，工具数 13）；② 封面候选池数据源（image 原图 3~7MB 无凭据可下）。
5. 模块：src/pdc_site.py（含限速熔断与失效快照）。
