# Terminal Configuration Guide

> Agent 专用配置指南：按照本文件完成 Ghostty 终端 + Zsh 增强配置

## 概述

| 组件 | 用途 |
|------|------|
| Ghostty | 终端模拟器 |
| Powerlevel10k | Zsh 主题（彩色分段提示符） |
| zsh-autosuggestions | 历史命令自动补全（灰色提示，按 → 采纳） |
| zsh-syntax-highlighting | 命令语法高亮（正确绿色，错误红色） |

---

## 第一步：安装依赖

```bash
# 安装 Zsh 增强插件
brew install zsh-autosuggestions zsh-syntax-highlighting powerlevel10k
```

---

## 第二步：配置 Ghostty

配置文件路径：`~/.config/ghostty/config`

```ini
# 字体配置
font-family = "Sarasa Mono SC"
font-size = 14
font-feature = ss01
font-feature = ss02
adjust-cell-height = 20%
font-thicken = true

# 主题 - Catppuccin Mocha (深色主题，颜色丰富)
theme = catppuccin-mocha

# 窗口样式
window-padding-x = 12
window-padding-y = 8
window-decoration = true
macos-titlebar-style = tabs

# 光标样式
cursor-style = block
cursor-style-blink = true

# 背景透明度 (1.0 = 不透明)
background-opacity = 0.95

# 开启真彩色
bold-is-bright = true

# 滚动
scrollback-limit = 100000

# 复制粘贴
clipboard-read = allow
clipboard-write = allow
copy-on-select = clipboard

# 分屏快捷键
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down

# Shell 集成
shell-integration = zsh
```

---

## 第三步：配置 Zsh (~/.zshrc)

在 `~/.zshrc` 末尾添加以下内容：

```zsh
# ============ Zsh 增强配置 ============

# Powerlevel10k instant prompt（必须放在 .zshrc 最顶部）
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# Powerlevel10k 主题
source /opt/homebrew/share/powerlevel10k/powerlevel10k.zsh-theme

# 自动补全建议（输入时显示灰色历史命令，按 → 采纳）
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# 语法高亮（正确命令绿色，错误命令红色）
source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# Powerlevel10k 配置文件
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```

**注意**：Powerlevel10k instant prompt 代码块必须放在 `.zshrc` 的最顶部。

---

## 第四步：配置 Powerlevel10k 主题

重启终端后会自动弹出配置向导。如需重新配置：

```bash
p10k configure
```

向导会询问：
1. 能否看到特定图标（菱形、锁、箭头）
2. 提示符风格（经典/彩虹/精简）
3. 显示内容（时间、git 状态、目录层级等）

---

## 配置效果

- **提示符**：用户名、目录、Git 状态各自不同颜色
- **自动补全**：输入时显示灰色历史命令建议，按 `→` 键采纳
- **语法高亮**：正确命令绿色，错误/不存在的命令红色
- **终端外观**：Catppuccin Mocha 深色主题，95% 透明度
- **字体**：Sarasa Mono SC（更纱黑体），代码与中文显示统一协调，开启 font-thicken
- **分屏**：`Cmd+D` 右侧分屏，`Cmd+Shift+D` 下方分屏
- **滚动**：10 万行回滚缓冲区

---

## 常用命令

| 命令 | 说明 |
|------|------|
| `p10k configure` | 重新配置 Powerlevel10k 主题样式 |
| `source ~/.zshrc` | 重新加载 Zsh 配置 |

---

## 故障排查

### 图标显示为方块
安装 Nerd Font 字体：
```bash
brew install --cask font-meslo-lg-nerd-font
```
然后在 Ghostty 配置中修改：
```ini
font-family = "MesloLGS NF"
```

### 安装 Sarasa Mono SC 字体
```bash
brew install --cask font-sarasa-gothic
```

### Intel Mac 路径不同
将 `/opt/homebrew/` 替换为 `/usr/local/`
