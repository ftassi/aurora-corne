# Aurora Corne - ZMK Firmware Config

## Hardware
- Board: splitkb Aurora Corne (split 3x6+3)
- Controller: nice!nano v2
- Firmware: ZMK (Zephyr-based)
- Build: GitHub Actions (see `build.yaml`)
- No encoders, no OLED, no RGB enabled

## Layout: 4 layers

### Layer 0 — QWERTY (default)
- Home row: plain keys (no home row mods)
- Left outer column: Alt/ESC (gqt), Ctrl (plain), caps_word
- Right outer column: Alt/\ (gqt), Ctrl/' (gqt), Shift/- (gqt)
- Thumbs: `&dead_grave` | `&lt NUM ESC` | `&gqt LSHIFT SPACE` || `&kp RET` | `&lt SYM TAB` | `&kp RGUI`
- BS and DEL are NOT on base layer — available on both NUM and SYM layers (top outer columns)

### Layer 1 — NUMBER (activated by left thumb hold)
- Left top/mid: numbers 1-0
- Left bottom (cols 4,5,6): `, . :` for IP/port input (e.g. `192.168.1.1:8080`)
- Right top: arithmetic operators `+ - * / =`
- Right mid: arrow navigation (←↓↑→, mirrors vim HJKL)
- Right bottom: Home/PgDn/PgUp/End (symmetric with arrows above)
- Top outer: BS (left), DEL (right)
- Thumb right inner: `.` for quick decimal/IP entry

### Layer 2 — SYMBOL (activated by right thumb hold)
- Left: brackets/braces paired by row (`{}`, `()`, `[]`)
- Left: special chars `#`, `@`, `$`, `!`, `` ` ``, `~`, `^`, `%`
- Right: operators `+`, `-`, `/`, `*`, `%`, `=`, `&`, `|`, `_`
- Top outer: BS (left), DEL (right) — same positions as NUM layer
- Left thumb inner: `=`

### Layer 3 — CONTROL (activated by holding NUM + SYM together)
- Conditional layer: activates when both NUM and SYM are held
- F1-F10 on top row, F11-F12 on mid-left
- Volume: VOL- top-left, VOL+ top-right (low→high = left→right)
- Mute: mirrored on bottom outer columns (both sides)
- BT: profile selection (BT1-4) + BT_CLR on bottom-right

## Custom behaviors (config/splitkb_aurora_corne.keymap)

### `gqt` — global-quick-tap hold-tap
- Flavor: `tap-preferred`
- Tapping term: 200ms
- Quick-tap: 150ms
- `global-quick-tap` enabled (any key resets tap timer)
- Used for modifier-on-hold / key-on-tap on outer columns

### `dead_grave` — macro for Italian accented vowels
- Sends `RALT+GRAVE` (dead key on US International layout)
- Tap then tap a vowel → à, è, ì, ò, ù
- On left thumb outer position for quick access

## Key design decisions
- Modifiers on outer columns via hold-tap, NOT home row mods
- `global-quick-tap` preferred over standard mod-tap for reducing misfires during fast typing
- Left Ctrl is plain `&kp LCTRL` (no hold-tap) for reliable combos (tmux, vim)
- Right Ctrl keeps hold-tap with `'` (less latency-sensitive)
- GUI only on right thumb (`&kp RGUI`, non-sticky) — essential for i3wm
- Left thumb outer: `dead_grave` macro for Italian accents (replaces former sticky GUI)
- Sticky GUI removed: caused accidental activations more often than intended use
- BS/DEL on top outer columns of BOTH NUM and SYM layers — mirrored, always accessible
- Combo layer (NUM+SYM) enabled by `&trans` on thumb middle positions across layers
- Layer taps on thumbs: NUM on left, SYM on right
- `&caps_word` instead of caps lock
- US International layout for coding; `dead_grave` macro solves accent input pain

## File structure
```
config/
  splitkb_aurora_corne.keymap  — keymap, behaviors, macros, conditional layers
  splitkb_aurora_corne.conf    — hardware feature flags
  west.yml                     — ZMK manifest
build.yaml                     — CI matrix (left + right shields)
```

## Notes
- Keymap uses `&trans` extensively on non-base layers to inherit base layer modifiers
- TODO: evaluate CAPSW as shift on tap / caps_word on hold
