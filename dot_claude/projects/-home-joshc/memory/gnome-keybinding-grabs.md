---
name: gnome-keybinding-grabs
description: "GNOME on user's Linux box grabs SUPER+* and Ctrl+(Shift+)Alt+Arrow chords at compositor level — apps never see them. Informs WezTerm and any other Linux app keymap design."
metadata:
  type: user
---

On the user's Linux box (Ubuntu 26.04, Wayland, GNOME — see [[wezterm-glyph-clipping]] for full env), the compositor intercepts a long list of chords before any application sees the keystroke. Confirmed via `gsettings list-recursively org.gnome.desktop.wm.keybindings` on 2026-05-16:

- **`Super+1..9`** — switch to workspace N
- **`Super+<Arrow>`** — window tiling / snap
- **`Super+D`** — show desktop
- **`Super+H`** — hide window
- **`Ctrl+Alt+<Arrow>`** — workspace switch
- **`Ctrl+Shift+Alt+<Arrow>`** — `move-to-workspace-{left,right,up,down}`

**Why:** These are GNOME WM defaults on this user's install (not removed). Wayland routes them at the compositor before the focused app's keygrabber runs, so WezTerm/etc. literally never sees the key event. No amount of in-app binding fixes this — only `gsettings set ... move-to-workspace-* "[]"` (or equivalent) frees the chord.

**How to apply:** When designing or proposing Linux keybindings for terminal/editor/app configs the user deploys here, do not suggest any of the above chords as primary bindings unless the user wants to unbind GNOME first. This is why [[wezterm-glyph-clipping|the WezTerm config]] uses `Ctrl+Shift` as its Linux primary mod and routes pane resize through `Ctrl+Shift+Arrow` (the obvious `Ctrl+Alt+Arrow` and `Ctrl+Shift+Alt+Arrow` are both grabbed). macOS deploys of the same dotfile are unaffected.
