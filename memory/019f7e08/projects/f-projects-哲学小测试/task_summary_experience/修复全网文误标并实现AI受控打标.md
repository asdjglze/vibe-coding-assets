---
title: "修复全网文误标并实现AI受控打标"
usage_scenario:
    - "修复基于文件后缀的粗暴分类/打标逻辑"
    - "实现基于AI识别信号的精细化标签管理"
    - "引入受控词表限制AI自由发挥的打标场景"
keywords:
    - "网络小说"
    - "误标修复"
    - "受控词表"
    - "AI判定"
    - "标签流"
---

## 任务描述
修复代码中将所有 .txt 文件强制标记为"网络小说"的错误逻辑，改为由 AI 在识别阶段判定是否为网文，并在打标阶段使用受控词表进行精准打标。

## 执行过程
```mermaid
graph TD
    A[需求:修复全网文误标逻辑] --> B[定位 organzier.py/tagger.py/补标脚本中的 .txt 硬编码]
    B --> C[设计新方案:引入 webnovel 信号 + 受控词表打标]
    C --> D[修改 agent_system.md 增加 AI 判定规则]
    D --> E[创建 webnovel_tag_task.md 提示词]
    E --> F[在 tagger.py 实现 webnovel_generate_tags 及 _clean_webnovel_tags]
    F --> G[修改 organizer.py tag_after_archive 按信号分流]
    G --> H[删除 tagger.py 轮对账中的 txt 分支]
    H --> I[修改 d_归档.py 透传 webnovel 字段]
    I --> J[清理 补文学网文标签.py 误伤逻辑]
    J --> K[更新源程序流程.md 文档]
```

## 任务总结
成功消除基于文件格式的误标风险。实现了三层联动：
1. **识别层**：AI 在 final 输出中增加 `webnovel` 布尔值，依据正文证据判定。
2. **流转层**：通过 `d_归档.py` 将信号透传给归档流程。
3. **打标层**：`organizer.py` 根据信号调用 `webnovel_generate_tags`，该函数结合 `webnovel_taxonomy.json` 受控词表让 AI 打标，并由 `_clean_webnovel_tags` 进行程序侧校验过滤，确保标签合规。
