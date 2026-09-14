---
title: "Android资源文件名非法字符与扩展名限制"
usage_scenario:
    - "Android项目编译报resource name character无效错误"
    - "处理头像/图标资源时遇到打包失败"
keywords:
    - "Android"
    - "资源命名"
    - "drawable-nodpi"
    - "打包错误"
---

Android资源文件名限制：1. 只能包含小写字母a-z、数字0-9和下划线_，括号()等字符会导致打包报错`Error: '(' is not a valid file-based resource name character`；2. 图片仅支持.jpg/.png/.gif/.webp，不支持.jpeg扩展名。Windows复制文件自动生成的副本如avatar_xxx(1).jpg也是非法名，需删除或重命名。（来源：Bash）
