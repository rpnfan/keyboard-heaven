---
title: "Advanced Windows Keyboard Input: Special Characters, Dead Keys, and Software Remapping"
description: "This article explores the practical methods for accessing special characters, the limitations and quirks of dead keys, why Input Method Editors aren't suitable for Western keyboards, and how software remapping layers intercept and transform input at different levels."
date: 2026-01-10
cover:
  image: "keyboard-chain-advanced.jpg"  # Relative to static/ or page bundle
  alt: "Windows Keyboard Chain - Advanced"
  caption: ""
  relative: true  # For page bundles
---


## Introduction

The [first article](/deep-dive/windows-keyboard-chain) explained how Windows transforms keyboard hardware into input through the HID → Scancode → VK → Layout chain. But that explanation left important questions: How do you actually access special characters like é, ñ, or €? Why do some keyboard remapping tools cause dead keys to stop working? How do software remapping layers interact with the Windows keyboard system?

This article explores the practical methods for accessing special characters, the limitations and quirks of dead keys, why Input Method Editors aren't suitable for Western keyboards, and how software remapping layers intercept and transform input at different levels.


## Part 1: Methods for Accessing Special Characters

Windows keyboard layouts support the **full Unicode character set** — over 140,000 characters. The question isn't whether you can type them, but *how*. There are two native methods built into Windows keyboard layouts.

### Method 1: AltGr Layer (Third-Level Characters)

**What it is:** An additional character layer accessed by pressing AltGr (Right Alt) or Ctrl+Alt.

The Windows keyboard layout DLL can define up to four character levels per key:
- Base level (no modifiers)
- Shift level
- AltGr level (third level)
- AltGr+Shift level (fourth level)

```
Example: German keyboard
Key Q (physical position):
  Q → 'q'
  Shift+Q → 'Q'
  AltGr+Q → '@'
  AltGr+Shift+Q → '¡'
```

**Pros:**
- Native OS-level support
- Fast and immediate (characters appear instantly)
- Works everywhere Windows works
- Each key can hold up to 4 character variants

**Cons:**
- Limited to 4 characters per key
- **Windows treats Ctrl+Alt as AltGr on non-US layouts**, creating conflicts[1]
- **Microsoft advised developers since 2004 not to use Ctrl+Alt shortcuts** due to this collision[1]
- Yet many modern applications (Firefox, OneNote, Visual Studio) still use Ctrl+Alt shortcuts, breaking AltGr for international users[1]
- Stateless — no composition possible

**Which layouts use AltGr heavily:**
European layouts (French, German, Italian, Spanish) use AltGr extensively for accents and symbols. US layout typically doesn't use it. Dvorak and Colemak define AltGr layers for special characters.



### Method 2: Dead Keys (Composition via Stateful Input)

**What it is:** A key that waits for the next keystroke, combining them to produce accented characters.

**How it works:**

```
User presses: Dead acute (´)
  → Windows stores state in kernel buffer
  → No output yet
  
User presses: 'a'
  → Layout checks: "´ + a" = á
  → Output: 'á' (one composed character)
```

Dead keys enable true **composition** — two keystrokes produce one character. This is the only native Windows keyboard feature with stateful, cross-keystroke behavior.

**Technical detail:** When a dead key is pressed, `ToUnicode` returns a negative value, signaling "waiting for composition." The kernel maintains this state until the next keystroke.

**Pros:**
- True composition (two keystrokes → one character)
- Can chain dead keys (´ + ` + a → á with grave combining)
- Works reliably in all Windows versions
- Works in all applications without special support

**Cons:**
- **Cannot be canceled**: Once activated, the dead key state persists in the kernel buffer until you press another key. There is **no escape or undo**[8]
  - Pressing Space outputs the dead key character itself (´ + space = two characters: ´ and space)
  - Pressing any other key attempts composition or outputs the dead key separately
- Keyboard hooks and software remapping tools can **destroy dead key state** by calling `ToUnicode`[2]
- Difficult to use Shift with dead keys — Windows doesn't support "one-shot Shift"[11]
- Confusing for English speakers unfamiliar with diacritics

**Which layouts use dead keys:**
European layouts (French, Portuguese, Spanish) for accents: acute (´), grave (`), circumflex (^), diaeresis (¨), tilde (~). US International layout (optional). Nordic and Eastern European layouts. Dvorak International.



### Comparison: AltGr vs. Dead Keys

| Feature | AltGr | Dead Keys |
|---------|-------|-----------|
| **Native to Windows** | Yes | Yes |
| **Speed** | Instant (one key combo) | Medium (two keys) |
| **Characters per key** | Up to 4 | Unlimited via sequences |
| **Can cancel** | N/A (stateless) | No |
| **Destroyable by hooks** | No | Yes[2] |
| **Works with Shift** | Yes | Difficult |
| **Composition** | No | Yes |

**When to use:** AltGr for quick access to a few variants per key; dead keys for diacritic composition.



## Part 2: Why Dead Keys Can Be Problematic

Dead keys are powerful but fragile. Understanding why they fail is crucial for avoiding frustration.

### Hook-Based Remapping Destroys Dead Key State

Software remapping tools using keyboard hooks (WH_KEYBOARD_LL) often call `ToUnicode` to examine the current keystroke.

**The problem:** `ToUnicode` is destructive. It modifies the kernel keyboard buffer, **consuming any active dead key state**.[2]

```
Scenario:
1. User presses dead key (´)
   → Kernel buffer stores state
   
2. Remapping tool's hook executes
   → Calls ToUnicode to examine the key
   → This consumes the dead key state
   
3. User presses 'a'
   → No dead key state left
   → Just outputs 'a' (not 'á')
```

**Which tools have this problem:**
- Some AutoHotkey hotstring implementations[9]
- Keyboard remapping tools that examine input via `ToUnicode`
- Screen readers and accessibility tools that process keyboard input
- Some IME systems[10]

**How to avoid it:**
- Use firmware-level remapping (QMK, ZMK) instead of hooks
- If using hooks, don't call `ToUnicode`; just modify VK codes instead
- Use kernel driver or hardware-level interception instead of LLHOOK



### AltGr vs. Ctrl+Alt Conflict

On non-US Windows keyboard layouts, **Windows internally treats Ctrl+Alt as equivalent to AltGr**.

**Why this breaks applications:**

```
User has French AZERTY layout (Right Alt = AltGr)
Application shortcut: Ctrl+Alt+R (reload)

User presses: Ctrl+Alt+R
  → Windows converts to AltGr+R
  → Layout interprets as: "access AltGr layer for R"
  → User sees character instead of reload

Result: Shortcut doesn't work
```

**The impact:**
- Firefox, OneNote, Visual Studio, and many others use Ctrl+Alt shortcuts
- These break for international users with AltGr-based layouts
- Microsoft warned developers in 2004 not to use Ctrl+Alt, but compliance is poor[1]

**No perfect workaround** for users — only applications can fix this by avoiding Ctrl+Alt shortcuts.



## Part 3: Input Method Editors (IME) — Not for Western Keyboards

Input Method Editors are designed for languages with large character sets (Japanese, Chinese, Korean). They are **not suitable for Western keyboard remapping** despite sometimes appearing as an option.

### What IME Does

IME converts sequences of keystrokes into complex characters through composition:

```
User types romaji: "nihongo"
  → IME suggests candidates:
     - にほんご (hiragana)
     - 日本語 (kanji, most common)
  → User selects
  → Output: 日本語 (one word from seven keystrokes)
```

IME also provides mode switching (Hiragana, Katakana, Direct passthrough, Full-width), selectable via Alt+` or toolbar.

### Why NOT to Use IME for Western Layers

**1. Application-Specific Only**

IME works **only in text input fields** (TextBox, browsers, Notepad, Word). It has **zero effect** on:
- System menus, dialogs, file explorers
- Games
- Command-line interfaces
- Hotkeys and shortcuts
- Most system-level operations

If you need layers for navigation or function keys, IME doesn't help.[4][7]

**2. Per-Application Mode State**

Windows remembers your IME mode **per application**. Switch between Notepad (Hiragana mode) and Firefox (Alphanumeric mode), and Firefox reverts to whatever mode you last used there. **You cannot rely on consistent mode across applications.**[5]

**3. Non-Japanese Keyboard Compatibility**

Using a non-Japanese keyboard with Japanese IME requires manual registry editing (undocumented, unsupported by Microsoft, prone to breaking).[3][6]

**4. Limited to Text Composition**

IME cannot remap non-text keys (arrows, function keys, modifiers). It's strictly for character composition in supported languages.

### When IME IS Appropriate

Use IME **exclusively for typing in languages that require phonetic-to-character conversion: Japanese, Chinese, Korean, Vietnamese, Thai**. Do not attempt to repurpose it for Western keyboard layers or general remapping.



## Part 4: Software Remapping Layers — Different Approaches

While Windows keyboard layouts offer only AltGr and dead keys for special characters, software can add additional transformation layers by intercepting the keyboard chain at different levels.

### Two Practical Interception Approaches

#### Level 1: Firmware (QMK, ZMK)

**Where it operates:** At the keyboard controller itself, **before HID usage IDs are sent**, at the very source of input.

```
Hardware key → Firmware processes → Modified HID usage ID → Windows HID driver → Scancode → VK → Layout
```

**Interception point:** Pre-HID (hardware level)

**Scope:** System-wide (works everywhere — games, BIOS, all applications)

**Capabilities:** Unlimited (layers, macros, combos, hold-tap, timed actions)

**Trade-offs:**
- **Advantages:** Cleanest approach; Windows sees standard HID; no hook side effects; works everywhere; no dead key conflicts
- **Disadvantages:** Requires programmable keyboard; steeper learning curve (C code)

**Why it's best:** The OS is unaware remapping happened. No conflicts with Windows keyboard layout system.



#### Level 2: Hook-Level User-Mode (Kanata LLHOOK, AutoHotkey)

**Where it operates:** Via the WH_KEYBOARD_LL low-level keyboard hook, which intercepts **before messages are posted to the application's message queue**, but **after Windows has already generated the VK code**.

```
Hardware keystroke
  ↓
Windows generates WM_KEYDOWN message (with VK code already resolved)
  ↓
WH_KEYBOARD_LL hook executes (receives WM_KEYDOWN with VK code in KBDLLHOOKSTRUCT)
  ↓
Hook can: block message (return 1), MODIFY the VK code, or inject new messages
  ↓
If hook returns 0: message with (possibly modified) VK code is posted to thread input queue
  ↓
Application's message loop calls GetMessage() → receives WM_KEYDOWN
  ↓
Application calls TranslateMessage() → keyboard layout DLL calls ToUnicode() with hook-modified VK code → WM_CHAR generated
```

**How Kanata LLHOOK achieves remapping:**

The hook modifies the VK code in the KBDLLHOOKSTRUCT before the message reaches the application. When the application later calls `TranslateMessage()`, the Windows keyboard layout DLL uses this **modified VK code**, producing different character output than the original key.

```
Example: Remap A key to B key
1. User presses A
2. Windows generates WM_KEYDOWN with VK_A
3. Kanata hook intercepts, modifies KBDLLHOOKSTRUCT.vkCode from VK_A to VK_B
4. Modified message posted to application's queue
5. Application calls TranslateMessage()
6. Keyboard layout DLL sees VK_B, outputs 'b' instead of 'a'
```

**Interception point:** Post-VK generation, but **before the application's message loop receives it**. The hook can modify the VK code before posting.

**Scope:** Usually system-wide, but some applications (especially games using raw input API) bypass hooks entirely. Also does not affect programs running with elevated privileges unless Kanata itself runs with admin rights.

**Capabilities:** High for typical use cases (remapping, macros, layer switching)

**How it works with the keyboard layout:** The hook doesn't bypass the keyboard layout DLL. Instead, it changes which VK code the layout DLL receives, so the layout DLL produces different output. The hook operates at the VK level, not the character level.

**Privilege requirements:**
- Works without admin rights, but will not affect programs running with elevated privileges (like admin terminal)
- Can be run with admin rights to also affect admin-level programs

**Trade-offs:**
- **Advantages:** Accessible (scripts vs. C code); easy to disable; no driver development needed; can remap to different characters by modifying VK codes
- **Disadvantages:** Destroys dead key state if calling `ToUnicode`[2][9]; interferes with hotkey recording; some applications bypass hooks (raw input games); performance overhead; does not affect elevated privilege programs without admin rights

**The dead key problem:** If the hook calls `ToUnicode` to examine or process input, it destroys any active dead key state. This is why dead keys stop working when using certain remapping tools. A well-designed hook that only modifies VK codes (without calling `ToUnicode`) won't destroy dead keys.



#### Level 3: Driver-Level Interception (Kanata Interception, Interception driver)

**Where it operates:** At the Windows keyboard driver layer, **before HID-to-scancode conversion**, intercepting at the kernel driver level.

```
Hardware key → Interception driver intercepts (BEFORE HID processing) → Modified signals sent to Windows HID → Scancode → VK → Layout
```

**Interception point:** Pre-HID / Pre-scancode (kernel driver level)

**Scope:** System-wide in normal Windows operation

**Capabilities:** Very high (can transform at driver level before Windows processes input)

**Trade-offs:**
- **Advantages:** Dead key state is safe (operates before Windows kernel buffer); more powerful than LLHOOK for system-wide coverage; affects privileged programs
- **Disadvantages:** Requires kernel driver installation; more complex setup; requires Interception driver (https://github.com/oblitum/Interception) which is not actively maintained (last update 2017); known stability issues[12]

**Known issues:**
- Stability issues reported on Windows 11: sleeping/unplugging devices can cause input failures[12]
- Some less-frequently used keys not supported or handled correctly
- Driver maintenance status uncertain



### Practical Comparison

| Aspect | Firmware | LLHOOK (Kanata/AutoHotkey) | Driver (Kanata Interception) |
|--------|----------|---------------------------|---------------------------|
| **Works in typical use** | Yes | Usually (games bypass) | Yes |
| **Works with elevated programs** | Yes | Only if Kanata has admin rights | Yes |
| **Dead key safe** | Yes | Depends on implementation[2] | Yes |
| **Learning curve** | Steep (C) | Low (scripts) | Low (scripts) |
| **Hardware required** | Programmable keyboard | Any | Any |
| **Stability** | High | Moderate | Moderate (reported issues[12]) |
| **Best for** | Dedicated custom keyboard | General remapping, accessibility | Full system coverage needed |



## Part 5: Kanata's Two Windows Implementations

{{< figure src="kanata-icon.svg"
           alt="Kanata"
           class=""
           caption="" >}}

Kanata, a popular open-source keyboard remapper, offers two different Windows implementations that highlight the trade-offs between hook-level and driver-level interception:

### LLHOOK (Default): `kanata.exe`

**How it works:**
- Uses Windows keyboard hook (WH_KEYBOARD_LL)
- Modifies VK codes before messages reach the application's message queue
- Works with or without admin privileges (with limitations)

**Pros:**
- Simple setup (just run the executable)
- Works in most applications
- Lower complexity

**Cons:**
- **Can destroy dead key state** if configured to call `ToUnicode`
- Games using raw input bypass it
- Does not affect programs running with elevated privileges unless Kanata itself is run as admin

**Privilege behavior:**
- Without admin: Works for normal user programs
- With admin: Also works for admin-elevated programs

**Use case:** General-purpose keyboard remapping where dead keys aren't critical, or where configuration is set up to avoid calling `ToUnicode`.



### Interception Driver: `kanata_wintercept.exe`

**How it works:**
- Uses the Interception driver (https://github.com/oblitum/Interception)
- Intercepts at kernel driver level (before HID processing)
- Requires driver installation
- Kanata provides the precompiled Interception driver in releases

**Pros:**
- **Safe with dead keys** (intercepts before dead key buffer exists)
- Works with games using raw input
- Works system-wide including privileged programs
- Captures input before Windows processes it

**Cons:**
- More complex setup (requires driver installation)
- Known stability issues: sleeping/unplugging devices can cause input failures[12]
- Interception driver is not actively maintained (last update 2017)
- Some less-used keys may not be supported

**How to use:**
1. Download Kanata release (includes Interception driver)
2. Run `install-interception` from the command line as administrator
3. Run `kanata_wintercept.exe` instead of `kanata.exe`

**Key differences in configuration:**
- Key naming differs between LLHOOK and Interception modes
- Configuration files may need adjustment (use `--debug` flag to find correct key codes)
- LLHOOK and Interception key codes are **not compatible** — you need separate configs or use conditional logic

**Use case:** When you need dead key support, game compatibility, or system-wide coverage including privileged programs; requires acceptance of stability caveats and potential hardware issues on Windows 11.



## Part 6: Summary — Choosing Your Approach

**For accessing special characters on an existing layout:**
- Use **AltGr** if your layout defines it (fast, immediate)
- Use **dead keys** if you need true composition (but understand the limitations and avoid tools that call `ToUnicode`)

**For general keyboard remapping or custom layers:**
- **Firmware (QMK)** if you have a programmable keyboard (cleanest, most powerful, fully reliable)
- **Kanata LLHOOK** (`kanata.exe`) if you need quick cross-platform remapping (accessible, may have dead key issues depending on configuration; can be run with or without admin)
- **Kanata Interception** (`kanata_wintercept.exe`) if you need dead key support, game compatibility, or system-wide coverage; requires accepting stability caveats and unmaintained driver status
- **AutoHotkey** for simple scripting (accessible, but test with dead keys carefully)
- Avoid attempting to use IME for general layers — it's strictly for language input

**For language input:**
- Use native **IME** for Japanese, Chinese, Korean, Vietnamese, Thai
- Don't repurpose IME for Western keyboard remapping

**Dead key best practices:**
- If using dead keys, test your remapping tool configuration carefully to ensure it doesn't call `ToUnicode` in ways that destroy dead key state
- Use firmware-level solutions if you need guaranteed dead key compatibility
- Understand that dead keys cannot be canceled — you must press a key to clear the waiting state
- Test your dead key setup thoroughly after any remapping tool installation

---

## References

[1] [Windows: Ctrl+Alt keyboard shortcut conflicts with AltGr](https://www.reddit.com/r/windows/comments/xsohxq/dead_keys_on_keyboard_layouts/)

[2] [Stack Overflow: ToAscii/ToUnicode in a keyboard hook destroys dead keys](https://stackoverflow.com/questions/1964614/toascii-tounicode-in-a-keyboard-hook-destroys-dead-keys)

[3] [Reddit: Tip: Use IME under Windows with a non-US keyboard layout](https://www.reddit.com/r/LearnJapanese/comments/f2x6mh/tip_use_ime_under_windows_with_a_nonus_keyboard/)

[4] [Microsoft Learn: Japanese IME](https://learn.microsoft.com/en-us/globalization/input/japanese-ime)

[5] [Learn Japanese: Japanese typing](https://learnjapanese.moe/ime/)

[6] [Microsoft Learn: How can I use the Japanese IME with non-Japanese keyboard](https://learn.microsoft.com/en-us/answers/questions/3292110/how-can-i-use-the-japanese-ime-(input-method-edito)) 

[7] [Microsoft Learn: Input Method Editors (IME)](https://learn.microsoft.com/en-us/windows/apps/develop/input/input-method-editors)

[8] [Microsoft: Dead Keys ... how to undo these?](https://learn.microsoft.com/en-us/answers/questions/4126343/dead-keys-how-to-undo-these)

[9] [AutoHotkey Community: Hotstrings breaks chained deadkey](https://www.autohotkey.com/boards/viewtopic.php?t=125016)

[10] [Stack Overflow: Correct logic for interpreting SetWindowsHookEx](https://stackoverflow.com/questions/17901828/correct-logic-for-interpreting-setwindowshookex-wh-keyboard-ll)

[11] Personal research note on Shift + dead keys Windows limitations

[12] [Kanata: Platform Known Issues - Windows 11](https://raw.githubusercontent.com/jtroo/kanata/main/docs/platform-known-issues.adoc)
