---
title: "Android项目自动化构建脚本编写技能"
usage_scenario:
    - "需要为Android项目创建一键构建脚本"
    - "在CI/CD环境中配置Android构建流程"
    - "向非开发者分发可执行的构建脚本"
keywords:
    - "Android构建"
    - "批处理脚本"
    - "环境检查"
    - "Gradle"
---

## 输入
- Android项目根目录路径
- 目标编译配置（如 compileSdk 版本）

## 步骤
1. **Java环境检查**：优先使用 Android Studio 自带的 JBR (`C:\Program Files\Android\Android Studio\jbr`)，若未定义 JAVA_HOME 则自动设置。
2. **SDK Platform检查**：根据目标 compileSdk 版本（如 android-37），在 SDK 目录（`%LOCALAPPDATA%\Android\Sdk` 等）中检查对应 `platforms` 文件夹是否存在。若缺失，提示用户在 Android Studio SDK Manager 中安装。
3. **执行构建**：调用 `gradlew.bat assembleDebug --console=plain` 进行编译。
4. **结果反馈**：成功时输出 APK 路径；失败时列出常见原因（如镜像同步延迟、SDK 缺失）并暂停等待用户处理。

## 输出
- 成功：生成 debug APK 并打印路径。
- 失败：打印错误日志及排查建议。

## 注意事项
- Windows环境下建议使用 `.bat` 脚本封装，利用 `chcp 65001` 解决中文乱码。
- 首次构建会下载 Gradle 和依赖，需预留时间。
- 针对特定 API 级别（如 alpha 版或最新正式版），务必在构建前校验 SDK 是否已安装。
