---
title: "封面候选池与AI择优机制实现规范"
usage_scenario:
    - "修改封面收集或归档链路时"
    - "排查封面选择/候选池问题时"
    - "新增封面数据源接入时"
keywords:
    - "封面候选池"
    - "封面择优"
    - "cover_pool"
    - "ai_pick"
    - "收集点"
    - "序号机制"
---

图书整理流水线的封面候选池与 AI 择优机制（2026-09-09 用户定案）：

1. 流程：任何接口/程序拿到封面时顺带收集 → 暂存候选池（state/cover_pool/<md5(main_file)[:12]>/pool.json，自动分配序号 seq，md5 去重，上限总数 8/单来源 3）→ 其他工序都定了之后 → 归档时单独一次 AI 会话择优（带序号1234选择）。
2. 五个收集点：① pdc_db 检索封面 ② book_api（微信读书/豆瓣 _api_book_ref 带 pool_key） ③ view_images 程序提取 ④ 识别开场 initial_images（pdf→程序截页/否则程序提取） ⑤ fill_gaps 归档补漏（PDF 前2页渲染/内嵌提取/local cover.jpg）。
3. 择优落点：organizer.organize_one（识别链与精修链共用；快通道 quick_publish 不经 organizer，保持纯本地不受影响）。
4. ai_pick 规则：候选≥2 才烧 AI（cover_select.ai_pick，缩略图+序号清单→{"seq","reason"}）；候选=1 直接采用；AI 无效输出退回第一候选；异常返 None 落原 ensure_cover 兜底链。归档后 cover_pool.clear 清池。
5. 涉及模块：src/cover_pool.py、src/cover_select.py、src/pdc_site.py（新）；agent.py/recognizer.py/douban_meta.py/organizer.py/skills/agent_system.md（改）。
