## 更新本地代码

```bash
git pull --rebase origin origin_branch
```

## 新建分支

```bash
git checkout -b new_branch  # 新建并切换分支
```

## 切换分支

```bash
git checkout branch   # 切换分支
git checkout -        # 切换到上一次分支
```

## 查看本地文件状态

```bash
git status
```

## 暂存代码

```bash
git add .
```

## 提交代码

```bash
git commit -m 'message'
```

## 查看提交信息

```bash
git log             # 当前分支
git log --all       # 所有分支
git log --oneline
```

## 代码推到远端

```bash
git push origin origin_branch
```

## 合并分支

```bash
git merge --no-ff branch
```

## 清除本地文件修改

```bash
git checkout -- xxx   # 某个文件
git checkout -f       # 所有
```

## 临时保存最近修改

```bash
git add .     # 没有被 git 管理的文件，需要使用
git stash
```

## 查看`stash`

```bash
git stash list
```

## 恢复临时保存

```bash
git stash pop stash@{stash_id}
```

## `git stash`冲突

`git stash pop` 冲突，不会自动删除`git stash`中的记录需手动清理

```bash
git stash drop stash@{stash_id}
```

## 删除本地分支

```bash
git branch -d local_branch    # 普通删除
git branch -D local_branch    # 强制删除
```

## 删除远端分支

```bash
git push --delete origin origin_branch
```

Tip：无法重命名远端分支，需要现删除远端分支，再将本地分支推到远端。

## 将本地分支推送到远端

```bash
git push origin local_branch:origin_branch
```

## 拉远端分支

```bash
git fetch
git checkout origin_branch
```

## 重命名本地分支

```bash
git branch -m old_branch new_branch   # 不在 old_branch
git branch -m new_branch      # 在 old_branch
```

## 修改最近一次提交信息

```bash
git commit --amend -m 'message'
```

## 修改`git`仓库开发者信息

```bash
git config --global user.name
git config --global user.email
```

## 撤销上一次提交

```bash
git reset HEAD^1
```

## 回滚

```bash
git reset --hard commit_id
```

## 取消合并

```bash
git merge --abort
```

## 查看所有操作记录

```bash
git reflog
```

Tip：搭配回滚可御剑飞行

## 打标签

```bash
git tag v1.2.3    # 当前版本打标签
git push origin --tags    # 标签推到远端
```

## 查看`git`远程关联

```bash
git remote -v
```

## 关联远程仓库

```bash
git remote set-url --add origin origin_url
```

## 添加远程仓库

```bash
git remote add origin_name origin_url
```