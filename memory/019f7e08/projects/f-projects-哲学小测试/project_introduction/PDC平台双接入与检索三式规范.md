---
title: "PDC平台双接入与检索三式规范"
usage_scenario:
    - "新增或调整外部标准数据源接入逻辑时"
    - "排查接口Token失效或反爬拦截问题时"
keywords:
    - "PDC"
    - "Token维护"
    - "中图分类"
    - "双路接入"
---

PDC（国家版本数据中心）接入规范（2026-09-11 升级）：
1. AI 工具 pdc_db 三式：①{"query":"书名 作者"}→前 10 条列表（带序号+作者筛选可选值，供 AI 自行判断目标）②带 filter 筛选搜索（zz 作者/ztc 主题词/publisher 出版单位，纯文本；cate 中图分类；publishingStart+End 出版年；ztype 数据类型）③{"detail":序号}读完整详情（副书名 subbookname/定价/摘要/版权页/封面大图 800x800）。
2. 个人账号铁限：只有第 1 页 10 条（pageNum>1 返回空）；目标被挤出时用筛选把目标挤进前 10。
3. 列表与详情分离：列表先给 AI 看，详情按需读（bookDetail2 接口 82 字段）。
4. 筛选项清单：rangeType 接口返回 {word,count}，7 天缓存（state/pdc_range_cache.json）省访问。
5. 封面：检索/详情命中时顺带存入候选池。
6. Token 失效：人工过滑块续期（并行路线，零等待快跳，详见专项记忆）。
