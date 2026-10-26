---
name: submit-github
description: 提交项目到 GitHub（SSH-over-443）使用。触发词：提交github、推送到github、github、push到github。不用于推送到 Gitee。
---

# 提交项目到 GitHub（SSH-over-443）

将当前项目提交并推送到 `ssh://git@ssh.github.com:443/an1-source/an_-.git`（使用 443 端口穿墙，避免 22 端口被墙）。

SSH 公钥已添加到 GitHub：`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIER5JjfczlSXOTVZpZvCiOR7v8JaKAo7x6yDPN6LIPzV`。

## 步骤

1. 确认在项目根目录（`D:\高级网页设计`），检查 git 状态与远程：

   ```powershell
   git status
   git remote -v
   ```

2. **不要覆盖已有的 origin（origin 指向 Gitee）**。使用一个新的 remote 名称 `github`，URL 用 443 端口穿墙格式：

   - 若不存在，添加：
     ```powershell
     git remote add github ssh://git@ssh.github.com:443/an1-source/an_-.git
     ```
   - 若已存在但 URL 不对，修改：
     ```powershell
     git remote set-url github ssh://git@ssh.github.com:443/an1-source/an_-.git
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

   > **重要**：首次推送或主机指纹未信任时，用以下带 `StrictHostKeyChecking=no` 的命令绕过交互确认；否则非交互环境会因 `Host key verification failed` 失败。
   > 若用户环境已在 `~/.ssh/config` 配好 Host github.com → HostName ssh.github.com + Port 443，可直接用 `git push github HEAD`。

   ```powershell
   $env:GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"; git push -u github HEAD
   ```

## 注意

- **不要改动 origin**：origin 仍指向 Gitee `https://gitee.com/an-jiacheng/ajc-high-network.git`，`github` 是第二个 remote。用户同时维护两个远程。
- **22 端口通常被墙**，必须走 443 端口的 SSH-over-TLS：`ssh://git@ssh.github.com:443/...` 或在 `~/.ssh/config` 里把 github.com 映射到 ssh.github.com 并指定 Port 443。
- Windows 非交互环境 `ssh` 无法弹密码输入框，如果私钥有 passphrase 需要先 `ssh-add`；若裸跑失败，先试 `$env:GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"`。
- 推送前用 `git status` 确认 `node_modules/`、`.env`、`*.log` 等敏感文件未被暂存。
- 推送完成后向用户报告 commit hash 与远程分支（`github/master` 或 `github/main`）。
