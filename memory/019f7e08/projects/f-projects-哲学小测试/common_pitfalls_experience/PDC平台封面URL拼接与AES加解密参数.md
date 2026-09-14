---
title: "PDC平台封面URL拼接与AES加解密参数"
usage_scenario:
    - "对接PDC国家版本数据中心API获取图书封面"
    - "逆向分析PDC前端JS中的加密协议"
keywords:
    - "PDC"
    - "封面URL"
    - "AES-CBC"
    - "加解密参数"
---

PDC平台封面图片URL拼接规则：API返回的image字段仅为长串Hash值，必须拼接 `/a.png` 后缀才能获取真实图片（如 `https://pdcapi.capub.cn/image/<hash>/a.png`）。若直接请求Hash值会返回 'not default image' 文本。（来源：Bash/PDC API）

PDC平台AES加解密参数（硬编码于前端JS）：密钥KEY=`zg35ws76swnxz679`，向量IV=`z66qa18l0w9o521k`，算法为AES-CBC-PKCS7，密文Base64编码后去除 '=' 号。（来源：Bash/PDC API）
