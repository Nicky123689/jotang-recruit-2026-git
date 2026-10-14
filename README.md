# jotang-recruit-2026-git

焦糖工作室 2026 招新 · 基础入门第 1 题（Git）的作业仓库。

## 这个仓库是干什么的

用来交基础入门的 Git 题和 AI agent 题：

- 用 Git 管理文件版本，并把仓库同步到 GitHub（本题要求）；
- 记录 Git / AI agent 的学习笔记，方便复查和复习。

## 仓库内容

```text
.
├── README.md                    # 仓库说明（本文件）
├── git-notes.md                 # Git 学习笔记：用过的命令与实验
├── notes/
│   └── ai-agent-notes.md        # AI agent 题：领养一只 Agent 的完整记录
├── AGENTS.md                    # 给 AI Agent 看的项目规则（"员工手册"）
└── AI_USAGE.md                  # AI 使用情况说明
```

## 怎么用这个仓库

```bash
git clone git@github.com:Nicky123689/jotang-recruit-2026-git.git
cd jotang-recruit-2026-git
git log --oneline        # 看提交历史
```

日常改动的流程：

```bash
git status               # 先看改了什么
git add <文件>
git commit -m "说明这次改了什么"
git push
```

## AI 使用与规则

- 本仓库的 AI 协助说明见 [`AI_USAGE.md`](AI_USAGE.md)。
- 给 AI Agent 的项目规则见 [`AGENTS.md`](AGENTS.md)。
- 仓库里**不允许**出现密码、Token、API Key 等敏感信息。
