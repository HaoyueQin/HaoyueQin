# GitHub 主页设置指南

## 实现原理分析

XTLine 的 GitHub 主页使用了两个核心工具：

### 1. 贪吃蛇动画 (Contribution Snake)
- **工具**: `Platane/snk` GitHub Action
- **原理**: 读取用户的 GitHub contribution graph，生成一个 SVG 动画，让一条蛇"吃掉"所有的贡献方块
- **输出文件**: `github-contribution-grid-snake.svg`（推送到 `output` 分支）

### 2. Metrics SVG 图片
- **工具**: `lowlighter/metrics` GitHub Action
- **原理**: 生成各种 GitHub 统计信息的 SVG 图片
- **输出文件**:
  - `metrics.left.svg`: 展示 header、activity、community、贡献日历等
  - `metrics.right.svg`: 展示编程语言统计、收藏的仓库、话题等

## 文件结构

```
github-profile/
├── .github/
│   └── workflows/
│       ├── snake.yml          # 贪吃蛇动画的 GitHub Action
│       └── metrics.yml        # 统计信息的 GitHub Action
├── image/                     # 存放图片文件夹
└── README.md                  # 主页内容
```

## 配置步骤

### 1. 创建 GitHub 仓库
在 GitHub 上创建一个与你的用户名同名的仓库（例如：`HaoyueQin/HaoyueQin`）

### 2. 配置 GitHub Secrets
进入仓库 Settings -> Secrets and variables -> Actions，添加以下 secrets：

#### METRICS_TOKEN
用于生成 metrics 统计图片的个人访问 token：

1. 访问 https://github.com/settings/tokens
2. 点击 "Generate new token (classic)"
3. 设置名称为 "METRICS_TOKEN"
4. 勾选以下权限：
   - `repo` (Full control of private repositories)
   - `read:user` (Read access to profile info)
   - `read:org` (Read access to organization info)
5. 生成并复制 token
6. 在仓库的 Secrets 中添加 `METRICS_TOKEN`，值为刚才复制的 token

### 3. 修改 README.md
将 README.md 中的占位符替换为你的个人信息：

- 将所有 `XTLine` 替换为你的 GitHub 用户名
- 修改个人介绍、项目链接等内容
- 如果需要，可以在 `image/` 文件夹中添加个人头像或图片

### 4. 启用 GitHub Actions
确保仓库的 Actions 功能已启用：
1. 进入仓库 Settings -> Actions -> General
2. 确保 "Allow all actions and reusable workflows" 已选中

### 5. 手动触发工作流
首次配置后，手动触发两个工作流：
1. 进入 Actions 页面
2. 选择 "Generate Snake Animation" -> Run workflow
3. 选择 "Generate GitHub Metrics" -> Run workflow

### 6. 验证
工作流运行成功后，你的 GitHub 主页应该会显示：
- 贪吃蛇动画（在 contribution graph 上方）
- 左侧统计信息（header、activity、community）
- 右侧统计信息（编程语言、收藏的仓库、话题）

## 自定义选项

### 修改贪吃蛇动画频率
在 `.github/workflows/snake.yml` 中修改 cron 表达式：
```yaml
schedule:
  - cron: "0 0 * * *"  # 每天UTC时间0点运行
```

### 修改 Metrics 生成频率
在 `.github/workflows/metrics.yml` 中修改 cron 表达式：
```yaml
schedule:
  - cron: "0 */6 * * *"  # 每6小时运行一次
```

### 修改 Metrics 统计内容
在 `.github/workflows/metrics.yml` 中修改 `base` 和 `plugin_*` 参数，详见 [lowlighter/metrics 文档](https://github.com/lowlighter/metrics)

## 参考资料

- [Platane/snk - GitHub Contribution Grid Snake Animation](https://github.com/Platane/snk)
- [lowlighter/metrics - GitHub Metrics](https://github.com/lowlighter/metrics)
- [GitHub Profile README Guide](https://docs.github.com/en/account-and-workflows/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme)

## 配置完成后的效果

配置完成后，你的 GitHub 主页应该会显示：

1. **贪吃蛇动画**：在 contribution graph 上方，一条蛇会"吃掉"你的贡献方块
2. **左侧统计信息**：header、activity、community、贡献日历等
3. **右侧统计信息**：编程语言统计、收藏的仓库、话题等

## 故障排除

### Q: 为什么贪吃蛇动画没有显示？
A: 确保：
1. `snake.yml` 工作流已成功运行
2. `output` 分支已创建并包含 `github-contribution-grid-snake.svg` 文件
3. README.md 中的 SVG 链接指向正确的分支

### Q: 为什么 Metrics 图片没有显示？
A: 确保：
1. `METRICS_TOKEN` 已正确配置
2. `metrics.yml` 工作流已成功运行
3. 仓库中已生成 `metrics.left.svg` 和 `metrics.right.svg` 文件

### Q: 如何修改贡献日历的颜色主题？
A: 在 `.github/workflows/snake.yml` 中添加 `palette` 参数：
```yaml
outputs: |
  dist/github-contribution-grid-snake.svg
  dist/github-contribution-grid-snake-dark.svg?palette=github-dark
```

## 参考资料

- [Platane/snk - GitHub Contribution Grid Snake Animation](https://github.com/Platane/snk)
- [lowlighter/metrics - GitHub Metrics](https://github.com/lowlighter/metrics)
- [GitHub Profile README Guide](https://docs.github.com/en/account-and-workflows/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme)
