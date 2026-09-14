---
title: "SQLite book 表 sortOrder 非关联字段且需全局唯一"
usage_scenario:
    - "排查文章无法定位到书籍的原因"
    - "修改书籍排序序号时发现跨人物重复"
    - "需要确保数据库排序字段的全局唯一性"
keywords:
    - "sortOrder"
    - "全局唯一"
    - "SQLite"
    - "外键"
    - "数据修复"
---

在 SQLite 数据库中，book 表的 sortOrder 列仅用于排序显示，**不是主键也不是外键**。文章与书籍的关联完全依赖 source 文本匹配和 toc_entry 表的外键引用，与 sortOrder 无关。修改 sortOrder 时需注意：**必须保持全局唯一性**，避免不同人物下的书籍序号冲突（如毛选与鲁迅的书都用了 1-5）。若出现跨人物重复，虽不影响按人物分组后的排序显示，但会破坏数据完整性。修复时需同步更新所有数据库副本（如 notes_editor/quote.db 和 app assets/quote.db），并通过 SQL 校验唯一性：`SELECT sortOrder, GROUP_CONCAT(id) FROM book GROUP BY sortOrder HAVING COUNT(*) > 1`。（来源：Bash）
