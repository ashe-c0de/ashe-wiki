# Git Tag 学习指南

## 1. 什么是 Tag

Tag（标签）是 Git 中用于标记特定提交（commit）的引用，通常用于标记发布版本（如 v1.0.0、v2.1.0）。

---

## 2. Tag 与 Branch 的区别

### 核心差异

| 特性 | Tag | Branch |
|------|-----|--------|
| 用途 | 标记特定版本 | 持续开发 |
| 是否移动 | 固定不变 | 随新 commit 移动 |
| 命名规范 | 版本号（v1.0.0） | 功能名（feature/xxx） |

### 为什么 Tag 和 Commit 版本强绑定？

**1. 不可变性**

- Tag 一旦创建，就永久指向某个特定的 commit，不会随时间改变
- Branch 会随着新 commit 的提交不断向前移动
- 这保证了 `v1.0.0` 在任何时间点都代表完全相同的代码状态

**2. 版本追溯性**

```bash
# Tag 保证你能精确复现历史版本
git checkout v1.0.0    # 永远回到发布时的那个 commit
git checkout main      # 回到最新状态（可能已变化）
```

- 当你修复 bug 后需要回溯对比，Tag 提供了可靠的版本锚点
- CI/CD 系统通过 Tag 触发构建，确保每次发布的产物可追溯

**3. 发布语义**

- Tag 代表"这是一个正式版本"的声明，具有里程碑意义
- Branch 只是开发线索，不代表发布状态
- 团队成员可以通过 Tag 快速识别哪些 commit 是经过测试发布的

**4. 依赖管理**

```bash
# 下游项目可以稳定依赖特定版本
pip install package==1.0.0    # 对应 git tag v1.0.0
go get github.com/user/repo@v1.0.0
```

- 包管理工具依赖 Tag 来锁定版本
- 如果用 Branch，依赖关系会随着代码更新而不稳定

**5. 工作流示例**

```bash
# 场景：线上出现紧急 bug，需要确认当前生产环境代码

# 使用 Tag：明确知道是 v1.2.3 版本
git show v1.2.3
git diff v1.2.3 v1.2.4    # 清晰看到版本间差异

# 使用 Branch：无法确定具体是哪个 commit
git log main    # 需要人工排查，容易出错
```

---

## 3. 创建 Tag

### 3.1 轻量标签（Lightweight Tag）

仅指向特定 commit 的引用，不包含额外信息。

```bash
git tag v1.0.0
```

### 3.2 附注标签（Annotated Tag）（推荐）

包含标签信息、标签作者、日期和注释，存储为完整对象。

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

### 3.3 在指定 commit 创建标签

```bash
# 在历史 commit 上打标签
git tag -a v1.0.0 <commit-hash> -m "Release version 1.0.0"

# 示例
git tag -a v1.0.0 abc1234 -m "Fix critical bug"
```

---

## 4. 完整工作流示例

### 场景：发布 v1.0.0 版本

```bash
# 1. 切换到主分支
git checkout main

# 2. 拉取最新代码
git pull origin main

# 3. 确认当前 commit 是正确的发布版本
git log --oneline -5

# 4. 创建附注标签
git tag -a v1.0.0 -m "Release version 1.0.0 - Initial release"

# 5. 验证标签创建成功
git tag -l
git show v1.0.0

# 6. 推送标签到远程仓库
git push origin v1.0.0

# 7. 验证远程 tag
git ls-remote --tags origin | grep v1.0.0
```

---

## 5. 快速参考卡片

```bash
# 创建并推送标签（最常用）
git tag -a v1.0.0 -m "Release message"
git push origin v1.0.0

# 查看标签
git tag
git show v1.0.0

# 删除标签
git tag -d v1.0.0
git push origin --delete v1.0.0
```
