# Aurora Corne - ZMK Firmware Config

## Hardware
- Board: splitkb Aurora Corne (split 3x6+3)
- Controller: nice!nano v2
- Firmware: ZMK (Zephyr-based)
- Build: GitHub Actions (see `build.yaml`)
- No encoders, no OLED, no RGB enabled

## Layout: 3 layers

### Layer 0 — QWERTY (default)
- Home row: plain keys (no home row mods)
- Corner keys use `gqt` (global-quick-tap hold-tap):
  - Top-left: Alt/ESC, Top-right: Alt/\
  - Mid-left: Ctrl/Backspace, Mid-right: Ctrl/'
  - Bottom-right: Shift/-
- Bottom-left: `&caps_word`
- Thumbs: `&dead_grave` | `&lt NUM ESC` | `&gqt LSHIFT SPACE` || `&kp RET` | `&lt SYM TAB` | `&kp RGUI`

### Layer 1 — NUMBER (activated by left thumb hold)
- Left: numbers 1-0
- Right: navigation arrows, Home/End/PgUp/PgDn
- Right outer column: volume up/down/mute
- BT profile selection + BT_CLR on right top row
- Thumb right: dot and comma

### Layer 2 — SYMBOL (activated by right thumb hold)
- Brackets/braces on left (paired: `{}`, `()`, `[]`)
- Operators on right: `+`, `-`, `/`, `*`, `%`, `=`, `&`, `|`, `_`
- Special chars: `#`, `@`, `$`, `!`, `` ` ``, `~`, `^`
- Left thumb: `:` and `=`

## Custom behaviors (config/splitkb_aurora_corne.keymap)

### `gqt` — global-quick-tap hold-tap
- Flavor: `tap-preferred`
- Tapping term: 200ms
- Quick-tap: 150ms
- `global-quick-tap` enabled (any key resets tap timer)
- Used for modifier-on-hold / key-on-tap on outer columns

### `dead_grave` — macro for Italian accented vowels
- Sends `RALT+GRAVE` (dead key on US International layout)
- Tap `dead_grave` then tap a vowel → à, è, ì, ò, ù
- On left thumb outer position for quick access

## Key design decisions
- Modifiers on outer columns via hold-tap, NOT home row mods
- `global-quick-tap` preferred over standard mod-tap for reducing misfires during fast typing
- GUI only on right thumb (`&kp RGUI`, non-sticky) — essential for i3wm
- Left thumb outer: `dead_grave` macro for Italian accents (replaces former sticky GUI)
- Sticky GUI removed: caused accidental activations more often than intended use
- Layer taps on thumbs: NUM on left, SYM on right
- `&caps_word` instead of caps lock
- US International layout for coding; `dead_grave` macro solves accent input pain

## File structure
```
config/
  splitkb_aurora_corne.keymap  — keymap and behaviors
  splitkb_aurora_corne.conf    — hardware feature flags
  west.yml                     — ZMK manifest
build.yaml                     — CI matrix (left + right shields)
```

## Notes
- Keymap uses `&trans` extensively on non-base layers to inherit base layer modifiers
- There are TODO comments about adding CAPSW hold behavior and left shift on NUM layer
