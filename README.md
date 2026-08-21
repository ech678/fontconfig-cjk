# fontconfig-cjk

A refined Fontconfig configuration optimized for standard-density displays (27-inch 2K / 2560x1440, ~109 PPI) and CJK typography on Linux.

## Features

- **2K Display Optimization**: Tailored for ~109 PPI panels with RGB subpixel rendering, slight hinting (`hintslight`), FreeType v40 interpreter, and LCD default filtering to ensure crisp text legibility without glyph distortion.
- **Weight Compensation**: Prioritizes Medium weight variants for UI and body text to prevent thin or washed-out strokes on non-HiDPI screens.
- **CJK Glyph Variant Lock**: Enforces `zh-CN` locale rules to eliminate cross-region font substitution and glyph bleeding.
- **Font Aliasing**: Transparent drop-in fallback mappings for common Windows, macOS, and Web typefaces (Arial, Segoe UI, Microsoft YaHei, SimSun, SimHei, KaiTi).

## Recommended Fonts

- **UI / Sans**: Noto Sans, Noto Sans CJK SC (Medium)
- **Serif**: Noto Serif, Noto Serif CJK SC (Medium)
- **Monospace**: JetBrains Mono / JetBrainsMono Nerd Font, Noto Sans Mono CJK SC
- **Script / Kaiti**: LXGW WenKai
- **Emoji**: Noto Color Emoji

## Quick Setup

```bash
mkdir -p ~/.config/fontconfig
ln -sf "$(pwd)/fonts.conf" ~/.config/fontconfig/fonts.conf
fc-cache -fv
```

## Verification

```bash
fc-match sans-serif
fc-match monospace
fc-match "Microsoft YaHei"
```

## License

MIT
