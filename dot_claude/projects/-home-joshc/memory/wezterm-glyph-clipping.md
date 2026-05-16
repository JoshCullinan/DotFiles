---
name: wezterm-glyph-clipping
description: "User's Linux WezTerm setup rasterises glyphs clipped at font sizes below 14pt — known dead-end, don't re-bisect"
metadata: 
  node_type: memory
  type: user
  originSessionId: 84f2def3-91a6-4ba5-9439-8d35ab72a9e2
---

On the user's Linux box (Ubuntu 26.04, Wayland, GNOME 1.15× text-scaling-factor, 4480×1440 display), WezTerm clips glyph descenders/ascenders at any `font_size` below ~14pt. The glyph *bitmap* is rasterised clipped — `line_height` does not fix it.

`~/.wezterm.lua` is chezmoi-managed and deployed to both Linux and macOS. The `font_size = 14` comment explains why; don't "tidy it up" back to 13.5 without testing on Linux.

**Why:** Investigated 2026-05-16 across 8 bisection rounds. All of the following were tested and ruled out: DPI override, WebGpu vs OpenGL `front_end`, `enable_wayland = false`, GNOME `text-scaling-factor = 1.0`, WezTerm 20240203 → 20260331 nightly, `line_height` up to 1.6, `JetBrainsMono Nerd Font` vs `... Nerd Font Mono` vs unpatched `JetBrains Mono`, and various freetype hint/render targets (Light/Normal/HorizontalLcd/NO_HINTING). None resolved the clipping; only raising `font_size` did.

**How to apply:** If the user reports WezTerm font rendering issues again, or asks about changing `font_size`, do not repeat the rule-outs above. The pragmatic answer is `font_size = 14` (or higher). The unresolved upstream lead is a WezTerm GitHub issue with GPU/driver info; the user has not filed one. macOS deployments of the same dotfile are unaffected and can use any size.
