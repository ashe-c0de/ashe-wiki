## Init

```bash
# 设置你的名字
git config --global user.name "Your Name"

# 设置你的邮箱 (必须和 GitHub/Gitee 账号一致，否则没绿点)
git config --global user.email "your.email@example.com"

# 设置别名 (alias)
git config --global alias.st status
git config --global alias.sw switch
git config --global alias.cm 'commit -m'
git config --global alias.br branch
git config --global alias.last "log -1 HEAD"
git config --global alias.push-fwl 'push --force-with-lease'
- git st 代替 git status
- git sw 代替 git switch
- git cm "commit message" 代替 git commit -m "commit message"
```

## verify your config
git config --list

## Undo

1. 撤销工作区的修改 (还没 add)
```bash
# 丢弃某个文件的修改，还原到上一次提交的状态
git checkout -- <文件名>
# 或新版
git restore <文件名>
```
2. 撤销暂存区的修改 (已经 add，还没 commit)
```bash
# 把文件从暂存区踢出去，但保留工作区的修改
git reset HEAD <文件名>
# 或新版
git restore --staged <文件名>
```
3. 撤销提交 (已经 commit，还没 push)
```bash
# 回退到上一个版本，保留修改代码 (最安全)
git reset --soft HEAD~1

# 回退到上一个版本，彻底丢弃修改 (危险！)
git reset --hard HEAD~1
```   
4. 穿越时空 (回到任意历史版本)
```bash
# 1. 先找到那个版本的哈希值 (ID)
git log --oneline

# 2. 回退过去 (小心，这会抹除之后的所有提交)
git reset --hard <commit-hash>
```

## Stash

写到一半，老板让你切分支修 Bug？别提交半成品，先藏起来。
```bash
# 储藏当前修改
git stash

# 查看储藏列表
git stash list

# 恢复最近一次储藏
git stash pop

# 恢复指定某一次
git stash apply stash@{1}
```

## Rebase

```bash
# 在当前（个人）分支上，把主线的更新“接”过来，而不是产生一个 merge commit
git rebase main
# 注意：不要在公共分支上使用，会改写历史！
```

## Cherry-pick

把别的分支上的某一次特定提交合过来
```bash
git cherry-pick <commit-hash>
```

## Workflow Paradigm

### 分支策略
- `dev` — 公共开发分支（主干）
- `dev-ashe` — 个人开发分支

### 日常开发流程

```bash
# 创建并切换到新分支
git sw -c new-branch

# 删除本地分支
git br -d old-branch


# 1. 切换到个人分支
git sw dev-ashe

# 2. 开发、提交代码
git add .
git cm "feat: xxx"

# 3. 同步主干最新代码
#    使用 rebase 保持线性历史
git sw dev
git pull
git sw dev-ashe
git rebase dev

# 4. 如果有冲突，解决后继续
#    无冲突则跳过
git add .
git rebase --continue

# 5. 推送个人分支
git push origin dev-ashe --force-with-lease

# 或者已设置 Git alias：
git push-fwl

# 6. 创建 MR
#    dev-ashe → dev
```

- 始终在 `dev-ashe` 上开发，不直接在 `dev` 上修改或提交
- 用 `rebase` 而非 `merge` 同步主干，保持线性历史
- 通过 MR 将 `dev-ashe` 合入 `dev`，提交历史干净无多余 merge commit

