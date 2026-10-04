# Git 学习笔记

## 1. Git 仓库与初始化

- 将当前目录初始化为 Git 仓库。
- 将本地仓库关联到 GitHub 等远端仓库。

## 2. Commit：提交记录

### 2.1 用 Commit ID 交流和查看改动

与 AI 或协作者讨论 Git 改动时，可以使用具体的 commit ID，明确讨论范围。

- 对比两个提交之间的改动：

  ```bash
  git diff <commit-a> <commit-b>
  ```

- 可以围绕某个文件或某次提交进行检查，也可以选择保留或移除特定提交。操作前先确认目标提交和保留范围。
- “言者所以在意，得意者忘言。”

## 3. Branch：分支

分支由指针管理，不需要完整复制一份代码。团队成员可以在各自分支上开发，最后再合并。

### 3.1 分支的用途

- 在自己的分支上开发和修复，不直接影响主分支。
- 在不同分支之间切换。
- 可以基于某个提交创建分支，例如建立测试分支。
- 当前分支示例：`not main yet`。

### 3.2 删除分支

不能删除当前所在的分支。需要先切换到其他分支，再删除目标分支。

### 3.3 Detached HEAD：分离头指针

分离头指针表示当前检出的是某个提交或标签，而不是分支。适合临时查看或测试；如果在此状态下产生提交，需要创建分支等方式保存，否则之后可能难以找到该提交。

```bash
git checkout a1b2c3d      # 切到某个 commit
git checkout v1.0.0       # 切到 tag
git checkout origin/main  # 切到远程跟踪分支
git switch --detach main  # 显式进入分离头指针状态
```

## 4. Worktree：工作树

Worktree 也是分支 与普通分支不同的是当创建分支后 需要把整体的项目的代码 复制到自己的目录 不是像普通的分支是通过指针来控制 共享代码的。每个工作树都有独立的工作目录，可以并行开发；它们共享同一个 Git 仓库和提交对象。

- 在 Codex 中可以创建永久工作树，为不同分支准备独立目录。
- 在一个工作树中的文件改动不会直接改动另一个工作树中的工作目录。

## 5. 工作区、暂存区与远端仓库

远端代码托管平台除了 GitHub，还包括 GitLab、Gitee 等。

### 5.1 常用命令

| 命令 | 笔记 |
| --- | --- |
| `git clone` | 克隆远端仓库到本地。 |
| `git push` | 将本地提交推送到远端仓库。 |
| `git pull` | 获取远端更新，并将其整合到当前本地分支。 |

`git pull` 通常包含获取远端更新和整合更新两个步骤。

### 5.2 暂存区

暂存区（staging area，也称 index）用于选择下一次提交中包含哪些改动。

### 5.3 关联远端仓库

常见方式有两种：

1. 使用 `git clone` 克隆已有远端仓库。
2. 将已有本地目录关联到一个已创建的远端仓库。

如果你是远端仓库的管理员，通常可以直接推送代码；参与他人的仓库时，通常通过 Pull Request（PR）提交改动。

远端仓库发生文件修改或新增文件后，可以使用 `git pull` 同步更新。

`origin/main` 表示名为 `origin` 的远端所跟踪的 `main` 分支；本地分支 `main` 是本地分支。

## 6. Pull Request：合并请求

以参与他人的开源项目为例：

1. 在 GitHub 上 Fork 原项目到自己的账号。
2. 将自己 Fork 后的仓库克隆到本地。
3. 创建个人开发分支，不直接在 `main` 上修改。
4. 在个人分支上修改代码，并推送到自己的远端仓库。
5. 在远端仓库发起 Pull Request。
6. 上游项目维护者进行代码 Review，再决定合并或拒绝。

### 6.1 同步上游仓库的更新

上游仓库（upstream）有更新时，可以通过 GitHub 页面上的 **Sync fork / Fetch upstream** 同步，也可以在命令行操作。

首次添加上游地址：

```bash
git remote add upstream https://github.com/原作者/原仓库.git
git remote -v
```

之后获取并合并上游 `main` 的更新：

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

提交 PR 前，先将分支更新到最新上游代码，并在本地处理好冲突。示例请求：

> `https://github.com/tech-shrimp/git_test` 是 upstream 项目。请把 upstream 的最新改动合并进当前分支；如果有冲突，先让我选择处理方式。

## 7. Cherry-pick：挑选提交

在自己的开发分支上，可以挑选特定 commit 合并到目标分支，而不是合并整个分支的所有提交。

## 8. Stash：暂存工作区改动

如果正在修改文件时需要临时切换分支，可以先提交改动，或使用 `stash` 暂存未提交内容。之后切回原分支，再恢复暂存的改动继续工作。

## 9. Rebase：变基

Rebase 会把当前分支的提交重新应用到新的基线上。

- 变基后，如果原分支已经推送到远端，通常需要使用 `--force-with-lease` 更新远端分支。
- 多人协作的共享分支要谨慎变基和强制推送，避免覆盖他人的提交。

## 参考资料

- [B 站视频](https://www.bilibili.com/video/BV1ySLc6QEcB/?spm_id_from=333.1391.0.0&vd_source=158718a4841d3b9ec4c24f290d04e206)
