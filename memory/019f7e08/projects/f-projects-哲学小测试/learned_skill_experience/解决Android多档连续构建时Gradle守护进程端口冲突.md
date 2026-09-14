---
title: "解决Android多档连续构建时Gradle守护进程端口冲突"
usage_scenario:
    - "Android项目多档位（如不同字体/主题）连续打包时出现BindException"
    - "Gradle构建报FileLockContentionHandler或Address already in use错误"
    - "排查Gradle守护进程残留导致的构建失败"
keywords:
    - "Gradle"
    - "BindException"
    - "守护进程"
    - "端口冲突"
---

## 输入
- Android项目多档位（如不同字体配置）连续打包脚本
- 报错 `java.net.BindException: Address already in use: bind` 或 `Could not create service of type FileLockContentionHandler`

## 步骤
1. 诊断环境：检查是否有残留的 java/javaw 进程占用端口（`Get-CimInstance Win32_Process -Filter "Name like '%java%'"`）
2. 优雅停止：每档构建前执行 `gradlew.bat --stop`（PowerShell 中需 `cmd /c "gradlew.bat --stop >nul 2>&1"` 包装，否则 stderr 触发 NativeCommandError）
3. 关键：固定等待不可靠——旧 daemon 退出后端口释放缓慢（Windows 最长约 120 秒，实测 --stop 后等 20 秒仍 BindException，即使无 java 进程残留）。必须改为失败自动重试循环：构建失败后等待约 15 秒重试，最多 6 次，成功即跳出
4. 验证：重试后构建 BUILD SUCCESSFUL，再继续下一档

## 输出
三档 APK 全部构建成功

## 注意事项
- 该错误由上一个守护进程退出延迟导致，而非真正的端口被其他软件占用
- 固定等待时间（5 秒、20 秒）均不可靠，自适应重试循环才能覆盖端口释放时长波动
[This memory was created recently and may be too specific. Check whether it applies to the current scenario; if not, generalize the content and update usage_scenario.]
