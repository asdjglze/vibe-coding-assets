---
title: "数据库双副本同步与MD5更新规范"
usage_scenario:
    - "修复语料库数据错误时"
    - "执行数据库结构变更或内容更新时"
keywords:
    - "数据库同步"
    - "MD5校验"
    - "双副本"
---

修改数据库后必须同步更新校对工具（notes_editor）中的quote.db和打包资源（app/src/main/assets/databases/quote.db），并重新计算MD5值，防止打包覆盖导致数据丢失
