---
title: "Android头像资源批量更新与一致性校验"
usage_scenario:
    - "Android项目中批量替换/更新res/drawable下的图片资源"
    - "处理带有重复后缀(如(1))的文件重命名任务"
    - "确保代码中资源ID引用与物理文件严格一致的校验流程"
keywords:
    - "Android资源更新"
    - "批量重命名"
    - "Python脚本"
    - "资源一致性校验"
---

## 任务描述
更新 Android 项目中的头像资源文件，包括替换旧图、新增头像，并确保文件名与代码引用一致。

## 执行过程
```mermaid
graph TD
    A[需求:更新头像资源] --> B[扫描 drawable-nodpi 目录获取最新文件列表]
    B --> C[编写 Python 脚本 _tmp_avatar_rename.py]
    C --> D[第一步: 删除被替代的旧版本文件 (jpg/gif/png)]
    D --> E[第二步: 将带 (1) 后缀的新文件重命名为标准 avatar_xxx 格式]
    E --> F[编写 Python 脚本 _tmp_avatar_verify.py]
    F --> G[读取 Theme.kt 提取 R.drawable 引用]
    G --> H[比对引用集合与目录文件集合，确保零缺失零多余]
    H --> I[执行 build_pack.ps1 重新打包 APK]
```

## 任务总结
成功完成头像资源更新：删除 65 个旧文件，重命名 56 个新文件。通过 Python 脚本交叉验证了代码中的 60 个资源引用与磁盘上的 60 个文件完全匹配，无遗漏或冗余，并完成了重新打包。
