---
title: "SFZ音名字段（pitch_keycenter/lokey）解析支持"
usage_scenario:
    - "SFZ加载报ValueError: invalid literal for int() with base 10: 'C4'"
    - "SFZ中使用科学音名定义keycenter导致解析失败"
    - "需要支持音名输入的SFZ解析器"
keywords:
    - "SFZ"
    - "音名解析"
    - "pitch_keycenter"
    - "note_to_midi"
---

SFZ文件中`pitch_keycenter`或`lokey`字段使用音名（如'C4'）而非MIDI数字时，`Region`构造函数会因`int('C4')`报`ValueError: invalid literal for int()`。解决方案：新增`note_to_midi(note_str)`函数（支持'C4'/'F#3'/'Db4'等格式），并在`Region.__init__`中调用该函数解析音名，fallback至`_safe_int`（来源：Bash + SearchReplace）
