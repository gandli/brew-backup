# 🍺 brew-backup

我的 Homebrew 软件包备份，由 [brew-sync](https://github.com/kyungw00k/brew-sync) 管理。

## 当前配置

| 类型 | 数量 |
|------|------|
| Tap | 21 |
| Formula | 79 |
| Cask | 57 |
| **总计** | **157** |

## 快速恢复

在新 Mac 上一键恢复所有软件包：

```bash
# 1. 安装 brew-sync
curl -fsSL https://raw.githubusercontent.com/kyungw00k/brew-sync/main/install.sh | bash

# 2. 克隆备份仓库
git clone https://github.com/gandli/brew-backup ~/.brew-sync-git

# 3. 配置 Git 存储
mkdir -p ~/.config/brew-sync
cat > ~/.config/brew-sync/last_storage << 'EOF'
method=git
path="$HOME/.brew-sync-git"
EOF
echo "default" > ~/.config/brew-sync/default_profile

# 4. 恢复
brew-sync restore
```

或者直接用 Brewfile：

```bash
git clone https://github.com/gandli/brew-backup
cd brew-backup/profiles/default
brew bundle install --file=Brewfile
```

## 备份

```bash
brew-sync backup
cd ~/.brew-sync-git && git add . && git commit -m "backup $(date +%F)" && git push
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
