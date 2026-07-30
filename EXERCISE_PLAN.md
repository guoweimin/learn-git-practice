# Git 实操练习计划

> 文件存档时间：2026-07-29
> 练习者：guoweimin
> 已有 GitHub 仓库：learn-git（公开）、my-utils（公开）
> 主练习目录：`/Users/weimin/Learning/learn-git`

---

## 总体路线

```
基础回退 ──→ 分支合并 ──→ 远程协作 ──→ 进阶技巧 ──→ 综合场景 ──→ 排错题
  (4题)        (4题)        (4题)        (5题)        (5题)        (4题)
```

---

## 第一组：基础与回退（4 道题）

### 第 1 题：完整工作流热身
- **类型**：操作题
- **目标**：不看笔记完成"初始化/改文件/add/commit/log/diff"全流程
- **任务**：
  1. 在当前 `learn-git` 目录初始化一个 git 仓库
  2. 配置 user.name 和 user.email（如果尚未配置全局）
  3. 创建 `README.md`，写入一行内容，add 并 commit
  4. 修改 `README.md` 添加第二行，add 并 commit
  5. 查看提交历史（oneline 格式）
  6. 用 `git diff` 查看工作区与最后一次提交的差异
- **考察点**：init → add → commit → log → diff 完整肌肉记忆

### 第 2 题：三大区域对比（三 reset 同场景跑三次）
- **类型**：操作题
- **目标**：亲手体会 `--soft` / `--mixed` / `--hard` 的区别
- **任务**：
  1. 创建 `demo.txt`，写入 A 行 → add → commit
  2. 修改为 B 行 → add → commit
  3. 修改为 C 行 → **不 add**
  4. 执行 `git reset --soft HEAD~1`，观察 `status` 和 `diff` 的变化
  5. 重新回到 B 行状态 → add → commit → 改 C 行不 add
  6. 执行 `git reset --mixed HEAD~1`（默认），对比差异
  7. 再回到 B 行 → add → commit → 改 C 行
  8. 执行 `git reset --hard HEAD~1`，观察工作区文件内容
- **考察点**：分清三种 reset 对"仓库/暂存区/工作区"的影响

### 第 3 题：amend 与 restore
- **类型**：操作 + 判断题
- **目标**：修补提交、撤销工作区改动、撤销 add
- **任务**：
  1. 创建新文件 `app.py`，写入一行代码 → add → commit
  2. 使用 `--amend` 修改提交说明（改得更准确）
  3. 修改 `app.py` 新增一行 → 用 `--amend` 把这次修改合并到上次提交（不修改说明）
  4. 再改 `app.py` → `git restore <file>` 撤销工作区改动
  5. 改 `app.py` → add → `git restore --staged <file>` 撤销 add
- **判断题**：`git commit --amend` 和 `git reset --soft HEAD~1` 都能修改最近提交，它们的本质区别是什么？
- **考察点**：amend 是"替换指针指向的新提交"；restore 的两种用法

### 第 4 题：clean 与安全问题
- **类型**：操作 + 排错
- **目标**：清理 untracked 文件，理解安全等级
- **任务**：
  1. 创建两个 untracked 文件和一个 untracked 目录（里面再放个文件）
  2. 用 `git clean -n` 预览会被删除的文件
  3. 用 `git clean -fd` 实际删除
- **判断题**：为什么不建议在公共仓库或 CI 环境用 `reset --hard`？clean 和 hard 哪个更危险？
- **考察点**：clean 的 -n 预览习惯、-fd 作用、安全等级排序

---

## 第二组：分支与合并（4 道题）

### 第 5 题：快速建分支与合并
- **类型**：操作题（计时练习）
- **目标**：快速度完成"建分支→切换→改代码→提交→切回→合并→删除"
- **任务**：
  1. 从 main 创建 feature/login 分支
  2. 切换到 feature/login，创建 `login.py` → add → commit
  3. 切回 main，合并 feature/login（观察是否 fast-forward）
  4. 删除 feature/login 分支
  5. 用 `git branch -d` 删一瞬
- **问答题**：这次合并为什么是 fast-forward？什么情况下不能 fast-forward？
- **考察点**：branch/switch/merge 的流畅度

### 第 6 题：三方合并与冲突解决
- **类型**：操作题
- **目标**：制造冲突、解决冲突、完成合并
- **任务**：
  1. 从 main 创建 feature/payment 分支，切换到该分支
  2. 修改 `app.py` 的同一区域 → add → commit
  3. 切回 main，修改 `app.py` 的**同一行** → add → commit
  4. 合并 feature/payment，观察冲突
  5. 打开文件看 `<<<<<<<` / `=======` / `>>>>>>>`，手动解决
  6. `git add` → `git merge --continue` 完成合并
- **考察点**：冲突标记的阅读与解决、continue 的习惯

### 第 7 题：rebase 让历史变直线
- **类型**：操作题
- **目标**：用 rebase 替代 merge，对比 log 形状差异
- **任务**：
  1. 从 main 创建 feature/api 分支，改文件 → commit（两次）
  2. 切回 main，也改文件 → commit（一次）
  3. 切回 feature/api，`git rebase main`
  4. 观察历史（应该变直线了）
  5. 切回 main，合并 feature/api（此时应为 fast-forward）
- **问答题**：使用 rebase 前 main 和 feature/api 的关系图？rebase 后呢？
- **考察点**：rebase 的"搬家"本质

### 第 8 题：merge vs rebase 判断
- **类型**：判断题（无操作）
- **场景题**：
  1. "你的 feature 分支只有你一个人用，还没 push 过" → merge 还是 rebase？
  2. "你和同事在同一分支协作，已 push 了你的提交" → merge 还是 rebase？
  3. "你想让 PR 合并后历史清晰干净" → 该用什么方式？
  4. "main 上有几个新提交，你想在 feature 上先用上它们" → 用 merge 还是 rebase？为什么？
- **考察点**：黄金法则、场景取舍

---

## 第三组：远程协作（4 道题）

### 第 9 题：本地仓库连接远程
- **类型**：操作题
- **目标**：将本地仓库与 GitHub 仓库关联并推送
- **任务**：
  1. 将本地仓库与 `guoweimin/learn-git` 关联（remote add）
  2. 推送到远程 main 分支（设置 upstream）
  3. 用 `git remote -v` 验证
  4. 在 GitHub 网页端查看是否推送成功
- **考察点**：remote add / push -u

### 第 10 题：模拟队友协作（本地模拟）
- **类型**：操作题
- **目标**：用 clone 模拟队友，体验 push/fetch/pull 完整循环
- **任务**：
  1. 在 `/Users/weimin/Learning/` 下 clone 一份仓库（当作"队友"）
  2. 在"队友"仓库中改文件 → add → commit → push
  3. 切回原仓库，`git fetch` 观察变化
  4. 然后 `git pull` 拉取队友的提交
- **考察点**：fetch（只下载不合并） vs pull（下载+合并）

### 第 11 题：远程冲突解决
- **类型**：操作题
- **目标**：push 被拒绝 → pull 解决冲突 → 重新 push
- **任务**：
  1. 在"队友"仓库修改同一文件同一行 → add → commit → push
  2. 在原仓库修改同一文件同一行 → add → commit → push（会被拒绝）
  3. 看到拒绝信息后，用 `git pull` 拉取
  4. 解决冲突 → add → commit → push
- **考察点**：push 被拒绝的典型流程、pull 的本质是 fetch+merge

### 第 12 题：pull.rebase 配置
- **类型**：判断题 + 操作
- **目标**：理解 `pull.rebase` 三种模式的区别
- **任务**：
  1. 查看当前 pull.rebase 配置
  2. 设置 `git config pull.rebase true`
  3. 再次模拟远程冲突场景，观察 pull 的行为（这次 pull 相当于 fetch + rebase 而非 merge）
- **问答题**：`pull.rebase = false` / `true` / `merges` 各代表什么行为？什么场景推荐哪种？
- **考察点**：pull 两种模式的本质区别

---

## 第四组：进阶技巧（5 道题）

### 第 13 题：stash 暂存
- **类型**：操作题
- **目标**：临时保存工作区改动，切分支修 bug 后恢复
- **任务**：
  1. 在工作区改一个文件（未 add），然后 `git stash`
  2. 确认工作区恢复干净
  3. 切到其他分支做点事（可选）
  4. 切回来，用 `stash pop` 恢复改动
  5. 再做一次 stash，这次加一条 `stash save "描述信息"`
  6. 用 `stash list` 查看，用 `stash apply` 恢复（不删除）
- **考察点**：stash pop vs apply、stash save 加描述

### 第 14 题：revert 撤销已推送提交
- **类型**：操作题
- **目标**：用 revert 生成反向提交撤销已有提交（区别于 reset）
- **任务**：
  1. 添加一个"错误"的提交（比如加一行错误代码）→ commit → push
  2. `git revert HEAD` 撤销它
  3. 查看 revert 生成的新提交，push 到远程
  4. 用 `git log` 可以看到 revert 的记录
- **问答题**：已经推送到远程的提交，为什么不能用 `reset` 而要使用 `revert`？
- **考察点**：revert vs reset、安全撤销已推送提交

### 第 15 题：reflog 找回"丢失"的提交
- **类型**：操作 + 排错
- **目标**：误操作 reset 后用 reflog 找回
- **任务**：
  1. 创建一个提交，添加 feature.txt → commit
  2. `git reset --hard HEAD~1`（误删！）
  3. 用 `git reflog` 找到被删提交的哈希
  4. 用 `git cherry-pick` 或 `git reset --hard <hash>` 找回
- **考察点**：reflog 是"后悔药"、cherry-pick 单提交摘取

### 第 16 题：tag 打标签
- **类型**：操作题
- **目标**：创建 annotated tag 并推送
- **任务**：
  1. 为当前最新提交打一个 annotated tag：`v1.0.0`，说明 "First stable version"
  2. 查看 tag 列表
  3. 用 `git show v1.0.0` 查看 tag 详情
  4. 推送 tag 到远程（默认 `git push` 不推送 tag，需指定）
  5. 在 GitHub 上查看 release
- **考察点**：annotated vs lightweight、tag 需单独推送

### 第 17 题：.gitignore 与 cherry-pick 综合
- **类型**：操作题
- **目标**：创建忽略规则、摘取提交
- **任务**：
  1. 创建 `.gitignore`，忽略 `*.log` 和 `node_modules/`
  2. 创建 `.env` 文件，也加进去忽略
  3. 验证 untracked 状态是否正确
  4. 创建 feature/temp 分支，做一个提交
  5. 切回 main，用 cherry-pick 只摘取这个提交
- **考察点**：gitignore 规则、cherry-pick 精准提权

---

## 第五组：综合场景题（5 道题）

### 第 18 题：正写代码突然要修 bug（stash）
- **类型**：场景判断 + 操作
- **场景**：你在 feature 分支上写了一堆代码，还没写完不能 commit。突然线上出了紧急 bug，需要你从 main 切个 hotfix 分支去修。
- **任务**：
  1. 模拟：在 feature 分支改文件（未 add 或已 add 但未 commit）
  2. 用 stash 保存当前工作
  3. 切到 main → 创建 hotfix 分支 → 修 bug → commit → 切回 feature
  4. stash pop 恢复工作
- **追问**：如果 stash 了多个，怎么选择恢复哪个？
- **考察点**：stash 实战场景

### 第 19 题：发 PR 前历史整理（rebase -i）
- **类型**：操作题
- **目标**：在发 PR 前用交互式 rebase 整理提交历史
- **任务**：
  1. 在 feature/squash 分支上创建 4 个琐碎提交（比如 "typo fix"、"add comment"、"fix format"、"real logic"）
  2. 用 `git rebase -i HEAD~4` 合并前 3 个 fixup 或 squash
  3. 最终只留 1-2 个有意义的提交
  4. 如果发生冲突，解决后 `--continue`
- **追问**：rebase -i 中的 pick / reword / squash / fixup / drop 各用于什么场景？
- **考察点**：交互式 rebase 整理历史

### 第 20 题：找 bug 引入点（bisect）
- **类型**：操作题
- **目标**：用 bisect 二分定位 bug
- **任务**：
  1. 造一批提交，前几个"正常"，中间某提交"引入 bug"，后几个继续正常
  2. 在 main 上准备一系列提交（至少 6-8 个），每个提交包含一个小脚本 `calc.py`
  3. 在第 4 个提交故意制造一个错误（比如 `add(2,2)` 返回 5）
  4. 用 `git bisect start` → `bad`（当前） → `good`（最早正常提交）
  5. 在每一步判断"当前文件是否正常"，二分定位到引入 bug 的提交
  6. 用 `git bisect reset` 结束
- **考察点**：bisect 二分法、实用 debug 技能

### 第 21 题：worktree 并行开发
- **类型**：操作题
- **目标**：用 worktree 同时打开两个分支的代码
- **任务**：
  1. 在当前目录创建 main 分支的一个 worktree 到 `../learn-git-hotfix`
  2. 在 worktree 目录中做修改 → add → commit（主仓库应该能看到）
  3. 用 `git worktree list` 查看
  4. 完成 worktree 中的工作后，用 `git worktree remove` 清理
- **问答题**：worktree vs stash，各在什么场景下使用？
- **考察点**：worktree 多目录并行

### 第 22 题：PR 工作流完整模拟
- **类型**：操作题
- **目标**：用 gh CLI 创建 PR、查看、合并
- **任务**：
  1. 创建新分支 feature/pr-demo，做修改 → commit → push
  2. `gh pr create --fill` 创建 PR
  3. `gh pr view --web` 在浏览器查看
  4. `gh pr merge --squash` 合并（选 squash 方式）
  5. 本地切回 main，pull 更新，删除 feature 分支
- **追问**：squash / merge / rebase 三种 PR 合并方式，对提交历史和协作有什么不同影响？
- **考察点**：gh CLI PR 工作流

---

## 第六组：排错题（4 道题）

### 第 23 题：push 被拒绝
- **类型**：排错题
- **错误场景**：
  ```
  $ git push
  ! [rejected]        main -> main (fetch first)
  error: failed to push some refs to 'github.com:guoweimin/learn-git.git'
  hint: Updates were rejected because the remote contains work that you do not
  hint: have locally.
  ```
- **问答题**：
  1. 这个报错说明什么？
  2. 应该用什么命令序列解决？
  3. 如果本地和远程的改动在同一文件的同一行，会发生什么？怎么处理？
- **考察点**：push 冲突的理解与解决流程

### 第 24 题：detached HEAD
- **类型**：排错题
- **错误场景**：
  ```
  $ git checkout v1.0.0
  Note: switching to 'v1.0.0'.
  You are in 'detached HEAD' state...
  ```
- **问答题**：
  1. detached HEAD 是什么意思？为什么会进入这个状态？
  2. 在这个状态下做了修改并 commit 了，怎么才能不丢失这些修改？
  3. 怎么安全退回到正常的分支上？
- **考察点**：HEAD 本质、detached HEAD 的应对

### 第 25 题：回退已推送但不想 revert
- **类型**：排错题
- **错误场景**：用户 `git reset --hard HEAD~1` 把本地 main 回退了一个提交，然后尝试 `git push` 报错：
  ```
  ! [rejected]        main -> main (non-fast-forward)
  ```
- **问答题**：
  1. 为什么 push 被拒绝？
  2. 如果用户坚持要用本地的状态覆盖远程，该用什么命令？（危险等级说明）
  3. 推荐的做法应该是什么？
- **考察点**：force push 的风险、推荐用 revert

### 第 26 题：rebase 冲突中想放弃
- **类型**：排错题
- **错误场景**：用户在执行 `git rebase main` 时遇到冲突，解决了一部分后觉得太乱了想放弃：
  ```
  $ git rebase main
  CONFLICT (content): Merge conflict in app.py
  ```
- **问答题**：
  1. 此时有哪些选择？
  2. 怎么完全放弃 rebase 回到之前的状态？
  3. 怎么继续 rebase（解决冲突→add→continue）？
- **考察点**：rebase 中的 --abort / --skip / --continue、冲突解决流程

---

## 附：评分标准与后续建议模板

每道题完成后按以下标准评价：
- ✅ 命令完全正确
- ⚠️ 基本正确但有更优解/小瑕疵
- ❌ 错误，给出正确做法并解释为什么

全部完成后给出：
- 已熟练掌握的知识点
- 还要加强的知识点
- 后续自主练习建议
