---
title: "提示词外部化工程:统一Loader与Skills目录规范"
usage_scenario:
    - "大型LLM应用项目的提示词管理"
    - "需要将Prompt与代码解耦的重构任务"
    - "多模块共享LLM交互逻辑的项目"
keywords:
    - "提示词外部化"
    - "prompt_loader"
    - "skills目录"
    - "代码重构"
---

## 任务描述
将项目中所有硬编码在 Python 源文件内的 LLM 提示词（System Prompt / Task Template / JSON Spec）拆分为独立的 Markdown 文件，实现提示词外部化管理。

## 执行过程
```mermaid
graph TD
    A[需求:提示词外部化] --> B[创建src/prompt_loader.py统一Loader]
    B --> C[定义skills/目录结构存放.md文件]
    C --> D[拆分agent.py中的系统提示与折叠/回退模板]
    D --> E[拆分merger.py/folder_judge.py/web_searcher.py的判定提示词]
    E --> F[拆分standard.py的标准识别与分类提示词]
    F --> G[大规模拆分recognizer.py的复杂识别流程提示词]
    G --> H[更新所有源文件引用:从硬编码改为_pl()调用]
    H --> I[编写冒烟测试验证模板占位符匹配性]
    I --> J[清理残留硬编码字符串]
```

## 任务总结
完成全项目提示词外部化改造：
1. **统一 Loader**：建立 `src/prompt_loader.py`，支持进程内缓存，路径自动指向 `skills/` 目录。
2. **文件拆分**：将 `agent`, `merger`, `standard`, `folder_judge`, `web_searcher`, `recognizer` 六大模块的提示词全部移至 `skills/*.md`。
3. **规范落地**：每个 .md 文件只包含一个完整的提示词内容；Python 代码中仅保留变量插值逻辑，禁止出现长文本硬编码。
4. **质量保障**：通过自动化脚本验证所有模板的占位符（%s/.format）与调用参数一致，防止运行时 KeyError。
