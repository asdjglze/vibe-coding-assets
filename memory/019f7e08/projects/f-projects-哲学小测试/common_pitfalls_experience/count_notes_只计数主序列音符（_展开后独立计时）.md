---
title: "_count_notes 只计数主序列音符（<> 展开后独立计时）"
usage_scenario:
    - "解析器对 <X>Y 格式返回段数为 0 而非预期 1"
    - "音乐播放中鼓组段数与其他声部不一致，怀疑 _count_notes 计数异常"
keywords:
    - "_count_notes"
    - "和弦段计数"
    - "尖括号解析"
    - "play_score.py"
---

`_count_notes` 当前只计数主序列（`>` 后的音符），不计入 `<>` 内音符。`<>` 块通过 `_expand_bracket_lines` 展开为独立内部行，各自独立计时和渲染。
