---
title: "本地HTTP服务器端口残留导致批量渲染失败诊断技能"
usage_scenario:
    - "批量转换任务中部分书籍渲染失败或页码缺失"
    - "本地服务启动后后续请求被拒绝或超时"
    - "排查Python脚本中HTTP服务器资源未释放问题"
keywords:
    - "HTTP服务器"
    - "端口残留"
    - "server_close"
    - "批量渲染"
    - "Socket泄漏"
---

## 输入
- 批量转换任务中某本书渲染失败或页码异常（如PDF页数少于SWF文件数）
- 怀疑是本地渲染服务或网络问题

## 步骤
1. **检查文件状态**：统计源文件（SWF/PNG）数量与生成文件（PDF）页数是否匹配
2. **排查日志**：查看批量任务日志，确认失败类型（Timeout/Connection Refused）及发生位置
3. **定位根因**：检查本地HTTP服务器代码，重点排查关闭时是否释放了socket（`srv.shutdown()`后必须调用`srv.server_close()`）
4. **实施修复**：
   - 设置 `allow_reuse_address = False` 禁止端口复用
   - 启动后增加自检（urlopen测试端口可达性）
   - 关闭时显式调用 `server_close()`
5. **验证修复**：重新运行任务，观察后续页面是否正常渲染

## 输出
- 所有书籍的PDF页数与源文件数一致（收付相符）
- 批量任务日志中无新的连接拒绝或超时错误

## 注意事项
### 扩展资源
- Windows环境下HTTP服务器关闭不彻底会导致端口残留，后续请求打到"死socket"上
- 修复核心：`ThreadingHTTPServer.allow_reuse_address = False` + `srv.server_close()`
