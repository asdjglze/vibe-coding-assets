---
title: "图书分类Agent置信度与留人工语义规范"
usage_scenario:
    - "图书分类Agent输出结果校验"
    - "调试Agent因置信度低导致的空转或错误拦截"
    - "定义Agent动作语义（final/manual/refuse）"
keywords:
    - "置信度"
    - "留人工"
    - "refuse"
    - "manual"
    - "Agent协议"
---

## 输入
- 图书编目 Agent 的输出结果（含 confidence、书名、责任者、分类路径）

## 步骤
1. **判定 confidence**：仅作为质量标注（身份确认程度），不影响程序是否接受 final。
   - high：元数据确凿；medium：个别字段不确定；low：信息少但可用（如书名有、作者不明）。
2. **判定 final 最低门槛**：检查书名 + 责任者（author/editor/translator/illustrator/other_contributors）。
   - 若全缺 → 视为【信息完全缺失】，打回并要求 AI 二选一：继续取证 或 输出 `refuse`。
   - 若有任一非空 → 进入分类校验。
3. **判定分类完成度**：检查类号合法性与层深。
   - 合法且完整 → 接受归档。
   - 非法/缺类号/层深不足 → 打回要求修正。
4. **处理留人工请求**：
   - `manual`（中途退出）：搞到一半发现干不了（损坏/加密），立即结束。
   - `refuse`（拒绝输出）：干完了仍找不到书名与责任者，拒绝不负责任地交卷。

## 输出
- 正常归档入库（保留 confidence 值）
- 或触发打回（反馈具体原因：信息缺失/分类未完成）
- 或留人工（标记为 manual/refuse 及原因）

## 注意事项
- **严禁将 low 视为不分类的理由**：没有 CIP/现成证据是常态，AI 必须按内容实质自主分类。
- **区分三种状态**：low（可用）、manual（中途退出）、refuse（拒绝输出）。
- **编者算责任者**：无作者时，editor/编委会等均可担当责任者角色。
- **来源工具**：agent.py, agent_system.md
