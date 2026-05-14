---
title: "A Systematic Overview of Keyboard Input Methods"
description: "When designing your personal *perfect keyboard experience* it is important to understand the different advantages as well as constraints of non-timed and threshold timed mechanisms. This article also outlines which features you can implement directly in the OS, with QMK or ZMK or a software remapper such as Kanata."
date: 2026-05-10
lastmod: 2026-05-14
cover:
  image: "input-methods.png"  # Relative to static/ or page bundle
  alt: "Keyboard Input Methods"
  caption: ""
  relative: true  # For page bundles

weight: -1
---



# Keyboard Input Methods — And why it matters to be aware of them

This is a first working draft for a coming article. But because I find the tables are potentially already helpful here already an overview of different input methods. 

> [!NOTE]  
>
> If you have comments how to improve the overview or find errors please post on [Reddit KeyboardLayouts](https://www.reddit.com/r/KeyboardLayouts/) (possibly cross-post to [Reddit ErgoMechKeyboards](https://www.reddit.com/r/ErgoMechKeyboards/ as well)). You can also reach me there via my handle *rpnfan* or under the same name via my gmail.com address).

I have divided keyboard input methods into three groups, because those will be relevant when to decide which input mechanism to consider for which type of keyboard activity.

- **Free-timed** — the timing window is controlled by your own physical action; output is always predictable
- **Threshold-timed** — the firmware or OS has a fixed invisible deadline; misfires are possible; you need to match your typing speed to the time-window or vice versa
- **Context-aware / adaptive** — the system watches your typing and modifies behavior automatically

Knowing which category a mechanism falls into immediately tells you what its tradeoffs are: reliability, latency, cognitive load, and learnability all follow directly from the category.

## Overview of Keyboard Input Methods

The following tables establish these three groups of possible keyboard input. The tables will be later extended to show which method is supported by a specific operating system or firmware/ software solution.

### Group 1 — Free-timed mechanisms

Under the first group you find the most basic input methods, beginning with a simple key press. This group is what most operating systems provide and rely on. The advantage of those methods is that they are "non-timed" or possibly better be described as "free-timed". That means there is no timing component a user has to account for. You can type as slow or as fast as you want and you will have the output you expect.

*(User controls the timing window physically; output is always predictable)*

| #    | Mechanism                 | Variants / notes                                             |
| ---- | ------------------------- | ------------------------------------------------------------ |
| 1    | Standard key              | Baseline: one press, one output                              |
| 2    | Held key → modified state | Modifier keys (Shift, Ctrl, Alt, GUI); Momentary layer — same mechanism, different scope |
| 3    | Toggle → persistent state | Layer toggle, Caps Lock                                      |
| 4    | One-shot                  | One-shot modifier, one-shot layer — press-and-release arms state for exactly one key |
| 5    | Sequential composition    | Dead key (1 step), Compose (multi-step), Leader key (arbitrary depth, arbitrary output) — same mechanism at different implementation levels |

### Group 2 — Threshold-timed mechanisms

In this group all input relies on a timing-window. It matters how fast you tap a key or if or how long you hold a key. The advantage is that this enlarges the input options, basically getting much more use out of the limited set of keys you have. The disadvantage is that typing speed matters, your personal typing style needs to be matched to the timing-window and the other way around. That makes the keyboard not universally usable by different people anymore and can also be prone to mis-triggers. Both likely being  a major reason operating systems normally do not support these mechanisms out-of-the-box. 

*(Firmware has a fixed **invisible timing window**; misfires are possible)*

| #    | Mechanism               | Variants / notes                                             |
| ---- | ----------------------- | ------------------------------------------------------------ |
| 6    | Dual-role: tap vs. hold | Mod-tap, Layer-tap; Auto Shift and Space Cadet are example applications |
| 7    | Tap count               | Tap dance                                                    |
| 8    | Tap-toggle              | Tap = toggle, hold = momentary; hybrid of groups 1 and 2     |
| 9    | Simultaneous keys       | Combo                                                        |

### Group 3 — Context-aware / adaptive

This group list methods which look for preceding or following context, typically text, but can also be actions like modifier-combos.

*(Firmware observes ongoing state and modifies behavior automatically)*

| #    | Mechanism                     | Examples                                                 |
| ---- | ----------------------------- | -------------------------------------------------------- |
| 10   | State-watching auto-behaviour | Caps Word, Autocorrect, Repeat Key, Key Lock, Magic Keys |

These three groups list the fundamental mechanisms. In the following tables each mechanism is displayed with subcategories where needed.

## Input Methods in Operating Systems

Unlike the firmware table (see next section), where a tool either supports a mechanism or not, the OS picture has three distinct layers for each mechanism:

1. **Native OS support** — built-in, no installation required
2. **OS-native layout tools** — tools that produce OS-level keyboard layout drivers (MSKLC/KbdEdit on Windows, Ukelele on macOS, XKB on Linux). These operate at the layout/driver level, not the hook level, and persist without a running process.
3. **OS-native remapping infrastructure** — SharpKeys/registry Scancode Map (Windows), Karabiner-Elements (macOS, IOKit-level), keyd/xremap (Linux, evdev-level)

Cross-platform tools (Kanata, AutoHotkey, Espanso...) are explicitly excluded from this table. They can extend all three platforms similarly and will be covered separately.

**Notation:**

- ✓ — native OS support, no additional software
- ◑[tool] — achievable with named OS-level tool
- ✗ — not achievable at this OS layer without cross-platform tools

------

### Group 1 — Free-timed Mechanisms

| #    | Mechanism                      | Windows 11                                  | Linux                                   | macOS                                  |
| ---- | ------------------------------ | ------------------------------------------- | --------------------------------------- | -------------------------------------- |
| 0    | Standard key                   | ✓                                           | ✓                                       | ✓                                      |
| 1    | **Held key → modified state**  |                                             |                                         |                                        |
| 1a   | modifier key                   | ✓                                           | ✓                                       | ✓                                      |
| 1b   | momentary layer (non-modifier) | ✗¹                                          | ◑ XKB levels / keyd²                    | ◑ Karabiner-Elements³                  |
| 2    | **Toggle → persistent state**  |                                             |                                         |                                        |
| 2a   | layer toggle                   | ✗¹                                          | ◑ XKB / keyd²                           | ◑ Karabiner-Elements³                  |
| 2b   | base layer switch              | ◑ Scancode Map [SC]⁴                        | ✓ XKB layout switching⁴                 | ✓ Input Source switching⁴              |
| 2c   | key toggle (Caps Lock style)   | ✓ Caps Lock native; others ◑ MSKLC/KbdEdit⁵ | ✓ XKB; configurable via setxkbmap⁵      | ✓ Caps Lock native; others ◑ Ukelele⁵  |
| 3    | **One-shot**                   |                                             |                                         |                                        |
| 3a   | one-shot modifier              | ✓ Sticky Keys [native accessibility]⁶       | ✓ XKB `stickykeys`; ◑ keyd `oneshot()`⁶ | ✓ Sticky Keys [native accessibility]⁶  |
| 3b   | one-shot layer                 | ✗                                           | ◑ keyd `oneshot()`                      | ◑ Karabiner-Elements³                  |
| 4    | **Sequential composition**     |                                             |                                         |                                        |
| 4a   | dead key                       | ◑ MSKLC / KbdEdit⁷                          | ✓ XKB (extensive native support)⁷       | ✓ native + ◑ Ukelele for custom⁷       |
| 4b   | compose key                    | ✗ natively; ◑ WinCompose [LLH]⁸             | ✓ XKB `compose:` option⁸                | ✗ natively; ◑ Karabiner + KeyBindings⁸ |
| 4c   | leader / key sequence          | ✗                                           | ◑ keyd macros (limited)⁹                | ✗⁹                                     |

------

### Group 2 — Threshold-timed Mechanisms

| #    | Mechanism                     | Windows 11                                      | Linux                         | macOS                                                   |
| ---- | ----------------------------- | ----------------------------------------------- | ----------------------------- | ------------------------------------------------------- |
| 5    | **Dual-role: tap vs. hold**   |                                                 |                               |                                                         |
| 5a   | mod-tap                       | ✗ natively; ◑ keyd [LLH not available on Win]¹⁰ | ◑ keyd `overload()` [evdev]¹⁰ | ◑ Karabiner `to_if_alone` / `to_if_held_down` [IOKit]¹⁰ |
| 5b   | layer-tap                     | ✗                                               | ◑ keyd [evdev]                | ◑ Karabiner-Elements³                                   |
| 5c   | hold-tap flavor variants      | ✗                                               | ◑ keyd / xremap [evdev]       | ◑ Karabiner (partial)¹¹                                 |
| 5d   | Auto Shift                    | ✗                                               | ✗                             | ✗                                                       |
| 6    | **Tap count**                 |                                                 |                               |                                                         |
| 6a   | tap dance                     | ✗                                               | ◑ keyd (limited)¹²            | ◑ Karabiner (limited)¹²                                 |
| 6b   | tap dance eager               | ✗                                               | ✗¹²                           | ✗¹²                                                     |
| 7    | **Tap-toggle** *(hybrid)*     | ✗                                               | ◑ keyd approximation          | ◑ Karabiner approximation                               |
| 8    | **Simultaneous keys (combo)** | ✗¹³                                             | ◑ keyd [evdev]¹³              | ◑ Karabiner `simultaneous`¹³                            |

------

### Group 3 — Context-aware / Adaptive

| #    | Mechanism                                | Windows 11                                  | Linux             | macOS                             |
| ---- | ---------------------------------------- | ------------------------------------------- | ----------------- | --------------------------------- |
| 9    | **State-watching auto-behavior**         |                                             |                   |                                   |
| 9a   | word-boundary capitalization (Caps Word) | ✗                                           | ✗¹⁴               | ✗¹⁴                               |
| 9b   | last-key repeat                          | ✓ OS key repeat (hold-to-repeat)¹⁵          | ✓ OS key repeat¹⁵ | ✓ OS key repeat¹⁵                 |
| 9c   | stream autocorrect                       | ◑ Windows text replacement (limited apps)¹⁶ | ✗ natively¹⁶      | ✓ Cocoa system-wide autocorrect¹⁶ |
| 9d   | layer lock                               | ✗                                           | ◑ keyd            | ◑ Karabiner (partial)             |

------

## Footnotes

**¹ Windows layers and non-modifier held states:** The Windows keyboard driver and layout system (managed via MSKLC or KbdEdit) operates only at the character-output level — it defines what character a key+modifier-state combination produces. It has no concept of user-defined layers that remap the full keymap. The Scancode Map registry remaps scancodes 1:1 at boot, with no runtime state. Neither mechanism can produce a QMK-style momentary or toggle layer. This is the most significant capability gap in the Windows native stack. It cannot be closed without a running hook process or driver.

**² Linux XKB levels vs. layers:** XKB supports up to eight "shift levels" per key, accessible via modifier combinations (Shift, AltGr, and additional custom modifiers). This is the closest native Linux equivalent to a momentary layer — holding a defined modifier shifts all keys to a different output level. However XKB levels are character-output mappings, not full keymap remappings; navigation actions, media keys, and arbitrary firmware-style behaviour cannot be expressed in XKB alone. keyd, operating at the evdev level below the display server, provides genuine layer switching with full action support and persists across X11 and Wayland sessions.

**³ Karabiner-Elements on macOS:** Karabiner operates at the IOKit HID driver level, giving it system-wide access equivalent to Linux evdev — not subject to the application-bypass limitations of Windows LLHook. It supports layers (via `set_variable` conditions), one-shot keys, layer-tap, combos, and most Group 2 mechanisms through its complex modifications JSON format. It requires installation of a kernel extension (or system extension on recent macOS), and Apple occasionally breaks compatibility at major OS version boundaries, requiring a Karabiner update before functionality is restored.

**⁴ Base layer switching:** All three OSes support switching between predefined keyboard layouts — this is the standard multilingual input mechanism, not a power-user feature. On Windows this is the Input Language switching (Win+Space); on Linux it is XKB layout groups with a configured toggle; on macOS it is Input Source switching. This is fundamentally different from firmware-style layer switching because layouts must be predefined and installed system-wide, not configured per-user on the fly.

**⁵ Caps Lock style toggle keys:** All three OSes treat Caps Lock as a special toggleable key natively. MSKLC and KbdEdit on Windows can define additional keys that behave like Caps Lock in a custom layout DLL — specifically the KANA key modifier and similar toggleable states are available. Ukelele on macOS cannot reassign modifier keys at all; it handles only character output assignments. XKB on Linux allows arbitrary keys to be mapped to modifier locking behaviour via `lock` type assignments.

**⁶ Sticky Keys / one-shot modifier:** The native Sticky Keys feature on Windows and macOS is an accessibility accommodation applied globally to all modifier keys. It cannot be configured to apply to only one specific modifier or to a non-standard key. It also has ergonomically awkward default behaviour (pressing a modifier twice locks it until pressed again, with an audible click). keyd on Linux provides a more precise `oneshot()` implementation that can be applied per-key without affecting other modifiers.

**⁷ Dead keys:** Linux has the most accessible dead key implementation — many standard layouts include dead keys out of the box and custom dead keys are straightforward to define in XKB symbols files. MSKLC on Windows supports dead keys but has known issues with chained dead keys (two-step compositions), with some Unicode ranges causing double-output bugs. KbdEdit resolves most of these MSKLC limitations and supports nested/chained dead keys more reliably, but is paid software. macOS via Ukelele supports dead keys in `.keylayout` XML files; the editor works well for single-step dead keys, though multi-step chains must be hand-edited in the XML, as Ukelele's GUI does not expose sequences longer than two steps.

**⁸ Compose key — OS support:** Linux/XKB is the only platform with native, first-class compose key support. The compose key is assigned via `setxkbmap -option compose:ralt` (or similar), and sequences are defined in `~/.XCompose` extending the system compose table. Windows has no native compose key; WinCompose is the standard third-party solution but operates via LLHook (see the stack discussion above), meaning it does not work in elevated processes or raw-input applications. macOS has no native compose key; a partial approximation is possible via `~/Library/KeyBindings/DefaultKeyBinding.dict`, but this mechanism only works in Cocoa/AppKit text fields — it does not function in non-Cocoa applications, web browsers' text fields, or terminal emulators.

**⁹ Leader key / sequences at OS level:** No OS provides a native leader key mechanism. keyd on Linux can approximate short sequences using its macro chaining, but it does not have a formal leader-key concept with arbitrary sequence depth. On Windows and macOS, a leader key is not achievable at the OS-native tool level without a cross-platform daemon (Kanata).

**¹⁰ Mod-tap — interception depth matters:** keyd on Linux achieves mod-tap via its `overload(modifier, key)` syntax at the evdev level, meaning it works system-wide, in Wayland, at the virtual console, and is not subject to display-server bypass. Karabiner-Elements on macOS achieves tap-vs-hold via `to_if_alone` / `to_if_held_down` at the IOKit level with equivalent system-wide reach. On Windows, mod-tap is not achievable at the OS-native tool layer. MSKLC and KbdEdit produce static layout DLLs with no timing logic. SharpKeys/Scancode Map is 1:1 static remapping only. The only Windows OS-level tools available operate at LLHook level (see previous discussion), and keyd does not have a Windows version.

**¹¹ Hold-tap flavour variants on macOS:** Karabiner-Elements supports `to_if_alone`, `to_if_held_down`, and `to_after_key_up` conditions, which correspond roughly to tap-preferred and hold-preferred disambiguation flavours. It does not expose the full range of QMK-style flavour options (permissive hold, retro tap, etc.), but covers the most common cases.

**¹² Tap dance at OS level:** keyd on Linux supports basic multi-tap routing through its `oneshot()` and `macro()` composability, but does not have a formal tap-dance primitive equivalent. Double-tap detection for a single key producing a different output is achievable but the implementation is constrained. Karabiner-Elements can detect `to_if_alone` combinations that approximate single-vs-double-tap distinction but full N-tap routing (3+) is complex and not natively supported. Tap dance eager (which fires output on each tap without waiting for sequence completion) is not available in either tool at OS level.

**¹³ Simultaneous key combos:** keyd supports simultaneous key detection natively via its chord notation. Karabiner-Elements has first-class `simultaneous` key support with a configurable timing window — this is one of Karabiner's strongest features relative to other OS-level tools. Windows has no native simultaneous-key mechanism beyond standard modifier+key shortcuts; the Scancode Map and MSKLC/KbdEdit operate on individual keys only.

**¹⁴ Caps Word at OS level:** Caps Word requires the input processor to observe the stream of characters being typed and deactivate capitalisation mode on a word-boundary character. This kind of stateful stream watching is not available in any OS-native keyboard layout tool. XKB, MSKLC, KbdEdit, and Ukelele all define static per-key mappings with no awareness of typing context. It is currently a firmware-only feature or requires a running cross-platform daemon.

**¹⁵ OS key repeat vs. firmware repeat key:** The OS hold-to-repeat mechanism is a standard feature on all platforms and is configurable (repeat delay and rate) in system settings. This is distinct from the firmware `QK_REPEAT_KEY` mechanism, which replays the last keypress on demand with a single tap of a dedicated key, without holding. No OS-native tool provides the latter behaviour.

**¹⁶ Autocorrect / text replacement:** macOS has the most complete native implementation — the Cocoa text system provides system-wide autocorrect and text replacement that works in all native AppKit/Cocoa applications. Web browsers and Electron applications are partially covered depending on whether they use the native text input APIs. Windows 11 has autocorrect in the touch keyboard and in some Microsoft Office and UWP applications, but it does not function in standard Win32 desktop applications. Linux has no native system-wide autocorrect facility.

------

#### Notes on OS Differences and Cross-Platform Tools

The tables above deliberately exclude cross-platform daemon tools such as Kanata and AutoHotkey, focusing instead on what each OS provides natively or through OS-level layout tools. This distinction matters because the interception depth of a tool determines its reliability and scope.

On Linux, tools like keyd and xremap operate at the evdev level, below the display server, giving them system-wide reach across X11, Wayland, and virtual terminals without the bypass limitations found elsewhere. XKB adds further native capability at the layout level. The combination makes Linux the most capable OS for software-level keyboard mechanism implementation.

On macOS, Karabiner-Elements operates at the IOKit HID driver level, equivalent in depth to Linux evdev. It works system-wide in virtually all applications and is not subject to application-level bypass. This makes macOS second in capability, though Karabiner depends on a kernel or system extension that Apple can break at major OS version boundaries.

On Windows, the native tool gap is largest. MSKLC and KbdEdit produce static layout DLLs with no timing logic; SharpKeys and the Scancode Map registry handle only 1:1 static remapping. Most advanced mechanisms therefore require a cross-platform daemon such as Kanata, which does run on Windows and covers much of the mechanism space, but it operates at LLHook level. As discussed in another article, LLHook does not intercept input directed at elevated processes, does not function at the login screen or UAC prompts, and is bypassed by applications using raw input APIs. This is a capability gap that Microsoft could close by implementing these mechanisms natively at the appropriate level of the input stack. 

## Input Methods in QMK, ZMK and Kanata

The tables compare which methods are available with different firmware or software methods. The list is not complete but covers the two main firmware options ZMK and QMK, where for QMK the list also shows the configuration options with a Via or Vial enabled keyboard, allowing a GUI-based configurations. Via and Vial are QMK-only, they do not apply to ZMK at all. ZMK has its own web configurator (ZMK Studio, formerly Keymap Editor) which is much more limited than Vial. Finally the overview covers Kanata which is relative new, but quickly becoming *the* software solution for keyboard remapping.

### Group 1 — Free-timed Mechanisms

| #    | Mechanism                     | QMK syntax                | Via¹ | Vial¹ | Kanata syntax                                | ZMK syntax               |
| :--- | :---------------------------- | :------------------------ | :--- | :---- | :------------------------------------------- | :----------------------- |
| 0    | Standard key                  | `KC_X`                    | ✓    | ✓     | key literal (`a`, `spc`)                     | `&kp X`                  |
| 1    | **Held key → modified state** |                           |      |       |                                              |                          |
| 1a   | modifier key                  | `KC_LSFT`, `KC_LCTL` …    | ✓    | ✓     | `lsft`, `lctl` …                             | `&kp LSFT`, `&kp LCTL` … |
| 1b   | momentary layer               | `MO(n)`                   | ✓    | ✓     | `(layer-while-held name)`                    | `&mo n`                  |
| 2    | **Toggle → persistent state** |                           |      |       |                                              |                          |
| 2a   | layer toggle                  | `TG(n)`                   | ✓    | ✓     | `(layer-toggle name)`                        | `&tog n`                 |
| 2b   | base layer switch             | `TO(n)`                   | ✓    | ✓     | `(layer-switch name)`                        | `&to n`                  |
| 2c   | key toggle (lock key down)    | `QK_LOCK`                 | –    | –     | *(not supported)*                            | `&kt X`                  |
| 3    | **One-shot**                  |                           |      |       |                                              |                          |
| 3a   | one-shot modifier             | `OSM(MOD_LSFT)`           | ✓    | ✓     | `(one-shot timeout lsft)`                    | `&sk LSFT`               |
| 3b   | one-shot layer                | `OSL(n)`                  | ✓    | ✓     | `(one-shot timeout (layer-while-held name))` | `&sl n`                  |
| 4    | **Sequential composition**    |                           |      |       |                                              |                          |
| 4a   | dead key                      | OS/layout level²          | n/a  | n/a   | OS/layout level²                             | OS/layout level²         |
| 4b   | compose key                   | OS/layout level²          | n/a  | n/a   | OS/layout level²                             | OS/layout level²         |
| 4c   | leader / key sequence         | `LEADER` + callbacks in C | –    | –     | `defseq` + `(sequence-timeout ms)`           | `zmk-helpers` module³    |

------

### Group 2 — Threshold-timed Mechanisms

| #    | Mechanism                                           | QMK syntax                                                   | Via¹ | Vial¹ | Kanata syntax                                                | ZMK syntax                            |
| :--- | :-------------------------------------------------- | :----------------------------------------------------------- | :--- | :---- | :----------------------------------------------------------- | :------------------------------------ |
| 5    | **Dual-role: tap vs. hold**                         |                                                              |      |       |                                                              |                                       |
| 5a   | mod-tap                                             | `MT(MOD_X, KC_Y)` / `LSFT_T(KC_X)`                           | ✓    | ✓     | `(tap-hold t t key mod)`                                     | `&mt MOD KEY`                         |
| 5b   | layer-tap                                           | `LT(n, KC_X)`                                                | ✓    | ✓     | `(tap-hold t t key (layer-while-held name))`                 | `&lt n KEY`                           |
| 5c   | custom hold-tap                                     | `HOLD_ON_OTHER_KEY_PRESS`, `PERMISSIVE_HOLD`, `RETRO_TAP` (per-key or global) | –    | ✓⁴    | `tap-hold-press`, `tap-hold-release`, `tap-hold-release-timeout` | `&hold-tap` with `flavor` property⁵   |
| 5d   | Auto Shift                                          |                                                              |      |       |                                                              |                                       |
| 6    | **Tap count**                                       |                                                              |      |       |                                                              |                                       |
| 6a   | tap dance                                           | `TD(n)` + `tap_dance_actions[]`                              | –    | ✓⁶    | `(tap-dance timeout (a1 a2 …))`                              | `zmk,behavior-tap-dance`⁷             |
| 6b   | tap dance eager                                     | *(custom)*                                                   | –    | –     | `(tap-dance-eager timeout (a1 a2 …))`                        | *(not supported)*                     |
| 7    | **Tap-toggle** *(hybrid: hold=free, tap=threshold)* | `TT(n)`                                                      | ✓    | ✓     | *(approximate via tap-dance + layer-while-held)*⁸            | *(approximate via tap-dance + &mo)*⁸  |
| 8    | **Simultaneous keys (combo)**                       | `COMBO(name, output)`                                        | –    | ✓     | `(defchords group timeout (keys) action)`                    | `combos { … key-positions = <n n>; }` |

------

### Group 3 — Context-aware / Adaptive

| #    | Mechanism                        | QMK syntax                                          | Via¹ | Vial¹ | Kanata syntax              | ZMK syntax                 |
| :--- | :------------------------------- | :-------------------------------------------------- | :--- | :---- | :------------------------- | :------------------------- |
| 9    | **State-watching auto-behavior** |                                                     |      |       |                            |                            |
| 9a   | word-boundary capitalization     | `CAPS_WORD`                                         | –    | ✓     | `(caps-word timeout)`      | `&caps_word`               |
| 9b   | last-key repeat                  | `QK_REPEAT_KEY`                                     | –    | ✓⁹    | `rpt`                      | `&key_repeat`              |
| 9c   | hold-duration auto-shift         | `AUTO_SHIFT_ENABLE` in [rules.mk](http://rules.mk/) | –    | ✓¹⁰   | *(not supported)*          | *(not supported)*          |
| 9d   | in-firmware autocorrect          | `AUTOCORRECT_ENABLE` + data file                    | –    | –     | *(not supported)*          | *(not supported)*          |
| 9e   | layer lock                       | `QK_LAYER_LOCK`                                     | –    | ✓     | *(not supported natively)* | *(not supported natively)* |

------

### Footnotes

**¹ Via / Vial are QMK configuration tools, not separate firmware.** They allow runtime configuration without recompiling. Via is the base tool; Vial is an extended fork with additional feature support. Whether a Vial feature is available on a specific keyboard depends on it being compiled into that keyboard’s firmware — keyboard manufacturers must enable features at compile time. A ✓ means the feature can be configured in the tool when the firmware supports it; a – means it requires direct firmware compilation regardless.

**² Dead keys and compose keys** are defined at the OS keyboard layout level (XKB on Linux, the layout .dll on Windows, or the system keyboard layout on macOS). Neither QMK, Kanata, nor ZMK implement dead key or compose key composition — they pass the relevant keycodes through to the OS, which resolves the composition. The keycode can be assigned to any physical key, but the composition logic lives entirely in the OS.

**³ ZMK leader key** is not part of the core firmware. It is available via the `zmk-helpers` community module (`ZMK_LEADER_SEQUENCE`). This requires adding the module to your ZMK config — it is not available in the standard ZMK web configurator (ZMK Studio).

**⁴ Vial QMK settings** expose global tap-hold behavior modifiers (tapping term, permissive hold, hold-on-other-key-press, retro tap, quick-tap term) as runtime-configurable parameters. Per-key hold behaviour variants still require firmware compilation in QMK.

**⁵ ZMK hold-tap flavors** are `hold-preferred`, `balanced`, `tap-preferred`, and `tap-unless-interrupted` — these correspond roughly to QMK’s hold-on-other-key-press, permissive-hold, tap-preferred, and retro-tap behaviours respectively, though the mapping is not exact.

**⁶ Vial tap-dance** supports four actions per entry (on tap, on hold, on double tap, on tap+hold). Full QMK tap-dance with arbitrary tap counts and custom callbacks still requires firmware compilation.

**⁷ ZMK tap-dance** requires defining a named behavior in the devicetree. It is not directly configurable via ZMK Studio (the web configurator) at present — it requires editing the keymap file.

**⁸ Tap-toggle (TT)** has no direct equivalent in Kanata or ZMK. The behaviour can be approximated by combining a tap-dance (single tap → toggle, timeout → momentary) with the respective layer mechanisms, but the feel is not identical to QMK’s TT().

**⁹ Alt-repeat key** (`QK_ALT_REPEAT_KEY`) is also supported in recent Vial versions — this is a context-aware variant of repeat that produces the “opposite” of the last key (e.g. after typing `(` it outputs `)`). ZMK does not have a native equivalent; QMK’s basic `QK_REPEAT_KEY` maps to both Kanata’s `rpt` and ZMK’s `&key_repeat`.

**¹⁰ Vial auto-shift** exposes the Auto Shift feature as a runtime-togglable setting with configurable timeout, but only when the keyboard firmware has been compiled with `AUTO_SHIFT_ENABLE`. It cannot be enabled purely via Vial if the firmware does not include it.

