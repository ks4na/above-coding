# git worktree 多分支并行开发

- [使用场景](#使用场景)
- [常用命令](#常用命令)
  - [git worktree add](#git-worktree-add)
  - [git worktree list](#git-worktree-list)
  - [git worktree remove](#git-worktree-remove)
  - [git worktree prune](#git-worktree-prune)

## 使用场景

多任务并行时如果需要临时切换分支（例如紧急 hotfix），可以使用 `git stash` 暂存工作内容，但是如果遇到以下场景，`git stash` 就不太可行：

- 正在 main 分支上跑长时间的测试，切换到 hotfix 或 feature, 测试就会中断
- 项目非常大，频繁的切换索引，成本非常高
- 切换分支，需要重新设置相应的环境变量，比如 dev/qa/prod
- 版本不同且不兼容，切换分支需要重新安装依赖等

虽然可以使用 `git clone` 多个 repo 分别对应不同分支，但是存在重复占用空间，不方便管理等问题。

此时可以使用 `git worktree` 命令来解决。`git worktree` 命令的作用就是：

```
仅需维护一个 repo，又可以同时在多个 branch 上工作，互不影响。
```

## 常用命令

命令行输入 `git worktree —help` 可以查看命令相关说明。

常用命令有以下四个：

```sh
 git worktree add [-f] [--detach] [--checkout] [--lock] [-b <new-branch>] <path> [<commit-ish>]
 git worktree list [--porcelain]
 git worktree remove [-f] <worktree>
 git worktree prune [-n] [-v] [--expire <expire>]
```

### git worktree add

```sh
# 常见用法：

# 1. 基于当前 main 分支创建一个 feature/abc 分支，且分支创建在上层目录的 feature/abc 目录下
git worktree add -b feature/abc ../feature/abc main # 如果 HEAD 指向 main 分支最新提交，则可以省略指定 main

# 2. 将一个已存在的分支 featrue/existed 创建到上层目录的 feature/existed 目录下
git worktree add ../feature/abc feature/existed
```

### git worktree list

所有的 worktree 都共用的一个 repo，所以在任意一个 worktree 下都可以执行如下命令查看 worktree 列表：

```sh
git worktree list
```

### git worktree remove

worktree 上的工作完成后，需要及时删除，否则也会占用磁盘空间。使用如下命令删除某个 worktree:

```sh
git worktree remove <worktree>
# 例： git worktree remove feature/abc
```

如果想要删除的 worktree 存在改动，则可以指定 `-f` 强制删除。

### git worktree prune

如果是手动删除 worktree 目录，使用 `git worktree list` 还是可以看到该 worktree，但是会显示 `prunable`，对于这些无效的 worktree 信息，需要使用以下命令清除：

```sh
git worktree prune
```
