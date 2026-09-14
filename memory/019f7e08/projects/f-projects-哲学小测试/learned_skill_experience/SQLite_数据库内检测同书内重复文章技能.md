---
title: "SQLite 数据库内检测同书内重复文章技能"
usage_scenario:
    - "需要检查数据库中是否存在同一书籍下不同编号但内容完全相同的文章"
    - "发现数据导入异常，疑似存在批量重复入库情况"
    - "进行数据清洗前的重复项分析与定位"
keywords:
    - "SQLite"
    - "重复检测"
    - "归一化"
    - "数据清洗"
    - "Python 脚本"
---

## 输入
- SQLite 数据库路径（包含 book, article, toc_entry 表）
- 归一化规则（去除 Markdown/HTML 标签、全角字符、标点符号、空白符等）
- 最小内容长度阈值（如 40 字符）

## 步骤
1. 连接数据库，加载 book, article, toc_entry 数据到内存字典
2. 遍历每本书的目录条目 (toc_entry)，按 bookId 分组
3. 对每组内的文章 content 应用归一化函数，过滤短文本
4. 将归一化后的内容作为 Key，收集对应的 articleId 列表
5. 筛选出 Key 对应多个不同 articleId 的组（即同书内不同编号但内容一致）
6. 输出重复组详情（书名、articleId、标题、引用次数）及统计信息

## 输出
- 重复组列表：包含书名、组号、涉及的 articleId 及其在书中的引用次数
- 汇总统计：重复组总数、冗余目录条目数
- 处理建议：保留 sortOrder 最小的 article，删除其他 article 的 toc_entry 或整条记录

## 注意事项
- PowerShell 不支持长多行内联 Python 代码，需先写入 .py 文件再执行
- 归一化必须彻底（包括全角/半角标点、特殊 Unicode 范围），否则误判
- 删除前务必备份数据库，并记录被删除文章的详细信息以便恢复
