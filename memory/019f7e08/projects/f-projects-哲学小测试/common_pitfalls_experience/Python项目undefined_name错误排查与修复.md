---
title: "Python项目undefined name错误排查与修复"
usage_scenario:
    - "运行Python脚本报name 'xxx' is not defined"
    - "排查Python项目中未导入却使用的变量或函数"
    - "批量检查多模块代码中的潜在NameError"
keywords:
    - "pyflakes"
    - "undefined name"
    - "import缺失"
    - "Python调试"
---

在Python项目中，使用`undefined name`错误（如`name 'xxx' is not defined`）通常是因为代码中引用了变量或函数但未在头部import。排查和修复此类潜伏Bug的标准流程：1. 安装并使用`pyflakes`进行静态分析：`python -m pyflakes src\*.py`；2. 筛选输出中的`undefined name`行；3. 检查对应文件的import区域，补全缺失的import语句（如`import json`或`from src import xxx`）。来源：Bash (pyflakes)
