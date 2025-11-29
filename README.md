# brew-sync

> 在多台 Mac 之间同步你的 Homebrew 软件包

## 它能做什么？

**问题**：你有多台 Mac，希望在所有设备上拥有相同的应用程序和开发工具。

**解决方案**：两个简单命令，即可备份和恢复全部内容。

```bash
# 在第一台 Mac 上
brew-sync backup

# 在第二台 Mac 上  
brew-sync restore
```

就这么简单！你所有的 Homebrew 软件包、GUI 应用和 Mac App Store 应用现在都已同步。

---

## 核心功能

- **智能初始化**：首次运行提供交互式引导，带智能默认值
- **多 Mac 同步**：按“配置文件”（profile）组织，适应不同使用场景  
- **灵活存储**：支持 iCloud、Dropbox、Google Drive、Git 或仅本地存储
- **本地模式**：使用 `.brew-sync` 文件夹，适合单 Mac 或注重隐私的用户
- **全面覆盖**：Homebrew + Cask 应用 + Mac App Store 应用  
- **配置文件**：可为工作、个人、开发等不同需求设置独立配置
- **机器专属默认配置**：自动加载每台 Mac 的默认配置文件
- **安全的配置编辑**：修改软件包列表时提供差异预览和确认提示
- **备份历史**：查看并可回滚到之前的备份版本
- **交互式回滚**：通过编号菜单选择历史版本进行恢复
- **简洁 CLI 界面**：默认输出简洁，加 `--verbose` 显示详细信息
- **智能帮助系统**：上下文感知的帮助信息，`--help` 支持大多数命令
- **预览模式**：`--dry-run` 可预览将要安装或恢复的内容
- **历史管理**：自动清理旧备份，保留策略可配置
- **简单易用**：只需 `backup` 和 `restore`，自动检测一切
- **自动迁移**：已有的基于主机名的配置会自动迁移到新的 profile 系统

---

## 安装

```bash
curl -fsSL https://raw.githubusercontent.com/kyungw00k/brew-sync/main/install.sh | bash
```

---

## 基本用法

### 首次运行（交互式设置）
```bash
# 首次运行时，brew-sync 会引导你完成设置
brew-sync backup

# 🍺 欢迎使用 brew-sync！
# 请选择备份存储位置：
#
#  1) Git 仓库 🔄 支持版本控制和手动同步
#  2) iCloud Drive（多 Mac 推荐）📱 
#  3) 本地存储 (.brew-sync) 💻 仅保存在本机
#
# 选择存储方式（1-3，回车默认选 2）： 
#
# 存储设置完成后，你会配置默认配置文件：
#
# 🔧 配置文件设置
#
# 发现已有配置文件：
# 配置文件（icloud）：
# default      2024-12-15T14:32:10+09:00    brew(85) cask(15) mas(12)
# work         2024-12-14T09:15:42+09:00    brew(67) cask(8) mas(5)
#
# 设置选项：
# 1) 使用 'default' 配置文件（推荐）
# 2) 从上述列表中选择已有配置文件
# 3) 基于主机名 'MacBook-Pro' 创建新配置文件
# 4) 输入自定义配置文件名
#
# 选择选项（1-4）： 
```

### 日常使用
```bash
# 备份软件包（默认简洁输出）
brew-sync backup
brew-sync backup --verbose     # 显示详细进度

# 恢复软件包（强烈建议先预览！）
brew-sync restore --dry-run    # 预览（推荐）
brew-sync restore              # 实际安装
brew-sync restore --verbose    # 显示安装详情

# 使用配置文件区分不同环境
brew-sync backup work
brew-sync restore work

# 注意：未指定配置文件的命令会使用你的默认配置
# （在 ~/.config/brew-sync/default_profile 中按机器配置）

# 配置文件管理命令
brew-sync status                   # 列出所有配置文件
brew-sync status work              # 显示 work 配置详情
brew-sync edit work                # 安全编辑 work 配置
brew-sync edit                     # 编辑默认配置或选择配置文件
brew-sync set work                 # 将 work 设为默认配置
brew-sync remove old               # 删除旧配置（'default' 除外）

# 历史记录与回滚
brew-sync history                  # 查看默认配置备份历史
brew-sync history work             # 查看指定配置备份历史
brew-sync rollback                 # 对默认配置进行交互式回滚
brew-sync rollback work 2          # 将 work 配置回滚到第 2 条备份
brew-sync update                   # 更新到最新版本
brew-sync update --check           # 仅检查更新
brew-sync uninstall                # 卸载 brew-sync
brew-sync help                     # 获取帮助

# 获取特定命令的详细帮助  
brew-sync backup --help           # 显示 backup 选项和用法
brew-sync restore --help          # 显示 restore 选项和用法
brew-sync <command> --help        # 多数命令支持 --help

# 支持 --help 的命令（13 个中有 10 个）：
# backup, restore, status, history, rollback, edit, set, remove, cleanup, update
# 注：'help' 显示所有命令，'uninstall' 和已弃用的 'profile' 不支持 --help
```

---

## 配置文件编辑（Edit）

`profile edit` 功能让你以差异对比的方式安全地修改软件包列表：

```bash
# 编辑 work 配置文件
$ brew-sync edit work
正在编辑配置文件 'work'

按任意键继续...

# 在编辑器中保存后：
Brewfile 编辑成功

将对配置文件 'work' 应用的更改：

安装：
  + brew "htop"
  + cask "notion"

移除：
  - brew "bat"  

应用这些更改？[y/N]: y
正在安装 brew 包：htop
正在安装 cask：notion  
正在移除 brew 包：bat
软件包更改已成功应用
配置文件 'work' 已更新
配置文件 'work' 同步成功
```

### 主要优势
- **安全编辑**：编辑的是临时副本，不影响实际配置
- **智能预览**：在应用前清楚看到将要安装/移除的内容
- **用户掌控**：任何系统修改前均需确认
- **精准操作**：仅安装/移除你明确修改的软件包
- **无意外**：不会激进清理无关软件包

### 使用模式
```bash
# 编辑指定配置文件
brew-sync edit work

# 编辑默认配置（若无则引导选择）
brew-sync edit

# 典型工作流程
brew-sync edit work        # 进行修改
# 仔细查看预览
# 输入 'y' 确认或 'N' 取消
```

---

## 备份历史与恢复

brew-sync 自动维护完整的备份历史，方便你从误操作或配置错误中恢复。

### 查看备份历史
```bash
# 查看默认配置的备份历史
brew-sync history

# 查看 work 配置的备份历史
brew-sync history work

# 输出示例：
# 'default' 的备份历史：
# [1] 2天前 (103 个包)
# [2] 5天前 (98 个包)
# [3] 1周前 (95 个包)
# [4] 2周前 (92 个包)
# [5] 3周前 (90 个包)
```

### 回滚到历史版本
```bash
# 对默认配置交互式回滚 - 显示菜单选择
brew-sync rollback
# 选择备份 [1-5]: 2

# 对 work 配置交互式回滚 - 显示菜单选择
brew-sync rollback work
# 选择备份 [1-5]: 2

# 直接回滚到指定备份
brew-sync rollback work 2

# 预览回滚操作（不实际更改）
brew-sync rollback work 2 --dry-run
```

### 备份管理
```bash
# 预览备份变更
brew-sync backup work --dry-run

# 清理旧备份（每配置保留最近 5 次）
brew-sync cleanup --keep-history 5

# 预览清理操作
brew-sync cleanup --keep-history 3 --dry-run
```

### 安全特性
- **自动备份**：回滚前自动备份当前状态
- **用户确认**：任何更改前均会提示确认
- **预览模式**：`--dry-run` 精确展示将要执行的操作
- **智能保留**：可配置的保留策略防止存储无限增长

---

## 实际使用案例

```bash
# 工作 MacBook：备份开发环境（简洁输出）
$ brew-sync backup work
备份完成 (67 个包)

# 带详细输出：
$ brew-sync backup work --verbose
使用上次保存的存储：icloud (/Users/john/Library/Mobile Documents/com~apple~CloudDocs/brew-backup)
使用配置：work
  开始为 'work' 配置在主机 'MacBook-Pro' 上备份
  生成当前软件包列表...
  软件包列表生成成功
备份完成 (67 个包)
  位置：/Users/john/Library/Mobile Documents/com~apple~CloudDocs/brew-backup/profiles/work
  软件包：67 brew, 8 cask, 5 mas

# 家用 iMac：恢复相同环境  
$ brew-sync restore work --dry-run
=== 干跑模式 - 不会实际安装 ===

  实际安装命令：brew-sync restore --profile work
恢复来源：配置文件 'work'
Brewfile 路径：/Users/john/Library/Mobile Documents/com~apple~CloudDocs/brew-backup/profiles/work/Brewfile
配置信息：
  最后备份时间：2024-12-14T09:15:42+09:00
  来源主机：MacBook-Pro
  软件包：brew(67) cask(8) mas(5)

待恢复软件包信息：
  - Homebrew 包：67
  - Cask 应用：8
  - Mac App Store 应用：5

[干跑] 正在复制 Brewfile 到临时目录
[干跑] 需要安装的软件包：
[干跑] 将执行以下命令：
[干跑] brew bundle --file="/var/folders/ab/1234567890/T/tmp.abc123def/Brewfile" 
[干跑] 如需实际安装，请去掉 -d 参数重新运行

推荐命令：
  实际安装：brew-sync restore --profile work
已清理临时文件

$ brew-sync restore work
恢复成功完成！

# 带详细输出：
$ brew-sync restore work --verbose  
开始从 /var/folders/ab/1234567890/T/tmp.abc123def/Brewfile 安装软件包
执行：brew bundle --file="/var/folders/ab/1234567890/T/tmp.abc123def/Brewfile" --no-upgrade
使用 node, python, docker, git, vscode, slack, cursor...
`brew bundle` 完成！Brewfile 中的 80 个依赖现已安装。
恢复成功完成！
```

---

## 常用选项

### 存储选项
```bash
# 使用你已配置的存储（默认）
brew-sync backup                    # 使用保存的偏好设置

# 临时覆盖存储（不改变默认设置）
brew-sync backup --icloud           # iCloud Drive（临时）
brew-sync backup --dropbox          # Dropbox（临时）
brew-sync backup --google-drive     # Google Drive（临时）
brew-sync backup --git              # Git 仓库（临时）
brew-sync backup --path ~/backup    # 自定义路径（临时）
brew-sync backup --path /Volumes/MyUSB/brew-backup  # U盘（临时）

# 更改你的默认存储
brew-sync backup --select-storage   # 交互式选择（保存为新默认）
```

### 恢复选项
```bash
# 恢复哪个配置
brew-sync restore                   # 默认配置备份（加载机器专属默认）
brew-sync restore work    # work 配置备份
brew-sync restore dev     # dev 开发配置备份

# 从哪里恢复（临时覆盖）
brew-sync restore work --icloud # 从 iCloud 恢复 work 配置
brew-sync restore dev --git     # 从 Git 恢复 dev 配置

# 预览和更改默认
brew-sync restore --dry-run         # 仅预览（推荐）
brew-sync restore --select-storage  # 选择存储并执行恢复
```

### 详细输出
```bash
# 显示详细进度信息
brew-sync backup --verbose          # 详细备份过程  
brew-sync restore --verbose         # 详细安装过程
brew-sync edit work --verbose    # 详细编辑过程

# 与其他选项组合
brew-sync backup work --verbose
brew-sync restore --dry-run --verbose
```

### 更新选项
```bash
brew-sync update                     # 更新到最新版本
brew-sync update --check            # 仅检查更新
```

---

## 系统要求

### 必需
- macOS 10.12+
- Homebrew（建议使用最新版）

### 可选（取决于你的选择）
- **多 Mac 同步**：iCloud Drive、Dropbox、Google Drive、OneDrive 或 Git 仓库
- **单 Mac 使用**：无额外要求（使用本地 `~/.brew-sync` 存储）
- **现有用户**：你当前的配置将继续正常工作，无需改动

---

## 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。
