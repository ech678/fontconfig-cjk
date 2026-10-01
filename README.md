# fontconfig-cjk

A refined Fontconfig configuration optimized for standard-density displays (27-inch 2K / 2560x1440, ~109 PPI) and CJK typography on Linux.

## Features

- **2K Display Optimization**: Tailored for ~109 PPI panels with RGB subpixel rendering, slight hinting (`hintslight`), FreeType v40 interpreter, and LCD default filtering to ensure crisp text legibility without glyph distortion.
- **Weight Compensation**: Promotes Regular (400) weight to Medium (500) at the `target=font` level for Noto Sans/Serif families, preventing thin or washed-out strokes on non-HiDPI screens while preserving full Bold/Italic/Light weight spectrum.
- **CJK Glyph Variant Lock**: Enforces `zh-CN` locale rules to eliminate cross-region font substitution and glyph bleeding.
- **Font Aliasing**: Transparent drop-in fallback mappings for common Windows, macOS, and Web typefaces (Arial, Segoe UI, Microsoft YaHei, SimSun, SimHei, KaiTi).

## Recommended Fonts

- **UI / Sans**: Noto Sans, Noto Sans CJK SC (Medium)
- **Serif**: Noto Serif, Noto Serif CJK SC (Medium)
- **Monospace**: JetBrains Mono / JetBrainsMono Nerd Font, Noto Sans Mono CJK SC
- **Script / Kaiti**: LXGW WenKai
- **Emoji**: Noto Color Emoji

## Quick Setup

### 1. Fontconfig

```bash
mkdir -p ~/.config/fontconfig
ln -sf "$(pwd)/fonts.conf" ~/.config/fontconfig/fonts.conf
fc-cache -fv
```

### 2. Cross-Toolkit Sync

Fontconfig alone only covers apps that directly read it (Qt, Chromium, Alacritty, Foot, etc.). GTK3/4, XWayland legacy apps, and Electron each need separate configuration to achieve system-wide visual consistency.

#### GTK4 (fixes blurry text in Libadwaita / GNOME apps)

`~/.config/gtk-4.0/settings.ini`:

```ini
[Settings]
gtk-font-name = Noto Sans 10
gtk-font-rendering = manual
gtk-hint-font-metrics = 1
```

> GTK4 disables subpixel AA and uses subpixel glyph positioning by default, which causes blurry text on low-PPI screens. `gtk-hint-font-metrics=1` forces metric grid snapping to restore clarity.

#### GTK3

`~/.config/gtk-3.0/settings.ini`:

```ini
[Settings]
gtk-font-name = Noto Sans 10
gtk-xft-antialias = 1
gtk-xft-hinting = 1
gtk-xft-hintstyle = hintslight
gtk-xft-rgba = rgb
```

#### GSettings (dconf)

```bash
gsettings set org.gnome.desktop.interface font-name 'Noto Sans 10'
gsettings set org.gnome.desktop.interface font-antialiasing 'rgba'
gsettings set org.gnome.desktop.interface font-hinting 'slight'
```

#### XWayland (legacy X11 apps via xsettingsd)

`~/.config/xsettingsd/xsettingsd.conf`:

```
Xft/Antialias 1
Xft/Hinting 1
Xft/HintStyle "hintslight"
Xft/RGBA "rgb"
Xft/lcdfilter "lcddefault"
Xft/DPI 98304
```

> `98304` = 96 × 1024. The DPI value uses 1024-based fixed-point encoding.

Add to your compositor startup (e.g. niri config):

```
spawn-at-startup "xsettingsd"
```

#### Electron / Chromium (native Wayland)

`~/.config/environment.d/electron-wayland.conf`:

```
ELECTRON_OZONE_PLATFORM_HINT=auto
```

> Forces Electron apps (VS Code, Discord, etc.) to run natively on Wayland instead of through XWayland, avoiding scaling-induced blur.

## Verification

```bash
fc-match sans-serif              # → Noto Sans Regular
fc-match sans-serif:bold          # → Noto Sans Bold
fc-match monospace                # → JetBrainsMono Nerd Font Regular
fc-match "Microsoft YaHei"       # → Noto Sans CJK SC Regular
```

## License

MIT
