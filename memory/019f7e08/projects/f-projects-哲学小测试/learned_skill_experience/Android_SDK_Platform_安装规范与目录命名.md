---
title: "Android SDK Platform 安装规范与目录命名"
usage_scenario:
    - "需要离线或快速安装特定版本的 Android SDK Platform"
    - "解决因 SDK 目录名不匹配导致的构建错误"
    - "在 CI/CD 环境中自动配置 Android 构建依赖"
keywords:
    - "Android SDK"
    - "AGP"
    - "目录命名"
    - "ApiLevel"
    - "构建配置"
---

## 输入
- Android SDK 目录路径（默认 `C:\Users\<用户名>\AppData\Local\Android\Sdk`）
- 目标 API 版本（如 API 37）

## 步骤
1. **下载**：从 Google 官方仓库下载对应版本的 platform zip 包（如 `platform-37.0_r02.zip`）。
2. **校验**：计算文件 SHA1 哈希值，确保完整性。
3. **解压**：将 zip 包解压到临时目录。
4. **目录命名检查**：**关键步骤**。解压后的文件夹名称必须严格匹配 source.properties 中的 ApiLevel。若源名为 `android-37.0`，则**必须保留**该名称；若为 `android-37`（缺失小数部分），需将其重命名为 `android-37.0` 以符合 AGP 规范。
5. **安装**：将处理好的文件夹移动到 `<SDK_DIR>/platforms/` 目录下。
6. **验证**：确认 `platforms/<version>/android.jar` 存在。

## 输出
SDK 平台文件安装成功，可被构建工具识别。

## 注意事项
- **目录名一致性**：AGP 严格要求目录名与 ApiLevel 完全一致（例如 `android-37.0`）。切勿将带小数的正确名称（`android-37.0`）误删后缀改为 `android-37`，否则会导致构建极慢或报错。
- **网络问题**：若无法访问 `dl.google.com`，需配置代理或使用镜像源。
- **权限**：Windows 下写入 SDK 目录可能需要管理员权限。
