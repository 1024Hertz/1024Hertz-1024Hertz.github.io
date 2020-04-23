---
title: Git-Squash
tags:
  - Git
  - Squash
---

## 说明
> 进行多个`commit`的合并操作
**注意** 已经`push`的`commit`, 不可以进行合并
---
## 命令
* 最后三个`commit`合并为一个`commit`
~~~shell
git rebase -i HEAD~3
~~~
* 将`B-分支`上的`commit`压缩后合并到`B-分支`
~~~shell
git checkout A-分支
git merge --squash B-分支
~~~
**注意** 必须保留一个`pick`，如果将所有的`pick`都改为了`s`那就没有合并的载体
---