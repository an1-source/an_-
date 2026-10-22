---
name: submit-project
description: 提交项目时使用——用 git commit 并 push 当前项目到 Gitee 仓库 https://gitee.com/an-jiacheng/ajc-high-network.git。触发词：提交项目、push、推送到 gitee、上传代码。
---

# 提交项目（Gitee）

将当前项目提交并推送到 `https://gitee.com/an-jiacheng/ajc-high-network.git`。

## 步骤

1. 确认在项目根目录（`D:\高级网页设计`），检查 git 状态与远程：

   ```powershell
   git status
   git remote -v
   ```

2. 若无 remote，添加 Gitee 远程：

   ```powershell
   git remote add origin https://gitee.com/an-jiacheng/ajc-high-network.git
   ```

3. 暂存所有变更（含新文件）：

   ```powershell
   git add -A
   ```

4. 提交（message 用中文概括本次变更内容；若无变更则跳过 commit）：

   ```powershell
   git commit -m "提交项目：简述变更内容"
   ```

5. 推送。若远程尚无分支或被拒绝，使用 `-u`：

   ```powershell
   git push -u origin HEAD
   ```

   常见分支名冲突时：`git push -u origin HEAD:master` 或先 `git branch -M main` 再推。

## 注意

- 推送需要 Gitee 凭据；若提示认证失败，告知用户输入 Gitee 用户名/密码（或 token）。
- `node_modules/`、`.env`、`*.log` 不应提交：`AI智能化编程/.gitignore` 已覆盖，其余忽略文件按仓库现有规则处理；push 前用 `git status` 确认没有敏感文件被暂存。
- 推送完成后向用户报告 commit hash 与远程分支。
