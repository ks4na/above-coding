# git 导出特定提交记录变更过的文件

- [背景](#背景)
- [相关命令介绍](#相关命令介绍)
- [导出指定的两个提交间的变更文件](#导出指定的两个提交间的变更文件)

## 背景

增量部署等场景下，需要能导出某个分支上所有的变更过（新建或修改，不包含删除）的文件，而且还需要按照目录结构的形式，此时就需要使用以下方式来实现。

## 相关命令介绍

第一个命令：

```sh
git archive -o xxx.zip HEAD
```

这个命令会将当前最新提交（即 `HEAD`）对应的仓库全部文件打包成 `xxx.zip`，虽然这不是想要的效果，但是这个命令可以后接参数指定需要导出的文件列表，如果能自动找到变更过的文件列表并加到该命令后面，就满足要求了。

第二个命令：

```sh
git diff --name-only --diff-filter=d HEAD^
```

这个命令会列出当前最新提交（即 `HEAD`）与上一个提交之间变更过（新建或修改，不包含删除）的文件。

最终两个命令组合起来就可以实现导出当前提交与上次提交之间变更过（新建或修改，不包含删除）的文件。

```sh
git archive -o xxx.zip HEAD $(git diff --name-only HEAD^)
```

> 注意：两个提交之间不能只包含删除文件而没有新增或修改的文件，否则导出内容是有问题的。

## 导出指定的两个提交间的变更文件

上述命令能够导出最新提交和它上一次提交之间的变更文件，如果想要导出指定的某两个提交(`(commitId1， commitId2]`，前开后闭区间且 `commitId1` 早于 `commitId2` )间的变更文件，需要对上面的命令作微调：

```sh
git archive -o xxx.zip commitId2 $(git diff --name-only commitId1..commitId2)
```

> 说明：  
> 外层 `git archive -o xxx.zip commitId2` 如果不加后面的文件列表参数，是导出 `commitId2` 时仓库中的所有文件；  
> 内层 `git diff --name-only --diff-filter=d commitId1..commitId2` 列出闭合区间 `(commitId1， commitId2]` 之间变更过的（新建或修改，不包含删除）文件。
