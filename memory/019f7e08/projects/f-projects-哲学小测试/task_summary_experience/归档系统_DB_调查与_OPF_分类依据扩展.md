---
title: "归档系统 DB 调查与 OPF 分类依据扩展"
usage_scenario:
    - "调查图书归档系统中数据库的实际写入状态与用途"
    - "在 OPF 元数据文件中安全添加自定义扩展字段（如 AI 判断理由）"
    - "处理中图分类流程中 AI 层级钻入逻辑与自动补全的冲突"
keywords:
    - "library.db"
    - "OPF 扩展"
    - "分类依据"
    - "逐层钻入"
    - "XML 验证"
---

## 任务描述
调查图书馆数据库 (library.db) 的真实写入情况与必要性，并在 metadata.opf 中增加 AI 分类依据字段，同时修正分类流程中的层级钻入逻辑。

## 执行过程
```mermaid
graph TD
    A[调查 library.db 写入状态] --> B[查询规范图集/中图分类库表行数与时间戳]
    B --> C{确认写入真实存在}
    C -->|是 | D[分析 OPDS 订阅依赖与去重作用]
    D --> E[调查 OPF 生成代码结构]
    E --> F[发现 reason 字段未写入 OPF]
    F --> G[使用 meta name=clc_reason 扩展机制写入]
    G --> H[编写 Python 脚本验证 XML 结构与转义]
    H --> I[确认 XML 解析合法]
    I --> J[排查分类日志中的层级矛盾]
    J --> K[尝试添加 build_path 自动补全层级]
    K --> L[用户纠正：必须逐层钻入以让 AI 查看候选]
    L --> M[立即删除 build_path 并回退代码]
    M --> N[确认 os.makedirs 自动创建祖先目录]
```

## 任务总结
1. **DB 调查**：确认 library.db 真实写入（规范图集库 2 行，最新 15:47:57），用于 OPDS 订阅及防重复入库，非必需但无害。
2. **OPF 扩展**：在 organizer.py 中通过 `<meta name="clc_reason" content="..."/>` 写入 AI 分类理由，利用 OPF 2.0 标准扩展机制，经 XML 解析器验证结构合法且不破坏兼容性。
3. **流程修正**：确认归档时 `os.makedirs` 自动创建完整祖先路径；坚决保留“逐层钻入”的分类逻辑，删除了试图自动补全层级的 `build_path` 函数，确保 AI 每层都能看到官方候选类目。
