---
name: submit-github
description: 提交项目到 GitHub（SSH）使用。触发词：提交github、推送到github、github、push到github。不用于推送到 Gitee。
---

# 提交项目到 GitHub（SSH）

将当前项目提交并推送到 `git@github.com:an1-source/an_-.git`。SSH 公钥已添加到 GitHub：`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIER5JjfczlSXOTVZpZvCiOR7v8JaKAo7x6yDPN6LIPzV`。

## 步骤

1. 确认在项目根目录（`D:\高级网页设计`），检查 git 状态与远程：

   ```powershell
   git status
   git remote -v
   ```

2. **不要覆盖已有的 origin（origin 指向 Gitee）**。使用一个新的 remote 名称，例如 `github`：

   - 若不存在，添加：
     ```powershell
     git remote add github git@github.com:an1-source/an_-.git
     ```
   - 若已存在但 URL 不对，修改：
     ```powershell
     git remote set-url github git@github.com:an1-source/an_-.git
     ```

3. 暂存所有变更（含新文件）：

   ```powershell
   git add -A
   ```

4. 提交（message 用中文概括本次变更内容；若无变更则跳过 commit）：

   ```powershell
   git commit -m "提交到 GitHub：简述变更内容"
   ```

5. 推送到 `github` remote（注意是 `github` 不是 `origin`）：

   ```powershell
   git push github HEAD
   ```

   若远程尚无分支，使用：

   ```powershell
   git push -u github HEAD
   ```

   常见分支名冲突时：`git push -u github HEAD:master` 或先 `git branch -M main` 再推。

## 注意

- **不要改动 origin**：origin 仍指向 Gitee `https://gitee.com/an-jiacheng/ajc-high-network.git`，`github` 是第二个 remote。用户同时维护两个远程。
- SSH 公钥已配置，首次 push 若提示主机指纹确认，输入 `yes` 即可；若仍报 `Permission denied (publickey)`，告知用户检查 `ssh -T git@github.com` 是否连通。
- 推送前用 `git status` 确认 `node_modules/`、`.env`、`*.log` 等敏感文件未被暂存。
- 推送完成后向用户报告 commit hash 与远程分支（`github/master` 或 `github/main`）。
