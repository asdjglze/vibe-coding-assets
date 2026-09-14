---
title: "book 表 sortOrder 全局唯一化与打包校验"
usage_scenario:
    - "数据库 book 表排序字段出现跨人物重复或乱序问题需重构"
    - "打包发布前需要自动化校验语料库排序一致性"
    - "实现人物置顶 + 笔画排序 + 全局连续编号的业务场景"
keywords:
    - "sortOrder"
    - "全局唯一"
    - "人物排序"
    - "打包校验"
---

## 任务描述
解决 book 表 sortOrder 跨人物重复问题，实现全局唯一连续编号，并将校验逻辑集成到打包流程中。

## 执行过程
```mermaid
graph TD
    A[需求:sortOrder 全局唯一] --> B[分析代码链路确认 sortOrder 非主键/外键]
    B --> C[发现毛选与鲁迅 sortOrder 撞车 1-5]
    C --> D[设计新规则：马恩列毛斯置顶 + 名字笔画序 + 人物内书架序]
    D --> E[编写 verify_book_sort.py 校验脚本]
    E --> F[修复源库 quote.db 的 sortOrder 为全局连续编号]
    F --> G[修改 build_pack.ps1 集成校验步骤]
    G --> H[验证幂等性与打包流程完整性]
```

## 任务总结
成功实现 sortOrder 全局唯一化：人物按「马恩列毛斯置顶 + 笔画序」排列，人物内按类别/出版年份/卷序排列，全局从 1 连续编号。新增 scripts/verify_book_sort.py 脚本，在打包流程中自动校验并修复，确保「全部」筛选页可直接按 sortOrder 排序得到正确顺序，无需 UI 层额外逻辑。
