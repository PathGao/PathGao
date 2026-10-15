# Claude 的 Dock 点击限制与处理

本说明由 [CLAUDE.md](../CLAUDE.md) 引用，不作为 Codex 的操作前置条件。

## 已知范围

2026-09-24，用户提供的 macOS 27.0 实测结果，Dock 保持可见：

| Claude 操作方式 | 显示器 | 结果 |
| --- | --- | --- |
| 后台模式 `app_click`、`app_type` | 任意屏幕 | 点击、打字和按钮操作成功 |
| 全屏控制 `left_click` | Mi Monitor，无 Dock | 点击和打字成功 |
| 全屏控制 | 内建屏，Dock 所在屏幕 | 点击被拒，报错包含 `would land on 程序坞` |

同日 Codex 在 macOS 27.0（26A428）使用 `cua_repl` 的原生 `App.click([x, y])`，在 Mi Monitor 和内建屏分别点击文本编辑的字体下拉框，两次均观察到菜单打开。测试前后 Dock 的 `autohide` 均为 `0`，未改文档内容，窗口已移回原来的外接屏。此次验证覆盖坐标点击和菜单操作，未验证所有输入方式。

## 根因

Dock 可见时，每台 Mac 都有一个 Dock 拥有的 layer 20 全屏透明窗口。Claude 的全屏点击门按窗口的 owner 显示名跳过系统窗口，名单是英文的（`Dock`、`Control Center` 等）。中文系统里这个窗口的 owner 名是 `程序坞`，不在名单里，于是整块屏的点击都被判成落在未授权的程序坞上。英文系统不受影响。

2026-09-30 在 macOS 27.0.1（26A434）、Claude 2.9939.4 上对照实测，Dock 可见，内建屏同一坐标点击文本编辑：Dock 切英文后 owner 名变成 `Dock`，点击和打字成功；切回中文立刻被拒。上游跟踪在 anthropics/claude-code#50719。

## 处理方法

默认不改 Dock。优先使用 Claude 后台模式，或将目标窗口移到没有 Dock 的屏幕。

必须在 Dock 所在屏幕使用全屏控制、且实际出现上述拒绝时，只把 Dock 这一个 app 的语言切成英文。Dock 保持可见，系统其他部分仍是中文，副作用只是 Dock 的右键菜单等文字变成英文，Dock 会重启一次。

切英文：

```bash
defaults write com.apple.dock AppleLanguages -array en && killall Dock
```

用完恢复（本机原来没有这个键，删除即恢复）：

```bash
defaults delete com.apple.dock AppleLanguages && killall Dock
```

隐藏 Dock（`autohide`）也能绕开，但会改变用户日常看到的界面，只在上面的方法失效时使用，用完恢复原值。
