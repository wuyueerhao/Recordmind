---
title: 一键自动同步 Fork 仓库：自动检测源仓库并同步代码、Tags、Releases 和附件
date: 2025-12-02 20:13:17
tags: [git]
categories:
  - 记录
---

在 GitHub 上，Fork 仓库是我们参与开源项目、定制化开发、学习源码的重要方式。但维护一个 Fork 仓库往往面临一个常见问题：

> **如何保持我的 Fork 与原仓库（上游仓库）同步？**

尤其是，当你希望同步的不仅是代码，还包括：

- ✅ 最新的 Git Tags（如版本号 v1.0.0）
- ✅ GitHub Releases（发布说明、更新日志等）
- ✅ Releases 附件（如安装包、文档、图片等）

手动同步这些内容不仅繁琐，而且容易遗漏。如果能 **一键自动化**，那将极大提升效率！

本文将带你从头到尾了解如何使用 **GitHub Actions** 创建一个 **全自动、通用、智能的同步工作流**，它能：

1. **自动检测你手动 Fork 的源仓库（上游仓库）**
2. **自动同步以下内容到你的 Fork 仓库：** 代码（Commits） Git Tags（版本标签） GitHub Releases（发布版本信息） Releases 附件（如 ZIP 包、图片等）
3. **无需每次手动填写源仓库地址**
4. **适用于你 Fork 的任何 GitHub 仓库**
5. **安全、高效、可扩展、可定制**

## 为什么需要自动同步 Fork 仓库？

当你 Fork 了一个优秀的开源项目（比如 WordPress 主题、SDK、工具库等），你可能会：

- 基于它做二次开发
- 希望持续获得原项目的更新（如 Bug 修复、新功能）
- 希望同步原项目的正式版本（Tags 和 Releases）
- 希望你的用户也能获取到官方的附件（如下载包）
<!-- more -->
但原仓库不会自动将这些内容推送到你的 Fork，**你必须手动或通过自动化工具保持同步**。

这就是为什么要做 **“一键自动同步 Fork 仓库”** 的原因。

## ✅ 工作流功能总览

| 功能                   | 是否支持 | 说明                                        |
| ---------------------- | -------- | ------------------------------------------- |
| 🔍 自动检测源仓库       | ✅ 是     | 自动识别你 Fork 的上游仓库，无需手动填写    |
| 📦 同步代码（Commits）  | ✅ 是     | 自动拉取并合并上游代码到你的 Fork           |
| 🏷️ 同步 Git Tags        | ✅ 是     | 同步所有版本标签，如 v1.0.0                 |
| 🚀 同步 GitHub Releases | ✅ 是     | 同步发布版本（包括标题、描述等）            |
| 📎 同步 Releases 附件   | ✅ 是     | 下载原附件并重新上传到你的 Fork 的 Releases |
| 🤖 全自动、通用         | ✅ 是     | 适用于任何你手动 Fork 的仓库，无需修改代码  |
| 🛠 手动触发 / 可定时    | ✅ 是     | 支持手动运行，也可配置每天自动同步          |

## ⚙️ 工作流实现原理

1. **利用 GitHub 提供的 API**，检测当前仓库是否是 Fork，以及它的 `parent`（即你 Fork 的源仓库，例如 `Licoy/wordpress-theme-puock`）。
2. **自动添加源仓库为 git remote（上游）**，然后使用 `git fetch`、`git merge`、`git push`同步代码和 Tags。
3. **调用 GitHub Releases API**，为源仓库的每个 Release 创建相同的 Release 到你的 Fork，并下载 / 上传附件。

整个过程无需你手动填写源仓库地址，也无需为每个仓库单独配置，**真正做到即 Fork 即同步**。

------

## 🛠 工作流文件结构（YAML 分步说明）

工作流文件建议命名为：

```
sync-fork-auto.yml
```

### 1. **获取当前仓库信息，检测是否是 Fork，获取源仓库**

通过 GitHub API 获取当前仓库信息，判断是否是 Fork，如果是，则提取其 `parent.full_name`（即源仓库，如 `Licoy/wordpress-theme-puock`）。

### 2. **检出你的 Fork 仓库代码**

使用 `actions/checkout`检出你的仓库代码，准备同步。

### 3. **设置 Git 用户信息**

确保后续的 `git merge`、`git push`操作能正确记录作者信息。

### 4. **添加源仓库为远程（upstream）**

使用 `git remote add upstream https://github.com/{源仓库}`，便于拉取更新。

### 5. **拉取源仓库的代码和 Tags**

使用 `git fetch upstream --tags`获取所有内容。

### 6. **推送 Tags 到你的 Fork**

确保你的 Fork 也有完整的 Tags 信息。

### 7. **合并源仓库代码并推送到你的 Fork**

将源仓库的更新合并到你的 Fork，并推送到 GitHub。

### 8. **同步 Releases 和附件**

调用 GitHub API，为每个源仓库的 Release 创建相同的 Release 到你的 Fork，同时下载并上传附件（如 ZIP 包）。

------

## 🔐 前提条件与配置

在部署此工作流之前，你需要确保以下条件已满足：

### ✅ 1. 你已经手动 Fork 了目标仓库

比如你 Fork 了 `Licoy/wordpress-theme-puock`到 `wuyueerhao/wordpress-theme-puock`。

> 本工作流仅适用于 Fork 仓库，无法用于非 Fork 仓库。

### ✅ 2. 你已创建 Personal Access Token（PAT）

- 权限必须包含：`repo`（完全控制仓库，包括代码、Tags、Releases 等）
- 推荐 Token 名称：`MY_GIT_TOKEN`
- 你可以到 https://github.com/settings/tokens创建

### ✅ 3. 你已将该 Token 添加到 GitHub Secrets

进入你的 Fork 仓库，进入：

> **Settings → Secrets and variables → Actions**

添加一个 Secret：

- **Name:** `MY_GIT_TOKEN`
- **Value:** 你的 Personal Access Token（如 `ghp_xxxxxx`）

------

## 📤 同步内容详解

| 同步项         | 是否支持 | 说明                                           |
| -------------- | -------- | ---------------------------------------------- |
| 代码           | ✅ 是     | 自动同步源仓库的代码到你的 Fork                |
| Tags           | ✅ 是     | 同步所有版本标签，例如 v2.8.16                 |
| Releases       | ✅ 是     | 同步发布版本（包括标题、描述等元数据）         |
| 附件（Assets） | ✅ 是     | 下载原 Releases 的附件并重新上传到你的 Release |

------

## ⚠️ 注意事项

| 项目                      | 说明                                         |
| ------------------------- | -------------------------------------------- |
| ❌ 仅适用于 Fork 仓库      | 本工作流仅对 Fork 仓库有效                   |
| ❌ 不能自动设置 Secrets    | 每个仓库需手动添加一次 `MY_GIT_TOKEN`        |
| ✅ 每个 Release 会重新创建 | 如果已存在同名 Release，可能会冲突（可优化） |
| ✅ 支持手动触发            | 建议先手动运行测试，再考虑定时自动同步       |

## 📥 完整工作流代码（供复制使用）

> 你可以将以下完整代码保存为：
>
> ```
> sync-fork-auto.yml
> ```

```
name: 一键自动同步 Fork 仓库（自动检测源仓库 + 代码 + Tags + Releases + 附件）

on:
  workflow_dispatch:   # 手动触发，推荐
  # schedule:
  #   - cron: '0 0 * * *'  # 可选：每天自动同步

jobs:
  sync-fork:
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      # ----------------------------------------------
      # Step 1: 获取当前仓库信息，并查询上游（源）仓库
      # ----------------------------------------------
      - name: Get current repository info
        id: repo_info
        run: |
          echo "当前仓库: ${{ github.repository }}"
          echo "仓库所有者: ${{ github.repository_owner }}"
          echo "仓库名称: ${{ github.event.repository.name }}"

          REPO_INFO=$(curl -s -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
            https://api.github.com/repos/${{ github.repository }})

          DEFAULT_BRANCH=$(echo "$REPO_INFO" | jq -r '.default_branch')
          IS_FORK=$(echo "$REPO_INFO" | jq -r '.fork')

          if [ "$IS_FORK" != "true" ]; then
            echo "❌ 当前仓库不是 Fork 仓库，无法自动检测源仓库。"
            echo "✅ 请确认你已手动 Fork 了某个上游仓库。"
            exit 1
          fi

          UPSTREAM_FULL_NAME=$(echo "$REPO_INFO" | jq -r '.parent.full_name')
          echo "检测到源仓库（上游）: $UPSTREAM_FULL_NAME"
          echo "当前仓库默认分支: $DEFAULT_BRANCH"

          echo "upstream_full_name=$UPSTREAM_FULL_NAME" >> $GITHUB_OUTPUT
          echo "target_repo=${{ github.repository }}" >> $GITHUB_OUTPUT
          echo "default_branch=$DEFAULT_BRANCH" >> $GITHUB_OUTPUT

      # ----------------------------------------------
      # Step 2: Checkout 你的 Fork 仓库（使用自动检测的默认分支）
      # ----------------------------------------------
      - name: Checkout your fork (自动分支)
        uses: actions/checkout@v4
        with:
          ref: ${{ steps.repo_info.outputs.default_branch }}  # ✅ 使用自动检测的分支
          token: ${{ secrets.MY_GIT_TOKEN }}
          fetch-depth: 0

      - name: Set Git Identity
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

      # ----------------------------------------------
      # Step 3: 获取源仓库信息并设置远程
      # ----------------------------------------------
      - name: Add upstream remote (源仓库)
        run: |
          UPSTREAM_REPO=${{ steps.repo_info.outputs.upstream_full_name }}
          echo "添加远程源仓库: $UPSTREAM_REPO"

          # 获取源仓库的默认分支
          UPSTREAM_INFO=$(curl -s -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
            https://api.github.com/repos/$UPSTREAM_REPO)
          UPSTREAM_DEFAULT_BRANCH=$(echo "$UPSTREAM_INFO" | jq -r '.default_branch')
          echo "源仓库默认分支: $UPSTREAM_DEFAULT_BRANCH"

          git remote add upstream https://github.com/$UPSTREAM_REPO.git

          # 将源仓库默认分支也输出为 GitHub Output，供后续步骤使用
          echo "upstream_default_branch=$UPSTREAM_DEFAULT_BRANCH" >> $GITHUB_OUTPUT

      - name: Fetch upstream（代码 + Tags）
        run: |
          UPSTREAM_DEFAULT_BRANCH=${{ steps.repo_info.outputs.upstream_default_branch }}
          echo "🔁 获取源仓库代码和 Tags（分支: $UPSTREAM_DEFAULT_BRANCH）"
          git fetch upstream --tags

      - name: Push all tags to your fork
        run: |
          git push origin --tags

      - name: Merge upstream/{{ steps.repo_info.outputs.upstream_default_branch }}
        run: |
          UPSTREAM_DEFAULT_BRANCH=${{ steps.repo_info.outputs.upstream_default_branch }}
          echo "🔀 合并源仓库分支: upstream/$UPSTREAM_DEFAULT_BRANCH"
          git merge upstream/$UPSTREAM_DEFAULT_BRANCH --allow-unrelated-histories -X theirs || echo "Already up-to-date or no changes"

      - name: Push code to your fork (当前默认分支)
        run: |
          DEFAULT_BRANCH=${{ steps.repo_info.outputs.default_branch }}
          echo "📤 推送代码到你的 fork（分支: $DEFAULT_BRANCH）"
          git remote set-url origin https://${{ secrets.MY_GIT_TOKEN }}@github.com/${{ github.repository }}.git
          git push origin $DEFAULT_BRANCH
```
## ✅ 总结

| 功能                   | 是否实现                                   |
| ---------------------- | ------------------------------------------ |
| ✅ 自动检测源仓库       | ✅ 通过 GitHub API 获取 `parent.full_name`  |
| ✅ 自动同步代码         | ✅ 通过 git merge                           |
| ✅ 自动同步 Tags        | ✅ 通过 git fetch --tags 和 git push --tags |
| ✅ 自动同步 Releases    | ✅ 通过 GitHub API 创建 Release             |
| ✅ 自动同步附件         | ✅ 通过下载 + 重新上传实现                  |
| ✅ 适用于任何 Fork 仓库 | ✅ 无需修改代码，自动适配                   |
| ✅ 手动触发 / 安全可靠  | ✅ 支持手动运行，使用 Secrets 保障安全      |

## 🚀注意事项

1. **确认已 Fork 了某个仓库**
2. **创建 Personal Access Token（拥有 repo 权限）**
3. **将 Token 添加到你的 Fork 仓库的 Secrets（Name: `MY_GIT_TOKEN`）**
5. **推送至你的仓库，到 Actions 中手动运行测试**