# 🍺 brew-backup

我的 Homebrew 软件包备份，由 [brew-sync](https://github.com/kyungw00k/brew-sync) 管理。

## 当前配置

| 类型 | 数量 |
|------|------|
| Tap | 21 |
| Formula | 79 |
| Cask | 57 |
| **总计** | **157** |

## 快速开始

### 1. 安装 brew-sync

```bash
curl -fsSL https://raw.githubusercontent.com/kyungw00k/brew-sync/main/install.sh | bash
```

### 2. 克隆备份仓库

```bash
git clone https://github.com/gandli/brew-backup ~/.brew-sync-git
```

### 3. 配置 Git 存储

```bash
mkdir -p ~/.config/brew-sync
cat > ~/.config/brew-sync/last_storage << 'EOF'
method=git
path="$HOME/.brew-sync-git"
EOF
echo "default" > ~/.config/brew-sync/default_profile
```

### 4. 恢复所有软件包

```bash
brew-sync restore
```

> 💡 也可以跳过 brew-sync，直接用 Brewfile：
> ```bash
> cd ~/.brew-sync-git/profiles/default && brew bundle install
> ```

## 日常使用

### 备份

```bash
# 备份当前已安装的软件包
brew-sync backup

# 推送到 GitHub
cd ~/.brew-sync-git && git add . && git commit -m "backup $(date +%F)" && git push
```

### 恢复

```bash
# 先预览（推荐）
brew-sync restore --dry-run

# 确认无误后恢复
brew-sync restore
```

### 查看状态

```bash
# 查看配置文件概览
brew-sync status

# 查看备份历史
brew-sync history
```

### 回滚

```bash
# 交互式选择历史版本
brew-sync rollback

# 直接回滚到第 2 个备份
brew-sync rollback default 2

# 先预览
brew-sync rollback default 2 --dry-run
```

### 编辑软件包列表

```bash
# 安全编辑（带差异预览和确认）
brew-sync edit
```

## 目录结构

```
profiles/
└── default/
    ├── Brewfile                 # 当前软件包列表
    └── Brewfile_YYYYMMDD_*     # 历史备份
```

## 设备

| 设备 | 最后备份 |
|------|----------|
| Mac mini (Apple Silicon) | 2026-02-16 |
| MacBook Air | 2026-02-01 |

---

Powered by [brew-sync](https://github.com/kyungw00k/brew-sync) · MIT License
