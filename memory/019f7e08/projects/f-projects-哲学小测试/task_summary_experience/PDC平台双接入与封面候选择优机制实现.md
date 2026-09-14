---
title: "PDC平台双接入与封面候选择优机制实现"
usage_scenario:
    - "接入新的外部数据源（需登录/AES加密/Token管理）"
    - "实现基于候选池的 AI 择优功能（如封面/图片/方案选择）"
    - "重构现有归档流程以集成多源数据"
keywords:
    - "PDC接入"
    - "封面择优"
    - "AES逆向"
    - "候选池"
    - "Token管理"
---

## 任务描述
接入中国国家版本数据中心（PDC）作为中图分类参考源与高清封面数据源，并实现封面候选择优机制。

## 执行过程
```mermaid
graph TD
    A[需求: PDC接入与封面择优] --> B[调研PDC登录与接口]
    B --> C[逆向前端JS获取AES密钥/IV及noneStr签名]
    C --> D[实现pdc_site.py: token管理/AES加解密/检索/封面下载]
    D --> E[设计cover_pool.py: 候选池/序号/去重/上限控制]
    E --> F[设计cover_select.py: fill_gaps补漏/ai_pick择优/落盘]
    F --> G[修改agent.py: 新增pdc_db工具/view_images入池]
    G --> H[修改recognizer.py: book_api入池/识别开场入池]
    H --> I[修改organizer.py: 归档时调用ai_pick/清池]
    I --> J[联测验证: PDC检索/封面下载/候选池/择优容错]
```

## 任务总结
1. **PDC接入**：实现 AES-CBC 协议逆向（key=`zg35ws76swnxz679`, iv=`z66qa18l0w9o521k`），支持 token 自动管理与失效人工指引；提供 `pdc_db` 工具供 AI 查询中图分类号与元数据。
2. **封面择优**：建立 5 个收集点（PDC/book_api/view_images/程序截页/补漏），自动编号暂存；归档时触发单独 AI 会话择优（≥2候选），失败或无候选则落原兜底链。
3. **验证**：PDC 检索《活着》得分 85，封面原图下载正常，编译全过。
