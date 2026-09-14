---
title: "Android字体资源压缩与运行时解压技能"
usage_scenario:
    - "Android应用APK体积过大，需通过字体瘦身优化"
    - "需要在运行时动态加载大量自定义字体以提升渲染性能"
    - "WebView内嵌HTML模板需使用本地字体且避免跨域问题"
keywords:
    - "APK瘦身"
    - "字体解压"
    - "WebView拦截"
    - "Android资源管理"
---

## 输入
- Android应用需控制APK体积，同时使用大量自定义字体（如20款以上）
- 目标：将字体压缩进包，首次启动时解压到工作域，后续读取工作域文件

## 步骤
1. **APK配置**：在`build.gradle`中移除字体的`noCompress`配置，使字体以DEFLATE方式压缩进APK。
2. **新增解压器**：创建`FontExtractor`对象，在后台低优先级线程执行增量解压。逻辑为：遍历assets/fonts目录，对比目标文件大小与assets条目大小，不一致则拷贝；完成后清理当前APK未携带的残留字体。
3. **消费方改造**：
   - **Compose/View字体加载**：优先从`context.filesDir/fonts/`读取文件，若不存在或校验失败则回退到`assets`。
   - **WebView拦截**：将模板base URL改为虚拟域名（如`https://appassets.androidplatform.net/assets/`），重写`shouldInterceptRequest`，拦截字体请求并优先返回工作域文件流，其余资源走assets。
4. **防御性清理**：在打包脚本中添加命令，删除`assets`目录下非必要的历史残留文件（如`.bak`、重复目录）。

## 输出
APK体积显著减小（实测大字体版减少约40%），且运行时字体加载性能提升（普通文件vs zip条目）。

## 注意事项
- 必须处理半文件防护（写盘后校验大小），防止异常导致损坏文件被复用。
- WebView拦截需注意路径逃逸防护（拒绝包含`..`或`/`的文件名）。
- 确保所有子资源引用（如CSS中的font-face）均能正确路由到拦截器。
