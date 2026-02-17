# Aurora Corne - ZMK Firmware Config

## Hardware
- Board: splitkb Aurora Corne (split 3x6+3)
- Controller: nice!nano v2
- Firmware: ZMK (Zephyr-based, Zephyr 4.1 / HWMv2)
- Build: GitHub Actions (see `build.yaml`), board identifier: `nice_nano//zmk`
- No encoders, no OLED, no RGB enabled

## Layout: 4 layers

### Layer 0 — QWERTY (default)
- Home row: plain keys (no home row mods)
- Left outer column: Alt/ESC (gqt), BS/Ctrl (hpt), caps_word
- Right outer column: Alt/\ (gqt), Ctrl/' (gqt), Shift/- (gqt)
- Thumbs: `&dead_grave` | `&lt NUM ESC` | `&gqt LSHIFT SPACE` || `&kp RET` | `&lt SYM TAB` | `&sk RGUI`
- BS on base layer as tap of left Ctrl (hpt); DEL only on NUM/SYM layers (top-right outer)
- Combo: Ctrl+Space (positions 12+38, 50ms) sends `LC(SPACE)` for tmux prefix

### Layer 1 — NUMBER (activated by left thumb hold)
- Left top/mid: numbers 1-0
- Right top: arithmetic operators `+ - * / =`
- Right mid: arrow navigation (←↓↑→, mirrors vim HJKL)
- Right bottom: Home/PgDn/PgUp/End (symmetric with arrows above)
- Top outer: DEL (right only, left inherits Alt/ESC via &trans)
- Right thumbs: `.` | `:` (lt SYM, for CTL layer access) | `,` — optimized for IP/port entry

### Layer 2 — SYMBOL (activated by right thumb hold)
- Left: brackets/braces paired by row (`{}`, `()`, `[]`)
- Left: special chars `#`, `@`, `$`, `!`, `` ` ``, `~`, `^`, `%`
- Right: operators `+`, `-`, `/`, `*`, `%`, `=`, `&`, `|`, `_`
- Top outer: DEL (right only, left inherits Alt/ESC via &trans)
- Left thumb center: `:` (lt NUM, for CTL layer access)
- Left thumb right: `=`

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

### `hpt` — hold-preferred-tap hold-tap
- Flavor: `hold-preferred`
- Tapping term: 200ms
- Quick-tap: 150ms
- Used for left Ctrl (hold=LCTRL, tap=BSPC, double-tap-hold=BSPC repeat)

### `dead_grave` — macro for Italian accented vowels
- Sends `RALT+GRAVE` (dead key on US International layout)
- Tap then tap a vowel → à, è, ì, ò, ù
- On left thumb outer position for quick access

### Combo: `combo_tmux`
- Keys: positions 12 (BS/CTRL) + 38 (SPC/SFT)
- Timeout: 50ms (chord-style)
- Output: `Ctrl+Space` — tmux prefix
- Takes priority over hold-tap on both keys

## Key design decisions
- Modifiers on outer columns via hold-tap, NOT home row mods
- `global-quick-tap` preferred over standard mod-tap for reducing misfires during fast typing
- Left Ctrl is BS/Ctrl via `hpt` (hold-preferred): tap=BS, hold=Ctrl, double-tap-hold=BS repeat
- Ctrl+Space combo (50ms chord) for tmux prefix — no hold-tap latency
- Right Ctrl keeps hold-tap with `'` (less latency-sensitive)
- GUI on right thumb as sticky key (`&sk RGUI`) — essential for i3wm combos (GUI+Enter, GUI+Shift+N)
- Left thumb outer: `dead_grave` macro for Italian accents
- DEL on top-right outer column of NUM and SYM layers
- Combo layer (NUM+SYM) enabled by `&lt` with layer-tap on thumb positions:
  - SYM layer thumb center: `&lt NUM COLON` (tap=`:`, hold=NUM→CTL)
  - NUM layer thumb center: `&lt SYM COLON` (tap=`:`, hold=SYM→CTL)
- Layer taps on thumbs: NUM on left, SYM on right
- `&caps_word` instead of caps lock
- US International layout for coding; `dead_grave` macro solves accent input pain
- `, . :` on NUM right thumb cluster for ergonomic numeric/IP entry

## File structure
```
config/
  splitkb_aurora_corne.keymap  — keymap, behaviors, macros, combos, conditional layers
  splitkb_aurora_corne.conf    — hardware feature flags
  west.yml                     — ZMK manifest
build.yaml                     — CI matrix (left + right shields + settings_reset)
```

## Notes
- Keymap uses `&trans` extensively on non-base layers to inherit base layer modifiers
- Board identifier is `nice_nano//zmk` (HWMv2 format, v2 is default revision)
- settings_reset shield included in build for BLE bond clearing after firmware upgrades
