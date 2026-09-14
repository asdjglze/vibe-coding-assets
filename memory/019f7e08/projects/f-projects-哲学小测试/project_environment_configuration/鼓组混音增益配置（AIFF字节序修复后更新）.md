---
title: "鼓组混音增益配置（AIFF字节序修复后更新）"
usage_scenario:
    - "调试音频输出响度不平衡问题时调整鼓组增益"
    - "验证鼓组音色是否被其他声部掩盖时参考基准增益值"
    - "新声部接入混音时统一增益标尺"
keywords:
    - "鼓组增益"
    - "混音平衡"
    - "drumkit_standard"
    - "音频响度"
---

鼓组混音增益配置：Discord GM Standard Kit 的 AIF 采样为 16-bit 大端序，原始采样峰值较低（kick=0.088, snare=0.053, hi-hat=0.015）。drumkit_standard 增益设置为 5.0（配合 velocity=0.8 时有效增益=4.0），使鼓声峰值达到 kick≈0.35、snare≈0.21、hi-hat≈0.04，可与 violin（peak≈0.65）在混音中形成合理平衡。历史上 gain=0.18 的值是因为 load_wav 用错了字节序（小端序解大端序），导致采样被错误放大到 peak=1.0 而产生的伪基准。
