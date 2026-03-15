---
title: "Best practice to launch Kanata with Windows"
description: "Start Kanata with Windows so it works reliably, also for programs running with admin rights."
date: 2026-03-15
cover:
  image: "kanata-task-scheduler-teaser.png"
  alt: "Kanata Task Scheduler"
  caption: ""
  relative: true

weight: 2
---


## Why you cannot put Kanata in the Autostart folder

To start Kanata automatically with Windows, the Autostart folder is the obvious first choice — but it has a limitation. Kanata (LLHook version) only becomes active after login. While placing a shortcut in the Autostart folder works for most applications, it will not work for Kanata when you need to run it with administrator rights — which is required for Kanata to function in Terminal windows or other programs that run elevated.


## Using Task Scheduler to automatically start Kanata at login

1. Save the Kanata executable in a folder where you keep your portable applications. A convenient location for most Windows users is something like `C:\portableApps\Kanata\kanata_windows_gui_winIOv2_x64.exe`.
2. Store your Kanata `.kbd` config files in the same folder. If you use only one config file, naming it `kanata.kbd` is most convenient, as Kanata will find it automatically. You can also use multiple config files with freely chosen names. I use three: two for different keyboard layouts, and a third empty config file that effectively disables Kanata — useful for quick debugging or when someone else needs to use your PC with the default layout.
3. Open Task Scheduler and create a new task by clicking **Create Task** under **Actions** on the right-hand side:
{{< figure src="ts0.png"
           alt="Create Task"
           class=""
           caption="" >}}

4. Under **General**, apply the following settings:
{{< figure src="ts1.png"
           alt="General tab"
           class=""
           caption="" >}}

5. Under **Triggers**, create a new trigger:
{{< figure src="ts2.png"
           alt="Triggers tab"
           class=""
           caption="" >}}

and fill in the following details:
{{< figure src="ts2b.png"
           alt="Trigger settings"
           class=""
           caption="" >}}

**Why add a startup delay?**
A delay ensures Kanata does not launch before all required Windows components have loaded. Starting too early can cause Kanata to malfunction — for example, some key remaps may not respond, or the taskbar icon may not appear. Both issues are resolved with a sufficient delay. Experiment with different values to find what works for your system. On my ARM64 laptop a 30-second delay is enough, while my x86 machine needs 45 seconds. Windows appears to reach a ready state faster on ARM64.

6. Under **Actions**, create a new action:
{{< figure src="ts3.png"
           alt="New action"
           class=""
           caption="" >}}

and configure the path and parameters to match your setup:
{{< figure src="ts3b.png"
           alt="Action parameters"
           class=""
           caption="" >}}

Here are the parameters I use as an example:

- **Program:** `C:\portableApps\Kanata\kanata_windows_gui_winIOv2_x64.exe`
- **Arguments:** `-c kanata.kbd -c anymak-end.kbd -c kanata-zero.kbd`
- **Start in:** `C:\portableApps\Kanata`

The `-c` parameter specifies which config file to load. You can omit it if you only use a single `kanata.kbd` config file.

The file `kanata-zero.kbd` deactivates all remapped keys:
```
#|
kanata config to deactivate all kanata commands (switch off basically)
|#

;; ======================================= Setup ======================================

(defcfg
  process-unmapped-keys yes
  concurrent-tap-hold yes
)

(defsrc)
(deflayer base)
```

7. On the **Settings** tab, apply the following options:
{{< figure src="ts4.png"
           alt="Settings"
           class=""
           caption="" >}}

8. Save the task. You can test it immediately by right-clicking and selecting **Run**. On the next login, the task will start Kanata automatically. Note that the login screen uses the default Windows keyboard layout — Kanata is not active there.