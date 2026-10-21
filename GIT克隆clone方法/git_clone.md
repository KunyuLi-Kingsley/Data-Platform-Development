# Git 克隆与分支切换操作指南（Windows）

本文记录如何在 Windows 本地使用 Git 克隆 GitLab 仓库、通过 Personal Access Token（个人访问令牌）完成 HTTPS 身份验证、查看和切换分支，并使用 VS Code 打开项目。最后介绍如何把**自己有权公开或分享的项目/文档**推送到个人 GitHub。

> 安全提醒
>
> - 不要把密码、Personal Access Token、SSH 私钥、`.env` 文件或公司机密上传到 GitHub。
> - 如果 Token 曾经出现在截图、聊天记录或公开仓库中，并且仍然有效，请在 GitLab 中撤销它并创建新 Token。
> - 公司 GitLab 项目可能属于公司资产。推送前先确认公司政策、代码版权、保密义务及仓库可公开性。不要把公司源代码复制到个人 GitHub，除非已获明确授权。
> - 本文中的仓库地址、目录和分支名均为示例；请替换为你有权限访问的实际值。

## 一、准备工作

1. 安装 Git for Windows。
2. 安装 Visual Studio Code（VS Code）。
3. 确认自己拥有 GitLab 仓库的访问权限。
4. 在 GitLab 中准备 HTTPS 身份验证方式。若使用 Personal Access Token，请只在 Git 提示输入密码时粘贴，不要把 Token 写进命令、脚本或文档。

检查 Git 是否安装成功：

```bat
git --version
```

如果命令提示找不到 `git`，请安装 Git for Windows，并重新打开 CMD。

## 二、新建本地文件夹并打开 CMD

1. 在电脑上新建一个用于存放项目的文件夹，例如：

   ```text
   D:\SAP工作\s4_BI
   ```

2. 在文件资源管理器中打开这个文件夹。
3. 点击地址栏，输入 `cmd`，然后按 Enter。
4. CMD 会在当前文件夹路径下打开。

也可以手动打开 CMD，然后使用 `cd` 进入目标目录：

```bat
cd /d "D:\SAP工作\s4_BI"
```

`/d` 可以同时切换盘符和目录。路径中包含空格时，请保留双引号。

## 三、从 GitLab 克隆仓库

在 GitLab 项目页面找到 **Clone** / **Code** 按钮，复制 HTTPS 克隆地址，然后执行：

```bat
git clone <GitLab仓库的HTTPS地址>
```

例如（仅为格式示例）：

```bat
git clone http://gitlab.example.com/group/project.git
```

如果公司 GitLab 使用内部域名或自签名证书，请遵循公司的 IT 配置，不要为了绕过错误而随意关闭 SSL 证书验证。

克隆成功后，Git 通常会在当前目录下创建一个与仓库同名的文件夹。进入该目录：

```bat
cd project
```

请把 `project` 替换为实际生成的目录名。

## 四、输入 Username 和 Password / Token

如果仓库要求身份验证，CMD 可能会显示类似提示：

```text
Username for 'https://gitlab.example.com': your_username
Password for 'https://your_username@gitlab.example.com':
```

按提示输入：

- **Username**：通常是你的 GitLab 用户名。
- **Password**：如果 GitLab 要求使用 Personal Access Token，就在这里粘贴 Token，而不是输入 GitLab 登录密码。

粘贴 Token 时，终端可能不会显示字符或星号，这是正常现象。粘贴后按 Enter。

### 在 GitLab 创建 Personal Access Token

GitLab 的界面名称可能因版本不同略有差异，一般可按以下步骤操作：

1. 登录 GitLab。
2. 打开头像菜单下的 **Preferences / Edit profile / User Settings**。
3. 找到 **Access Tokens**。
4. 创建新的个人访问令牌，填写 Token 名称和到期时间。
5. 只勾选完成任务所需的最小权限。
6. 创建后立即复制并安全保存 Token。通常创建页面只会显示完整 Token 一次。

权限选择原则：

- 仅克隆私有仓库时，通常需要 `read_repository`。
- 通过 HTTPS 推送仓库时，通常需要 `write_repository`。
- 只有确实需要访问 API 时才考虑 `api` 或 `read_api` 等权限；不要为了方便而默认授予所有权限。
- 具体可用权限取决于 GitLab 版本、实例配置和组织策略。

**Token 是凭据，不要提交到 Git 仓库、截图、README 或聊天记录中。**

## 五、确认克隆结果

进入克隆后的项目目录，执行：

```bat
git status
git branch -a
git remote -v
```

说明：

- `git status`：查看当前分支和工作区状态。
- `git branch -a`：查看本地分支和远程跟踪分支。
- `git remote -v`：查看远程仓库地址。

例如，远程分支可能包含：

```text
remotes/origin/20260120
remotes/origin/dev
remotes/origin/feature/guobin/修改
remotes/origin/master
remotes/origin/master_back
```

以上分支名仅用于说明，实际以仓库输出为准。

## 六、切换到需要的分支

### 方式 A：创建本地分支并跟踪远程分支（推荐）

如果希望在某个远程分支上继续开发，例如 `origin/feature/guobin/修改`，执行：

```bat
git switch -c "feature/guobin/修改" --track "origin/feature/guobin/修改"
```

这会创建同名本地分支，并将其设置为跟踪对应的远程分支。

如果本地分支已经存在，则执行：

```bat
git switch "feature/guobin/修改"
```

较旧版本的 Git 也可以使用 `checkout`：

```bat
git checkout -b "feature/guobin/修改" "origin/feature/guobin/修改"
```

### 方式 B：只查看远程分支（不建议用于日常修改）

如果执行：

```bat
git checkout origin/feature/guobin/修改
```

Git 可能提示：

```text
You are in 'detached HEAD' state.
```

这表示当前处于 **detached HEAD（分离头指针）** 状态：你查看的是远程分支所指向的提交，而不是一个正常的本地分支。此时创建的提交不会自动归属于你预期的本地分支。

如果已经进入该状态，而且还没有创建需要保留的提交，可以创建本地分支：

```bat
git switch -c "feature/guobin/修改"
```

如果已经在 detached HEAD 状态下创建了重要提交，请先记录当前提交哈希（`git log -1 --oneline`），再创建分支保留该提交；不要直接切换并丢弃未保存的工作。

### 确认当前分支

```bat
git branch
git status
```

带 `*` 的分支通常是当前本地分支。确认 `git status` 显示的是你预期的分支。

## 七、使用 VS Code 打开项目

先确认 CMD 当前位于项目目录，然后执行：

```bat
code .
```

如果系统提示 `code` 不是内部或外部命令：

1. 打开 VS Code。
2. 选择 **File → Open Folder...**。
3. 选择克隆后的项目文件夹并打开。

在 VS Code 左侧资源管理器中即可查看项目文件。左侧 **Source Control（源代码管理）** 面板可以查看文件修改、暂存和提交状态。

## 八、日常 Git 基本操作

查看当前状态：

```bat
git status
```

查看最近提交：

```bat
git log --oneline -5
```

查看当前分支：

```bat
git branch
```

获取远程更新：

```bat
git fetch
```

如果需要把当前分支更新到远程最新内容，请先确认工作区干净，再根据项目工作流选择 `git pull` 或其他同步方式。团队项目应遵循团队规定的合并、变基和代码审查流程。

## 九、把这份文档上传到个人 GitHub

以下步骤适用于**你自己创建的文档仓库**，或你明确获准发布的内容。若本地文件夹原本是公司 GitLab 项目，不要直接把公司代码推送到个人 GitHub；建议新建一个独立文件夹和独立的个人 GitHub 仓库，只放这份通用操作文档。

### 1. 在 GitHub 创建空仓库

1. 登录 GitHub。
2. 点击 **New repository**。
3. 仓库名称可使用 `git-clone-guide`。
4. 根据需要选择 Public 或 Private。
5. 为避免首次推送时出现历史冲突，创建时可以先不勾选自动生成 README、License 或 `.gitignore`。
6. 创建仓库后，复制仓库 HTTPS 地址。地址格式通常类似：

   ```text
   https://github.com/<你的用户名>/git-clone-guide.git
   ```

### 2. 在独立文件夹中保存 README.md

新建一个个人文档目录，例如：

```text
D:\Personal\git-clone-guide
```

将本文保存为该目录下的 `README.md`。注意文件扩展名应为 `.md`，而不是 `README.md.txt`。

打开该目录的 CMD：

```bat
cd /d "D:\Personal\git-clone-guide"
```

### 3. 初始化本地 Git 仓库

如果这个文件夹还不是 Git 仓库，执行：

```bat
git init
git branch -M main
git add README.md
git commit -m "docs: add Git clone guide"
```

如果 Git 提示没有配置提交者身份，可按自己的实际信息配置：

```bat
git config --global user.name "你的 GitHub 显示名"
git config --global user.email "你的 GitHub 邮箱"
```

然后重新执行 `git commit`。这里的姓名和邮箱用于 Git 提交记录，不是登录密码。

### 4. 连接 GitHub 远程仓库

把地址替换为你刚创建的 GitHub 仓库地址：

```bat
git remote add origin https://github.com/<你的用户名>/git-clone-guide.git
```

确认地址：

```bat
git remote -v
```

如果提示 `remote origin already exists`，先检查现有远程地址：

```bat
git remote -v
```

确认这是个人文档仓库后，再按需更新：

```bat
git remote set-url origin https://github.com/<你的用户名>/git-clone-guide.git
```

不要在公司项目目录中盲目执行此命令，以免覆盖原有的 GitLab 远程地址。

### 5. 推送到 GitHub

```bat
git push -u origin main
```

首次推送时，GitHub 可能要求通过浏览器登录、Git Credential Manager、个人访问令牌或其他组织允许的方式完成身份验证。GitHub HTTPS 推送不再接受账户登录密码作为 Git 密码；请使用官方支持的认证方式，切勿将 Token 直接写进远程 URL。

推送成功后，刷新 GitHub 仓库页面，即可看到 `README.md`。

### 6. 后续更新文档

每次修改 README 后，在文档仓库目录执行：

```bat
git status
git add README.md
git commit -m "docs: update Git guide"
git push
```

如果修改了多份文件，可以在确认没有敏感文件后使用：

```bat
git add .
```

推送前建议再次执行 `git status`，检查本次将要提交的文件。

## 十、常见问题

### 1. `Authentication failed`

- 检查用户名是否正确。
- 确认 Token 未过期、未被撤销，并具有所需权限。
- 如果电脑缓存了旧凭据，使用 Windows 的“凭据管理器”检查对应 GitLab / GitHub 凭据，删除或更新错误的条目后再重试。
- 遵循公司 GitLab 的单点登录或 IT 身份验证要求。

### 2. `Repository not found` 或没有权限

- 检查仓库 URL 是否复制完整。
- 确认账户有该项目的访问权限。
- 确认使用的是正确的 GitLab/GitHub 账号和服务器地址。

### 3. `detached HEAD`

使用 `git status` 确认状态。若需要在当前提交上开始正常工作，可创建分支：

```bat
git switch -c "my-work-branch"
```

如果已有未保留的重要提交，先确认并保留提交，再切换分支。

### 4. `src refspec main does not match any`

通常是还没有创建任何提交，或者当前分支名称不是 `main`。先检查：

```bat
git status
git branch
git log --oneline -3
```

确保已成功提交，再按实际分支名推送。

### 5. 不小心把 Token 提交进仓库

仅删除文件或修改 README 并不能保证凭据从 Git 历史中消失。应立即在对应平台撤销该 Token；如果它已推送到远程仓库，还需要按照平台和组织的安全流程处理历史记录及可能的泄露影响。

---

## 快速命令清单

### 克隆 GitLab 项目

```bat
cd /d "D:\SAP工作\s4_BI"
git clone <GitLab HTTPS URL>
cd <克隆后生成的目录>
git status
git branch -a
git switch -c "<本地分支名>" --track "origin/<远程分支名>"
code .
```

### 推送个人文档到 GitHub

```bat
cd /d "D:\Personal\git-clone-guide"
git init
git branch -M main
git add README.md
git commit -m "docs: add Git clone guide"
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

> 注意：以上两组命令属于两个不同的工作场景。不要把个人 GitHub 的 `origin` 地址误设到公司 GitLab 项目中，也不要把未经授权的公司代码上传到个人仓库。
