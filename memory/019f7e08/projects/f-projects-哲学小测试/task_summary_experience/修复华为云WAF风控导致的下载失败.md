---
title: "修复华为云WAF风控导致的下载失败"
usage_scenario:
    - "解决目标网站触发WAF/验证码导致脚本中断的问题"
    - "修复因服务器返回非预期内容（如HTML而非图片）导致的解析错误"
keywords:
    - "华为云WAF"
    - "风控拦截"
    - "自动退避"
    - "限速"
    - "脏包清理"
---

## 任务描述
修复电子书批量下载工具（driver_v2.py）因华为云WAF风控导致的‘服务器坏页’误判及老书入口URL拼接错误。

## 执行过程
```mermaid
graph TD
    A[诊断: 7904字节响应实为WAF验证页] --> B[定位 probe_pages 二分法被WAF破坏]
    B --> C[修改 stat/fetch1: 识别 WAF 特征并自动退避重试]
    C --> D[定位 fetch_legacy URL 拼接 bug (多斜杠)]
    D --> E[修改 dl_legacy: 拆分 base 目录与入口文件名]
    E --> F[全局限速 Gate + WAF 污染回滚机制]
    F --> G[清理 64 个受损脏包并重跑]
```

## 任务总结
成功识别华为云 WAF 挑战页（HuaweiCloudWAF / Access Verification），并在 driver_v2.py 和 batch_download.py 中实现了全局限速（0.45s间隔）与 WAF 自动退避（暂停40s+放大间隔）。修复了老书入口 URL 拼接错误，清除了所有被 WAF 污染的脏材料，大幅提升了下载成功率。
