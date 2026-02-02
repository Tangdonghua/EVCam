# 上游同步工作流使用说明

## 📖 功能说明

此 GitHub Actions 工作流用于自动保持本仓库与上游仓库的同步。它会定期检查上游仓库的更新并自动合并到本仓库。

## 🚀 功能特性

- **自动同步**: 每天北京时间 8:00 (UTC 0:00) 自动执行同步
- **手动触发**: 支持在 GitHub Actions 页面手动触发同步
- **灵活配置**: 可以指定不同的上游仓库和分支
- **安全合并**: 使用 `--ff-only` 参数，遇到冲突时会跳过同步，保护本地修改

## 📋 使用方法

### 方法一：配置 upstream remote（推荐）

在本地仓库中添加 upstream remote：

```bash
git remote add upstream https://github.com/原作者/原仓库名.git
git push origin main
```

配置完成后，工作流将自动使用配置的 upstream 仓库。

### 方法二：手动触发时指定上游仓库

1. 进入 GitHub 仓库页面
2. 点击 `Actions` 标签
3. 在左侧选择 `Upstream Sync` 工作流
4. 点击右侧 `Run workflow` 按钮
5. 填写上游仓库信息：
   - **上游仓库地址**: 格式为 `owner/repo`，例如 `torvalds/linux`
   - **上游分支名称**: 默认为 `main`，可以指定其他分支如 `master`、`develop` 等
6. 点击 `Run workflow` 开始同步

## ⚙️ 工作流程

1. **检出代码**: 获取本仓库的完整历史记录
2. **配置上游**: 确定上游仓库地址（从 remote 或手动输入）
3. **同步代码**: 使用 Fork-Sync-With-Upstream-action 执行同步
4. **结果通知**: 输出同步结果状态

## 🔧 自定义配置

如果需要修改同步频率或其他参数，可以编辑 `.github/workflows/upstream-sync.yml` 文件：

### 修改同步频率

```yaml
on:
  schedule:
    # 修改 cron 表达式来改变执行频率
    # 示例：每 12 小时执行一次
    - cron: '0 */12 * * *'
```

### 修改目标分支

```yaml
- name: 同步上游代码
  with:
    target_branch: develop  # 改为你想要同步到的分支
```

## ⚠️ 注意事项

1. **冲突处理**: 工作流使用 `--ff-only` 参数，如果遇到冲突会自动跳过。需要手动解决冲突后再次同步。
2. **权限要求**: 工作流使用 `GITHUB_TOKEN`，已自动配置，无需额外设置。
3. **分支保护**: 如果目标分支设置了保护规则，可能需要调整设置以允许自动推送。

## 📝 常见问题

**Q: 为什么同步失败了？**  
A: 可能的原因：
- 上游仓库地址配置错误
- 存在无法自动合并的冲突
- 网络问题导致无法访问上游仓库

**Q: 如何查看同步日志？**  
A: 在 GitHub 仓库的 `Actions` 标签页可以查看每次运行的详细日志。

**Q: 可以同步到不同的分支吗？**  
A: 可以，修改工作流文件中的 `target_branch` 参数即可。

## 📚 相关资源

- [Fork-Sync-With-Upstream-action](https://github.com/aormsby/Fork-Sync-With-Upstream-action)
- [GitHub Actions 文档](https://docs.github.com/cn/actions)
