---
title: "Python Lock不可重入导致死锁陷阱"
usage_scenario:
    - "Python多线程编程中使用Lock进行资源保护"
    - "排查程序无响应或卡死问题"
    - "重构涉及锁的代码逻辑"
keywords:
    - "Python"
    - "Lock"
    - "死锁"
    - "不可重入"
---

Python threading.Lock 是不可重入锁，在持有锁的代码块内禁止调用其他需要获取同一把锁的方法，否则会导致死锁。例如 search_quota.py 中 status() 方法内部直接调用了 is_cooling()，而 is_cooling() 也会尝试获取 self._lock，从而引发死锁。修复方案：在 status() 的 with self._lock 块内直接计算冷却状态（如比较时间戳），避免嵌套调用锁方法。（来源：Bash/Python）
