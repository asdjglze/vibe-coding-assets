---
title: "无 gh CLI 的 GitHub 发布流程（凭据管理器+API）"
usage_scenario:
    - "开源或发布项目到GitHub"
    - "自动化创建仓库与Release"
    - "凭据验证失败改用匿名API"
keywords:
    - "GitHub API"
    - "credential fill"
    - "建仓推送"
    - "Release资产"
    - "无gh发布"
---

本机未安装 gh CLI 时，可经 git credential fill 从 Windows 凭据管理器取出 github.com 凭据（不落盘不回显），配合 GitHub REST API 完成全流程：建仓（POST /user/repos）、推送（git push 使用内嵌 x-access-token 的临时 URL，完成后将 origin 改回干净地址）、创建 Release（POST /releases）、上传资产（POST upload_url）。要点：公开仓库的发布终验改用匿名只读 API（无需凭据，可避免凭据偶发为空导致的误报）。
