---
title: "数据库用途与 OPF 扩展规范"
usage_scenario:
    - "评估是否保留 library.db 数据库时参考其实际用途"
    - "修改 OPF 元数据或添加自定义字段时遵循标准扩展机制"
keywords:
    - "library.db"
    - "OPDS"
    - "OPF 扩展"
    - "分类依据"
---

library.db 数据库用于 OPDS 订阅目录生成及内容指纹去重（防止重复入库），若不使用 OPDS 则仅保留去重功能，不建议擅自删除。
OPF 文件通过<meta name="clc_reason" content="..."/>扩展字段存储 AI 分类依据，该方式符合 OPF 2.0 标准扩展机制，不会破坏文件结构或导致解析失败。
