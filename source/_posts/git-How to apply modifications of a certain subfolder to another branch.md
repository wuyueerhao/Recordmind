---
title: git 如何将某个子文件夹的修改应用到另一分支
date: 2024-09-02 12:16:43
tags: [git]
categories:
	- 记录
---

要将 Git 仓库中的某个子文件夹的修改从一个分支应用到另一条分支，可以按照以下步骤进行操作：

#### 1. 切换到目标分支
首先，切换到你希望应用这些修改的目标分支。例如，如果你想将修改应用到 `feature-branch` 分支上：

```bash
git checkout feature-branch
```

#### 2. 使用 `git checkout` 或 `git restore` 提取子文件夹的修改
假设你要从 `source-branch` 分支提取子文件夹 `path/to/folder` 的修改，可以使用以下命令：

```bash
git checkout source-branch -- path/to/folder
```

或者在较新的 Git 版本中，你也可以使用 `git restore`：

```bash
git restore --source=source-branch --staged --worktree path/to/folder
```
<!-- more -->
#### 3. 提交修改
现在，你的目标分支上应该只包含你选择的子文件夹的修改。你可以通过 `git status` 查看具体的修改情况。

如果确认无误，可以进行提交：

```bash
git add path/to/folder
git commit -m "Apply changes from source-branch to path/to/folder"
```

这样，修改就只会应用到该分支上的特定子文件夹中，而不会影响其他文件或文件夹。

#### 注意事项
- 在 `git checkout` 和 `git restore` 命令中，`path/to/folder` 可以是文件或文件夹的路径。
- 如果需要多次提取不同的子文件夹或文件的修改，可以重复以上步骤。

这种方式能够确保你只应用指定文件或文件夹的更改到目标分支，而不会引入其他不相关的更改。