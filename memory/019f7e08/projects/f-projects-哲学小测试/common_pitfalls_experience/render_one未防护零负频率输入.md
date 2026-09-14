---
title: "render_one未防护零/负频率输入"
usage_scenario:
    - "调用render_one传入freq=0或负数导致崩溃或静音异常"
    - "自动化测试中因频率生成逻辑错误传入非法值而失败"
    - "合成器接口暴露给用户时需防御性编程"
keywords:
    - "render_one"
    - "零频率"
    - "边界防护"
    - "freq<=0"
---

SFZ引擎`render_one`方法未处理`freq <= 0`边界情况，直接计算会导致`step=0`引发异常输出。修复：在函数开头添加`if freq <= 0: return [0.0] * max(1, int(sample_rate * dur))`。（来源：sfz_engine.py 第478行）
