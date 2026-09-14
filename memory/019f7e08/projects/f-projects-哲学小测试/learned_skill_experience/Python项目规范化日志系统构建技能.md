---
title: "Python项目规范化日志系统构建技能"
usage_scenario:
    - "需要为Python项目添加结构化日志功能"
    - "排查复杂错误时需要完整的调用链和上下文信息"
    - "调试AI Agent时记录模型输入输出以便分析"
keywords:
    - "Python日志"
    - "Traceback"
    - "Print镜像"
    - "LogRecord"
    - "Python3.12"
---

## 输入
- Python项目代码
- 需求：日志按当前日期分文件、终端print镜像到日志并带定位、异常/LLM调用全量记录

## 步骤
1. **创建统一日志模块**：使用logging.Logger("ztfl")，配置DailyFileHandler实现按当天日期自动切分日志文件。
2. **Patch全局Print**：替换builtins.print为自定义logged_print。原样输出到终端，同时通过sys._getframe(1)获取调用者位置，构造LogRecord写入日志文件（注意：只写文件handler，避免终端重复显示）。
3. **异常全量记录**：
   - 未捕获异常：注册sys.excepthook和threading.excepthook，将完整traceback写入日志文件。
   - 已捕获异常：封装log_exception函数，利用stacklevel=2定位到业务代码行，记录完整traceback。
4. **AI交互记录**：在LLM客户端入口拦截，记录system/history/prompt/images及完整回复，标注业务调用点。

## 输出
- logs/YYYY-MM-DD.log 包含所有操作细节、精准定位信息和完整报错堆栈

## 注意事项
- **Python 3.12兼容性**：严禁使用`_log.log(..., extra={'filename':...})`，会报KeyError。必须直接构造LogRecord对象再调用`_log.handle(rec)`或`_file_handler.emit(rec)`。
- **防重复**：print镜像和异常hook应只写入文件handler，不要上console handler，否则终端会显示两遍。
