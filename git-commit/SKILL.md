---
name: git-commit
description: Git commit with auto-generated message. Use when the user says "提交", "commit", "git commit", or "git:commit". Do not use when the user only wants a commit message without committing, or to push, open a PR, rebase, or rewrite history.
allowed-tools: shell(git:*)
---

## Instructions

1. Read repo rules: commitlint config (`commitlint.config.*`, `.commitlintrc*`), `CONTRIBUTING.md`, and `git log --oneline -5`. Repo rules override this skill
2. Run `git status --porcelain`, `git diff --staged`, `git diff` to understand all changes. If there are no changes, tell the user — do not make an empty commit
3. One logical change per commit. If changes are unrelated, split into multiple commits. Unstage unrelated files with `git reset -q -- <file>`
4. Stage only the relevant files by name (do NOT use `git add -A` / `git add .`; do NOT use interactive `git add -p` / `-i`; quote globs like `'*.test.*'`)
5. Check `git diff --staged --name-only` — never commit `.env`, credentials, or private keys
6. Commit with one `-m` per paragraph (works in Bash and PowerShell): `git commit -m "主题" -m "正文"`
7. Verify with `git log -1 --stat` and `git status --porcelain`

## Commit message rules

- **始终使用中文撰写提交信息**（无论最近提交是什么语言）
- Subject line: **50 chars or less**. State WHAT changed, not HOW.
- If multiple files changed for the same purpose, summarize the purpose — don't list files
- Body (optional): only when the subject alone is ambiguous. Max 2 lines, focus on WHY
- No scope prefixes (feat/fix/chore) unless the repo already uses them (log or commitlint)
- **禁止添加 Claude 署名**（`Co-Authored-By: Claude ...` 等 trailer），即使最近提交里有也不要模仿

**Bad:** `更新用户认证模块以处理令牌刷新的边界情况并增加错误日志`
**Good:** `修复令牌刷新边界情况`

## Safety

- If a hook fails, the commit did not happen: fix the issue, re-stage, re-run the same `git commit`. Do NOT use `--amend` or `--no-verify` (unless the user asks)
- Do not change git config. Do not use `git reset --hard` or `git rebase`
- Do not push unless the user asks. Never force push to the default branch
