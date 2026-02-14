# Aurora Corne — ZMK Config

Personal ZMK firmware configuration for a [splitkb Aurora Corne](https://splitkb.com/collections/aurora-corne) with nice!nano v2 controllers.

## Layout

4 layers: QWERTY, NUMBER, SYMBOL, CONTROL (combo NUM+SYM).

Run `kb` to visualize all layers, or `kb <0-3>` for a specific one:

```
$ kb 0
LAYER 0 — QWERTY

┌─────────┬───┬───┬───┬───┬───┐       ┌───┬───┬───┬───┬───┬─────────┐
│ ESC/Alt │ Q │ W │ E │ R │ T │       │ Y │ U │ I │ O │ P │  \ /Alt │
├─────────┼───┼───┼───┼───┼───┤       ├───┼───┼───┼───┼───┼─────────┤
│  CTRL   │ A │ S │ D │ F │ G │       │ H │ J │ K │ L │ ; │  ' /Ctrl│
├─────────┼───┼───┼───┼───┼───┤       ├───┼───┼───┼───┼───┼─────────┤
│ CAPSWORD│ Z │ X │ C │ V │ B │       │ N │ M │ , │ . │ / │  - /Shft│
└─────────┴───┴───┼───┼───┼───┤       ├───┼───┼───┼───┴───┴─────────┘
                  │DG`│ESC│SPC│       │RET│TAB│GUI│
                  │   │NUM│SFT│       │   │SYM│   │
                  └───┴───┴───┘       └───┴───┴───┘
```

## Install helper

```
stow -d <path-to-repo> -t ~ cheat
```

This symlinks `kb` into `~/.local/bin/`.

## Build

Firmware is built via GitHub Actions. Push to trigger a build for both halves (left + right shields).
