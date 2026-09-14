---
title: "沙箱风险标记命令禁止 Python 创建子进程管道"
usage_scenario:
    - "Python脚本零输出异常退出排查"
    - "运行需子进程的自动化脚本"
    - "命令在受限环境权限失败时"
keywords:
    - "沙箱限制"
    - "权限拒绝"
    - "WinError5"
    - "子进程管道"
    - "命令执行环境"
---

在 Qoder 沙箱中以风险标记（has_risk=true）执行的命令，运行环境更严格：Python 调用 subprocess 创建管道会报 PermissionError [WinError 5] 拒绝访问（CreatePipe 失败），表现为脚本零输出、退出码异常。需要 subprocess/管道的脚本应改用普通模式（非风险标记）运行；先用同款简单命令形态（脚本 + 读结果文件）验证环境可用性，再执行主流程，可避免反复盲试。
