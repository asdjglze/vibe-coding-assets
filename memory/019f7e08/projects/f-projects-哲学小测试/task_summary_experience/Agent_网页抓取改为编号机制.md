---
title: "Agent 网页抓取改为编号机制"
usage_scenario:
    - "优化 Agent 工具交互以减少 Token 消耗和错误率"
    - "实现本地内存映射替代外部 URL 传递的架构模式"
    - "排查 fetch_page 工具无法定位目标网页的问题"
keywords:
    - "Agent 工具"
    - "编号机制"
    - "URL 映射"
---

## 任务描述
优化 Agent 工具链，将网页抓取从"AI 输入长 URL"改为"AI 输出数字编号"，解决 URL 易错、浪费 Token 的问题。

## 执行过程
```mermaid
graph TD
    A[需求:移除 AI 对 URL 的直接依赖] --> B[诊断:原 search_web 返回无链接或链接难用]
    B --> C[设计:引入内存 url_map 与全局递增 ID 机制]
    C --> D[修改_search_tool:生成[#N]编号结果并存入ctx[url_map]]
    D --> E[修改_fetch_page_tool:优先解析id参数从内存取URL]
    E --> F[更新提示词:指导AI使用{id}而非url调用fetch_page]
    F --> G[编写单元测试:验证跨批次ID递增/内存映射/异常反馈]
```

## 任务总结
成功实现 URL 编号化机制：`search_web` 返回 `·[#0] 标题 + 片段`，链接存入 `ctx['url_map']`；`fetch_page` 支持 `{'id': N}` 参数，自动从内存获取 URL 抓取。AI 仅需输出数字即可抓取正文，避免抄错 URL 且节省 Token。每本书独立计数，跨多次搜索保持唯一性。
