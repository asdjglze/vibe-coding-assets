---
title: "引注显式建链机制与quote链接渲染"
usage_scenario:
    - "修改阅读器注释弹窗渲染逻辑"
    - "处理注释内容中引注跳转需求"
    - "修改notes_editor标题注释按钮交互"
keywords:
    - "嵌套引注"
    - "FootnoteDialog"
    - "叠加弹窗"
    - "noteSegs"
    - "标题注释"
---

毛语录引注采用「校对阶段显式建链」机制：notes_editor 的「建引用」按钮——用户选中文字后弹窗选择目标文章（搜索框，候选来自 state.tree 全量文章）与注释键（api/get 加载目标文章后 parseDefs 提取注释列表），确认后选区替换为 `[文字](quote:文章id#注号)` Markdown 链接；注码扫描（collectTags/collectDefTags/scan）经 maskQuoteLinks 把 quote 链接整体遮蔽避免误伤链接内文字；预览中 quote 链接渲染强调色，点击经 pvBody 拦截查 api/get 显示目标注释。Android 端：MdParser 的 Link span（CommonMark 解析）url 以 quote: 开头时 mdAnnotated 渲染为强调色可点击；点击经 ReaderViewModel.quoteNote 解析 quote:(\d+)#(.+) → BookshelfRepository.articleFootnote（按 id 取文章 MdParser.parse 后 footnotes[key]）→ FootnoteDialog 递归叠加弹窗显示（sub 状态叠层逐层关闭）。MdArticleBody/MdSubtitleHeader/MdTitle/FootnoteDialog 均带 onOpenQuote 参数。禁止任何文本识别/猜测跳转目标。
