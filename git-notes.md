# Git 学习笔记

> 边做边记：下面每一条命令都是我在这个仓库里**真的敲过**的，附上当时看到的现象。

## 一、最基本的四个动作

| 命令 | 作用 | 我的理解 |
| --- | --- | --- |
| `git init` | 把当前目录变成 Git 仓库 | 生成隐藏目录 `.git/`，版本历史都存在这里面；删掉它就不再有“后悔药” |
| `git status` | 看当前状态 | 最常用的命令，告诉我哪些文件改了、哪些已经放进暂存区 |
| `git add <文件>` | 把改动放进**暂存区** | “我确认这次要提交这些改动” |
| `git commit -m "信息"` | 把暂存区的内容存成一个版本 | 每个提交有一个哈希值，是历史的“存档点” |

三个区的关系：

```text
工作区（我改的文件） --git add--> 暂存区（准备提交） --git commit--> 版本库（历史）
```

## 二、我这道题实际用过的命令

### 1. 建仓库、做第一次提交

```bash
mkdir jotang-recruit-2026-git && cd jotang-recruit-2026-git
git init
git add README.md
git commit -m "docs: 添加仓库说明"
```

现象：提交后 `git status` 显示 `nothing to commit, working tree clean`——说明工作区里没有未记录的改动了。

### 2. 尝试 `git commit` 后才发现要先配置作者

现象：`git commit` 报错

```text
Author identity unknown
*** Please tell me who you are.
```

解决：告诉 Git 这次提交的作者是谁。为了不改全局配置，我用了临时指定：

```bash
git -c user.name="nicky" -c user.email="nicky123689@163.com" commit -m "docs: 添加仓库说明"
```

**为什么要写邮箱**：Git 的历史是按“作者 + 时间”记录的，多人协作时靠它区分是谁改的；GitHub 也用它把提交和你账号关联起来（所以最好和 GitHub 账号的邮箱一致）。

### 3. 看改了什么

```bash
git diff                      # 工作区 vs 暂存区：还没 add 的改动
git diff --staged             # 暂存区 vs 上次提交：已经 add 的改动
git log --oneline --graph --all   # 一行一提交，画成图看分支走向
git show <commit>             # 看某一次提交具体改了什么
```

这道题里我故意先改一个文件、**不** add，运行 `git diff` 看到的就是这次改动；`git add` 之后再运行 `git diff`，输出变空（因为改动已经进了暂存区），要用 `git diff --staged` 才看得到——这个细节很容易搞混。

### 4. 撤销（"后悔药"真正的样子）

```bash
git restore <文件>            # 丢弃工作区改动，回到上次提交的样子
git restore --staged <文件>   # 把文件从暂存区拿出来（改动本身还在）
git revert <commit>           # 生成一个新提交，用来"抵消"某个已推送的提交（安全，不改历史）
git reset --soft HEAD~1       # 撤掉最近一次提交但保留改动（只在自己本地、没推送时用）
```

区别：`revert` 是**加**一个新提交来抵消，历史不改，适合已经推送出去的提交；`reset` 是**改**历史，只适合还没推送的本地提交。

## 三、和 GitHub 连起来

```bash
git remote add origin git@github.com:Nicky123689/jotang-recruit-2026-git.git
git remote -v                 # 确认远程地址
git push -u origin main       # 第一次推送，-u 记住"本地的 main 对应 origin/main"
git push                      # 之后直接 push
git pull                      # 把远程的新提交拉下来并合并
```

我的环境里 `github.com` 走的是 443 端口的 SSH（`~/.ssh/config` 里写 `Hostname ssh.github.com` + `Port 443`），验证连通性用：

```bash
ssh -T git@github.com
# → Hi Nicky123689! You've successfully authenticated, but GitHub does not provide shell access.
```

最后那句提示是正常的：GitHub 支持用 SSH 认证，但不给你登录 shell。

## 四、分支

```bash
git branch                    # 看本地分支
git switch -c feature/docs    # 新建并切换到新分支
git switch main               # 切回 main
git merge feature/docs        # 把那个分支合并进来
git branch -d feature/docs    # 合并完删掉它
```

我的理解：分支就是指向某次提交的“可移动标签”，新建分支几乎不花成本，所以适合“每做一件事开一个分支”。

## 五、几件容易踩的事

1. **提交信息别写 `update`、`final`**：写清“改了什么、为什么”，以后翻历史才有用。我用的写法：`docs: 添加 Git 学习笔记`（类型: 内容）。
2. **`git add .` 之前先 `git status`**：确认没有把密钥、临时文件、`.zip` 之类的垃圾一起提交进去。
3. **`.gitignore`**：明确写出“不要提交的东西”（本项目忽略了 `*.tmp`、`.DS_Store`）。
4. **不要提交 `.env`、Token、API Key**：一旦推到公开仓库，即使后来删掉，历史里也还留着。
5. **`git diff` 是复盘 AI 改动的关键工具**：AI Agent 帮你改代码时，先 `git diff` 看清楚它到底动了什么，再决定要不要提交。
