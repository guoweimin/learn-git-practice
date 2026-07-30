# Git 速查表 (Cheatsheet)

> 按知识点分组，方便速查。核心命令用 **粗体** 标识。

---

## 一、基础工作流

| 操作 | 命令 |
|------|------|
| 初始化仓库 | **`git init`** |
| 查看状态 | **`git status`** |
| 添加到暂存区 | **`git add <file>`** / `git add .` |
| 提交 | **`git commit -m "说明"`** |
| 查看历史（简洁） | **`git log --oneline`** / `git log --oneline --graph --all` |
| 工作区 vs 暂存区差异 | **`git diff`** |
| 暂存区 vs 仓库差异 | **`git diff --staged`** |
| 配置用户名/邮箱 | `git config user.name "xxx"` / `git config user.email "x@x.com` |
| 查看配置 | `git config --list` / `git config --global --list` |

---

## 二、历史回退与撤销

| 操作 | 命令 | 影响范围 |
|------|------|---------|
| **修改最近提交说明** | `git commit --amend -m "新说明"` | 仓库 |
| 合并改动到最近提交 | `git commit --amend --no-edit` | 仓库 |
| **软重置**（历史回退，改动留暂存区） | `git reset --soft <commit>` | 仓库→暂存区 |
| **混合重置**（默认，改动留工作区） | `git reset --mixed <commit>` | 仓库+暂存区→工作区 |
| **硬重置**（全部丢弃 ⚠️） | `git reset --hard <commit>` | 全部丢失 |
| **撤销工作区改动** | `git restore <file>` | 工作区 |
| **撤销 add（取消暂存）** | `git restore --staged <file>` | 暂存区 |
| **预览删除 untracked 文件** | `git clean -n` | 只预览 |
| **删除 untracked 文件+目录** | `git clean -fd` | untracked 文件 |

> **安全等级（从安全到危险）**：`restore` → `reset --soft` → `reset --mixed` → `reset --hard` → `clean`

---

## 三、分支

| 操作 | 命令 |
|------|------|
| 列出分支 | **`git branch`** |
| 列出所有分支（含远程） | `git branch -a` |
| 创建分支 | `git branch <name>` |
| **切换分支** | `git switch <name>` |
| **创建并切换** | **`git switch -c <name>`** |
| 删除分支（已合并） | `git branch -d <name>` |
| 强制删除分支 | `git branch -D <name>` |
| 重命名分支 | `git branch -m <旧名> <新名>` |

---

## 四、合并

| 操作 | 命令 |
|------|------|
| **合并分支到当前分支** | **`git merge <branch>`** |
| 不快进合并（强制三方） | `git merge --no-ff <branch>` |
| 解决冲突后继续合并 | `git merge --continue` |
| 放弃合并 | `git merge --abort` |

> **Fast-forward**：主线没有新提交时，直接移动指针
> **三方合并**：两分支都有新提交，生成 merge commit

---

## 五、变基（Rebase）

| 操作 | 命令 |
|------|------|
| **变基** | **`git rebase <branch>`** |
| 继续变基（解决冲突后） | `git rebase --continue` |
| 跳过当前冲突提交 | `git rebase --skip` |
| 放弃变基 | `git rebase --abort` |
| **交互式变基** | **`git rebase -i HEAD~N`** |

### rebase -i 操作符

| 缩写 | 全称 | 作用 |
|------|------|------|
| `p` | pick | 保留该提交 |
| `r` | reword | 修改提交说明 |
| `s` | squash | 合并到上一个提交（合并说明） |
| `f` | fixup | 合并到上一个提交（丢弃说明） |
| `d` | drop | 删除该提交 |
| `e` | edit | 暂停以修改提交内容 |

> **黄金法则**：**永不 rebase 已推送到远程的提交**

---

## 六、远程协作

| 操作 | 命令 |
|------|------|
| 添加远程仓库 | **`git remote add origin <url>`** |
| 查看远程 | `git remote -v` |
| 删除远程 | `git remote remove <name>` |
| 克隆仓库 | **`git clone <url>`** |
| **首次推送（设上游）** | **`git push -u origin <branch>`** |
| 推送 | `git push` |
| 拉取（fetch + merge） | **`git pull`** |
| 仅下载不合并 | **`git fetch`** |
| 拉取远程分支信息 | `git fetch origin` |
| 查看远程分支 | `git branch -r` |

### Pull 模式配置

| 配置 | 行为 |
|------|------|
| `git config pull.rebase false` | pull = fetch + merge（默认） |
| `git config pull.rebase true` | pull = fetch + rebase |
| `git config pull.rebase merges` | pull = fetch + rebase，保留 merge commit |

> **push 被拒绝的解决流程**：`pull`（解决冲突）→ `add` → `commit` → `push`

---

## 七、stash（暂存）

| 操作 | 命令 |
|------|------|
| **暂存当前工作** | `git stash` |
| 暂存并添加描述 | `git stash push -m "描述"` |
| **恢复并删除最上层** | `git stash pop` |
| 恢复但不删除 | `git stash apply` |
| 恢复指定的 stash | `git stash apply stash@{n}` |
| 查看 stash 列表 | `git stash list` |
| 删除指定的 stash | `git stash drop stash@{n}` |
| 清空所有 stash | `git stash clear` |

---

## 八、tag（标签）

| 操作 | 命令 |
|------|------|
| **打 annotated 标签** | **`git tag -a v1.0 -m "说明"`** |
| 打 lightweight 标签 | `git tag v1.0` |
| 查看标签列表 | `git tag` |
| 查看标签详情 | `git show v1.0` |
| 删除本地标签 | `git tag -d v1.0` |
| **推送单个标签** | **`git push origin v1.0`** |
| 推送所有标签 | `git push origin --tags` |

---

## 九、revert（撤销已推送提交）

| 操作 | 命令 |
|------|------|
| **撤销最近提交** | **`git revert HEAD`** |
| 撤销指定提交 | `git revert <commit>` |
| 不自动生成提交 | `git revert --no-commit <commit>` |

> `revert` 生成**新的反向提交**，历史不会改变，适合已推送的提交。
> `reset` 会**改写历史**，仅适合本地未推送的提交。

---

## 十、cherry-pick（摘取提交）

| 操作 | 命令 |
|------|------|
| **摘取提交到当前分支** | `git cherry-pick <commit>` |
| 摘取一段连续提交 | `git cherry-pick <A>..<B>` |
| 不自动提交 | `git cherry-pick --no-commit <commit>` |

---

## 十一、reflog（后悔药）

| 操作 | 命令 |
|------|------|
| **查看所有 HEAD 移动记录** | `git reflog` |
| 查看某分支的 reflog | `git reflog <branch>` |
| 找回丢失提交 | `git cherry-pick <hash>` 或 `git reset --hard <hash>` |

> reflog 记录所有 HEAD 的移动（即使 commit 被 reset 也能找到），有效期约 90 天。

---

## 十二、bisect（二分查找）

| 操作 | 命令 |
|------|------|
| 开始二分 | `git bisect start` |
| 标记本次为坏 | `git bisect bad` |
| 标记某版本为好 | `git bisect good <hash>` |
| 自动测试 | `git bisect run <script>` |
| 结束二分 | `git bisect reset` |

---

## 十三、worktree（多目录并行）

| 操作 | 命令 |
|------|------|
| **添加 worktree** | `git worktree add ../路径 <branch>` |
| 查看 worktree 列表 | `git worktree list` |
| 删除 worktree | `git worktree remove <path>` |
| 清理（修剪已删除的） | `git worktree prune` |

---

## 十四、.gitignore

| 规则 | 示例 | 说明 |
|------|------|------|
| 忽略具体文件 | `secrets.txt` | 忽略该文件 |
| 忽略通配 | `*.log` | 忽略所有 .log 文件 |
| 忽略目录 | `node_modules/` | 忽略整个目录 |
| 取反 | `!important.log` | 不忽略该文件 |
| 根目录 | `/build` | 只在项目根目录匹配 |

---

## 十五、Git Hooks

| 钩子 | 触发时机 |
|------|---------|
| `pre-commit` | 执行 commit 前 |
| `commit-msg` | 提交说明写入后 |
| `pre-push` | 推送前 |
| `post-merge` | 合并完成后 |

- 放 `.git/hooks/` 下（默认），或设置 `git config core.hooksPath <目录>` 共享
- 退出码 **0 = 通过**，**非 0 = 拦截**
- 可用 `git commit --no-verify` 临时绕过

---

## 十六、GitHub 分支保护

| 设置 | 作用 |
|------|------|
| `Required status checks` | 必须通过 CI 才能合并 |
| `Require pull request` | 必须通过 PR 才能合入 |
| `Enforce for admins` | 管理员也要遵守 |
| `Allow force pushes` | 是否允许强制推送 |
| `Allow deletions` | 是否允许删除分支 |

---

## 十七、GH CLI（PR 工作流）

| 操作 | 命令 |
|------|------|
| 创建 PR | `gh pr create --fill` |
| 查看 PR | `gh pr view` / `gh pr view --web` |
| PR 差异 | `gh pr diff` |
| 评论 PR | `gh pr comment -b "文本"` |
| 查看 CI 状态 | `gh pr checks` |
| 合并 PR | `gh pr merge` (默认) / `--squash` / `--rebase` |

### PR 三种合并方式

| 方式 | 效果 | 历史 |
|------|------|------|
| **squash** | 所有提交压成一个 | 一个 merge commit |
| **merge** | 保留所有提交 | 生成 merge commit |
| **rebase** | 逐个应用提交到目标分支 | 直线历史，无 merge commit |

> PR 更新方式：向同一分支 `push` 新提交即可自动更新 PR

---

## 十八、GitHub Actions（CI/CD）

| 操作 | 命令 |
|------|------|
| 列出 workflow 运行 | `gh run list` |
| 查看运行详情 | `gh run view <id>` |
| 查看运行日志 | `gh run view --log <id>` |

- 配置文件位置：`.github/workflows/*.yml`
- 关键字段：`on`（触发）、`jobs`、`steps`、`uses`、`run`
- 需要写权限：`permissions: contents: write`

---

## 十九、Submodule / Subtree

### Submodule

| 操作 | 命令 |
|------|------|
| 添加子模块 | `git submodule add <url> <path>` |
| 初始化子模块 | `git submodule update --init` |
| 更新子模块到最新 | `git submodule update --remote` |
| 删除子模块 | `git submodule deinit <path>` → `git rm <path>` |

### Subtree

| 操作 | 命令 |
|------|------|
| 添加子树 | `git subtree add --prefix=<dir> <url> <branch> --squash` |
| 拉取子树更新 | `git subtree pull --prefix=<dir> <url> <branch> --squash` |
| 推送子树变更 | `git subtree push --prefix=<dir> <url> <branch>` |

---

## 二十、速查：易混淆点对比

| 对比组 | 区别 |
|--------|------|
| **soft vs mixed vs hard** | soft=撤提交留暂存区 / mixed=撤销到工作区 / hard=全丢 |
| **fetch vs pull** | fetch=只下载不合并 / pull=下载+合并 |
| **merge vs rebase** | merge=保留分支历史 / rebase=重写线性历史 |
| **reset vs revert** | reset=删提交改历史 / revert=加反向提交不改历史 |
| **amend vs reword** | amend=修改提交内容+说明 / reword=仅修改说明 |
| **pop vs apply** | pop=恢复并删除stash / apply=恢复但不删除stash |
