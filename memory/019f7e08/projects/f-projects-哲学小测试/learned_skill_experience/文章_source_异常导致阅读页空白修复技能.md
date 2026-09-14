---
title: "文章 source 异常导致阅读页空白修复技能"
usage_scenario:
    - "App 端搜索命中文章但点击后阅读页显示空目录或空白"
    - "校对工具新增文章后无法在 App 端正确定位阅读"
    - "需要构建数据一致性兜底机制防止孤立文章"
keywords:
    - "source 异常"
    - "阅读页空白"
    - "目录兜底"
    - "数据修复"
    - "定位链路"
---

## 输入
- 现象：App 端搜索能命中文章，但点击后阅读页显示空目录或空白页
- 约束：需同时修复现有数据并防止未来新增/编辑时再次发生

## 步骤
1. **诊断根因**：检查数据库 `article` 表的 `source` 字段是否为空或与所挂 `book` 的 `source` 不一致；确认 App 端按 `(personId, source)` 定位书的逻辑在 source 异常时失效。
2. **数据修复**：编写脚本遍历 `notes_editor/quote.db` 和 `app/src/main/assets/databases/quote.db`，将 `article.source` 修正为 `toc_entry` 关联的 `book.source`。
3. **工具端防错**：
   - 修改后端 `add` 接口：强制使用所选书的 `source` 作为文章 `source`，前端隐藏 `source` 输入框。
   - 修改后端 `save` 接口：校验修改后的 `source` 必须对应存在的书，否则拒绝保存。
4. **App 端兜底**：
   - 新增 `TocDao.findBookByRef()` 方法，支持按篇目 ID 反查所属书。
   - 修改 `BookshelfRepository.bookKeyOfArticle()` 和 `searchArticles()`：增加三级回退逻辑（人物+出处 → 全局出处 → 目录挂靠反查），且 `bookKey` 统一用书的 `source` 构造。
   - 修改 `ReaderViewModel.loadContent()`：当目录为空但有定位文章时，直接构造单条目目录，避免空白页。

## 输出
文章可正常跳转至阅读页，且无论数据是否遗留异常，阅读页均不会显示空白。

## 注意事项
- 核心原则：文章归属由书决定，而非文章自身字段；App 端定位链路必须具备多级容错能力。
- 修复涉及双副本数据库（校对工具库 + 正式资源库），必须同步更新。
- 前端隐藏 `source` 输入框可避免用户误填导致数据不一致。
