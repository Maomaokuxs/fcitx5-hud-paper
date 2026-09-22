# fcitx5-hud-paper

Waybar HUD 风格的 fcitx5 皮肤（浅 + 深两套），颜色跟随壁纸中央色板动态生成。

A Waybar-HUD-style fcitx5 theme (light + dark) generated from a central wallpaper palette.

## 特点

- 0.8 等比缩放（`FCITX_SCALE` 可覆盖），9px 圆角，横排候选
- 高亮块吃壁纸 accent 色，字色按对比度自动挑黑/白
- 选中块与外框四边 2px 隔离缝；候选序号永不被高亮盖住
- 字底对比度兜底（WCAG 4.5）：任何壁纸都撞不出看不清的搭配
- 纯 `<path>` SVG + 九宫格边距，无滤镜（fcitx5 classic UI 渲染器兼容）
- 原子换装 + `fcitx5 -r` 彻底重载：重载永远读到完整的一代，不混版

## 目录结构

```
sync-waybar.sh      # 生成器：读色板 → 生成两套主题 → 可选安装重载
hud-paper/          # 浅色主题（theme.conf + panel.svg + highlight.svg）
hud-paper-dark/     # 深色主题（同上，纯黑底）
```

## 使用

```bash
# 只生成（输出到同目录 hud-paper{,-dark}/）
bash sync-waybar.sh

# 生成 + 安装到 ~/.local/share/fcitx5/themes/ + 彻底重载 fcitx5
bash sync-waybar.sh --install
```

然后在 `fcitx5-configtool` → 插件 → Classic UI 里选主题 `Hud Paper`，
深色主题选 `Hud Paper Dark`（或都填 `hud-paper`、关掉 `UseDarkTheme`，
只留取色脚本这一个深浅大脑，见下）。

建议字体：`Font="Sans 9"`，`MenuFont="Sans 8"`。

## 取色来源（优先级从高到低）

1. `$PALETTE_FILE` / `~/.cache/by-mgr/hellwal/global-palette.env`（中央库：`BG FG ACCENT MUTED COLOR0…`）
2. `$WAYBAR_CSS` / `~/.config/waybar/color-waybar.css`（兜底）

另可用环境变量覆盖：`FCITX_SCALE`（默认 0.8）、`XDG_DATA_HOME`（安装位置）。

## 换壁纸自动跟色

在取色脚本（如 `theme-sync.sh`）末尾加一段即可：

```bash
FCITX_SYNC="${FCITX_SYNC:-$HOME/Documents/fcitx5/sync-waybar.sh}"
if [ -x "$FCITX_SYNC" ]; then
    WAYBAR_CSS="$WAYBAR_DIR/color-waybar.css" bash "$FCITX_SYNC" --install \
        && echo "   Fcitx5 皮肤已跟随配色" \
        || echo "   ( Fcitx5 皮肤同步跳过)"
fi
```

## 要求

- fcitx5 5.x（classic UI）
- `bash`、`python3`（生成器仅依赖这两样）

## 协议

MIT，见 [LICENSE](LICENSE)。
