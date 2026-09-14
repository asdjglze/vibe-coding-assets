---
title: "站点数据查询撤走自动流程改为AI工具std_site"
usage_scenario:
    - "修改站点查询或富化逻辑时"
    - "调整AI工具集与会话能力时"
    - "核对快通道联网边界时"
keywords:
    - "站点查询"
    - "std_site"
    - "工标网"
    - "AI工具"
    - "预调用"
    - "快通道纯本地"
---

站点数据查询（工标网/openstd/国标图集站/gb99.cn）的形态规范（2026-09-09 主人定案）：
1. 不做任何自动批量流程（原 run.py 站点富化段已撤走，enrich_site 模块保留备用）；
2. AI 之前预调用保留（精修链 try_program_classify：GB 书先查工标网取 CCS，规范固定化命中率高，AI 不必重搜）；
3. 新增 AI 工具 std_site（src/agent.py）：多轮工具会话（编目/识别 Agent）按需调用，复用 std_sites.lookup_std（含 SiteCache 缓存与站级限速，与预调用共享缓存）；lookup_std 原有源未达 80 分时统一兜底补查 gb99.cn（25 万+ 数据量，URL 形式 /std/GB-T-22522-2021，未收录页靠字段校验识别）；
4. 快通道保持纯本地零联网（直接归档、缺数据留空、挂标待 AI）。
注：规范链三个单轮 chat 会话（judge_type/classify/atlas_id）无工具调用能力，其站点数据依赖预调用供给。
