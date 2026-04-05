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

# 主题 - 跟随系统明暗自动切换
theme = light:Apple System Colors Light,dark:Cursor Dark
window-theme = auto

# 窗口样式
window-padding-x = 12
window-padding-y = 8
macos-titlebar-style = transparent
window-save-state = always
quit-after-last-window-closed = true

# 光标样式
cursor-style = block
cursor-style-blink = true
cursor-opacity = 0.8

# 背景透明度 + 毛玻璃
background-opacity = 0.95
background-blur-radius = 128

# 开启真彩色
bold-is-bright = true

# 滚动
scrollback-limit = 25000000

# 鼠标和剪贴板
mouse-hide-while-typing = true
clipboard-read = allow
clipboard-write = allow
copy-on-select = clipboard
clipboard-paste-protection = true
clipboard-paste-bracketed-safe = true

# Quick Terminal (全局快捷键呼出)
quick-terminal-position = top
quick-terminal-screen = mouse
quick-terminal-autohide = true
quick-terminal-animation-duration = 0.15

# Shell 集成
shell-integration = zsh
shell-integration-features = no-cursor

# 分屏快捷键
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down
keybind = cmd+shift+enter=toggle_split_zoom
keybind = cmd+shift+f=toggle_split_zoom
keybind = shift+enter=text:\x1b\r
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

- **主题**：跟随系统明暗自动切换（亮色 Apple System Colors Light / 暗色 Cursor Dark）
- **毛玻璃**：95% 透明度 + 背景模糊效果
- **标题栏**：透明风格，沉浸感更强
- **Quick Terminal**：全局快捷键从屏幕顶部滑出终端（0.15 秒动画）
- **提示符**：Powerlevel10k 彩色分段，显示用户名、目录、Git 状态
- **自动补全**：输入时显示灰色历史命令建议，按 `→` 键采纳
- **语法高亮**：正确命令绿色，错误/不存在的命令红色
- **字体**：Sarasa Mono SC（更纱黑体），代码与中文显示统一协调
- **分屏**：`Cmd+D` 右侧分屏，`Cmd+Shift+D` 下方分屏，`Cmd+Shift+Enter` 最大化分屏
- **滚动**：2500 万行回滚缓冲区
- **安全**：打字时隐藏鼠标、剪贴板粘贴保护
- **窗口恢复**：重启后自动恢复窗口状态

---

## 快捷键速查

| 快捷键 | 功能 |
|------|------|
| `Cmd+D` | 右侧分屏 |
| `Cmd+Shift+D` | 下方分屏 |
| `Cmd+Shift+Enter` | 最大化/还原当前分屏 |
| `Cmd+Shift+F` | 最大化/还原当前分屏（同上） |
| `Cmd+[` / `]` | 切换分屏 |
| `Cmd+Alt+方向键` | 跳转到指定方向的分屏 |
| `Cmd+T` | 新标签页 |
| `Cmd+1~9` | 切换标签页 |
| `Cmd+Shift+P` | 命令面板 |
| `Cmd+,` | 打开配置文件 |
| `Cmd+Shift+,` | 重新加载配置 |
| `Shift+Enter` | 发送换行（不执行命令） |

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
