---
title: "图书分类Agent工具集与提示词配置"
usage_scenario:
    - "新增或修改Agent可用工具时"
    - "排查AI为何未使用特定工具或不知晓某工具用法时"
    - "审查提示词是否完整覆盖所有程序注册的工具"
keywords:
    - "工具清单"
    - "系统提示词"
    - "工具调用"
    - "Agent能力"
---

图书分类Agent工具集（9个工具+5个控制动作）及提示词配置：
1. 搜索取证类：search_web(联网搜证据)、fetch_page(抓网页全文)、book_api(微信读书/豆瓣核实正式出版信息)、look_calibre(查本地Calibre书库作辅助参考)、read_slice(截取正文判断体裁主题)、view_images(提取图片判封面题材)。
2. 目录导航类：get_children(获取当前层候选类目，decide_level定类号必须从中选择)、list_tree(递归展开官方子树用于开卷勘察和下钻预览，只读不改状态)。
3. 上下文管理类：summarize(长文本压缩防超限)。
4. 控制动作：decide_level(逐层定类号)、reset_level(走错回退)、final(交卷)、manual(中途退出)、refuse(拒绝输出)。所有工具说明均作为system prompt每轮注入AI。
