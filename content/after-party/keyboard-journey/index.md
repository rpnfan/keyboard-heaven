---
title: "My Keyboard Journey—From QWERTZ to anymak:END"
description: "Why I abandoned decades of QWERTZ typing and in the end created my own keyboard layout anymak:END. A journey toward comfortable, multi-language optimized typing."
date: 2022-12-26
---

{{< figure src="Lily58-choc-red-white.jpg"
           alt="Lily58 Choc"
           class="fullwidth"
           caption="" >}}

## Touch-Typing on German QWERTZ: A Growing Frustration

I learned touch typing as a teenager and typed extensively throughout my studies and career. I was a happy typis, reaching a speed of 60–70 WPM on real-world text with high accuracy.

I began to get annoyed with the keyboard ergonomics, or better said the lack thereof, as soon as I started using LaTeX and programming during my studies. The symbols I needed constantly -- backslashes, brackets, curly braces and more were trapped behind the AltGr layer. AltGr isn't reachable from home row without uncomfortable hand contortions. And it's unreliable across different keyboards and systems.

Around the same time, I began using Vim. That experience taught me something important: keeping your hands on the home row is powerful. Moving your hands away breaks rhythm and slows you down. When I could navigate and edit without leaving home row in Vim, everything flowed better.

I realized I could apply that same principle to typing besides the AltGr problem needing a solution.


## 2007–2022: DeutschlandPlus

I noticed that the CapsLock key on my keyboard never got used. What if I turned it into a layer modifier? Then I could access all those symbols from home row, plus bring navigation keys (arrows, Home, End, PageUp/Down) into reach—just like in Vim.

I created **DeutschlandPlus**, an Autohotkey script that transformed my keyboard into a more comfortable input device. When you held CapsLock, the right-hand keys became symbols and navigation. HJKL became arrow keys, just like in Vim. Brackets, braces, backslashes—all accessible without moving my hands.

It was simple and elegant. The solution maintained full compatibility with the standard QWERTZ layout. No relearning needed—just a few days to get acquainted with the new features. As a bonus the extra layer was transparent to everyone else; I could even run it on shared computers in a lab without anyone noticing.

Around 2010, I looked into alternative keyboard layouts like Neo. But I decided not to abandon my years of QWERTZ practice. DeutschlandPlus felt like the right balance.

For 15 years, it was.


## 2022: Exploring Alternatives

By 2022, after 15 years using DeutschlandPlus, I started wondering: *Could I do better?*

The answer required something I'd avoided: changing the base layout itself.

I tried Minimak—swapping just a few keys. But I quickly realized the gains would be limited. Next was Colemak. That works reasonably well for English, but it's much worse for non-English languages.

Because I type in German and Dutch besides English, I developed my own custom keyboard layout. Using Colemak's design principles (minimize changes, keep XCV on the left for shortcuts), I created a layout that achieved nearly Colemak's English performance while performing much better for German, Dutch, and other languages.

I practiced for about one month on keybr.com before making the full switch at around 40 WPM. As I got faster over the coming weeks and reached 50–60 WPM, I realized something disheartening: the left hand felt great, but the right hand was still too busy. Many patterns involved only or mainly the right hand. The improvement wasn't enough to justify the relearning.

I was disappointed. But then I realized something important: I'd just proven that optimization *works*. A better layout *does* feel better. So why settle for partial optimization?

I decided to go the full way: find a fully optimized layout.



## The Breakthrough: Discovering 'opt' and Multi-Language Optimization

Around the same time, I discovered Andreas Wettstein's 'opt' optimizer, which had been used to create the AdNW and KOY layouts around 2010. These layouts, based on Dvorak principles, had proven to be excellent solutions for English and German typists.

Maximilian Schillinger's articles demonstrated how to use 'opt' to develop and fine-tune personal layouts. I realized I could build a layout optimized specifically for English, German, and Dutch—the exact languages I typed.

I spent the rest of 2022 developing it. Using 'opt' extensively, testing practically, tweaking manually. The result was **END** (English, Deutsch, Nederlands)—named after my goal: the final destination of my keyboard journey.



## 2023: Learning Anymak + END

In 2023, I refined the END layout further using 'opt' and more practical testing. I also discovered something important about layers: **Space as a modifier works better than CapsLock.** Using Space as a layer trigger (SpaceFN) was easier to use and allowed both hands to participate, not just the right one. I integrated navigation keys, Enter, Backspace, Alt+Tab, copy/paste combos, and more.

Then came the real breakthrough: **shifting Shift itself.** Instead of keeping Shift in its default top-right position, I placed it on easy-to-reach keys—normally used for characters. But as one-shot modifiers (tap Shift, and the next keystroke gets capitalized), not held keys. This eliminated all finger strain from holding Shift.

I also discovered that the B-key had to be repositioned when designing for both standard row-staggered and split columnar keyboards. This constraint became a key design decision.

I named this complete system **Anymak**: the refined layer system paired with optimized alphanumeric layouts. The full name is **anymak:END**—Anymak layers on top of the END alphanumeric layout. Future variants would be anymak:COLEMAK, anymak:GRAPHITE, and so on.

Before switching, I practiced on keybr.com again—about 20 minutes most days—until I reached around 40 WPM. Then I switched for real.

The learning was intense at first. I reached 50–60 WPM after about one month. But true fluency—where typing felt as automatic and natural as QWERTZ had—took about 2 years. Remember, I'd been typing QWERTZ for decades at that point. The muscle memory was deeply ingrained. The umlauts were the last thing to feel natural, because I'd moved them to a layer—but this placement actually benefited from the symmetry of using the opposite hand for the layer-switch modifier, just like for Shift.

After the initial keybr.com practice, I didn't practice intensively during those 2 years of real-world use. This likely extended the fluency timeline. But it also proved something important: **anymak:END works intuitively. You don't need obsessive drilling for it to become natural.**



## 2024: Finalized

By 2024, I settled on the complete system with bottom-row modifiers and found hardware I'm satisfied with—a customized Lily58 (a split keyboard) with a 1.75u wide main thumb key for more comfortable modifier access.

I'm not tweaking anymore. The system is optimized. The problem I set out to solve—symbols trapped behind AltGr, navigation unreachable from home row, and later, multi-language typing efficiency—is solved. And it works on both standard row-staggered keyboards and split ergonomic boards.



## The Learning Process: An Unexpected Insight

This journey taught me something beyond keyboard comfort. I wanted to understand how to learn better—to transfer that knowledge to other areas, like practicing a musical instrument.

What I learned: **consistent daily practice beats occasional marathon sessions.** I improved more from 20 minutes daily on keybr.com than I would have from 3-hour weekend sessions. I also discovered that deeply ingrained patterns are hard to change—but it *is* possible if you're willing to invest the effort. And perhaps surprisingly, that effort doesn't have to be intense. Steady, consistent practice works.



## The Trade-Off: Comfort vs. Flexibility

I gained a genuinely comfortable typing experience. After decades of QWERTZ, my hands no longer ache. Typing flows. Patterns that were awkward now feel natural.

What I lost: flexibility. I can't walk into an office and type quickly on a shared keyboard. Work machines offer vanilla QWERTZ. I deliberately chose not to maintain active QWERTZ skills, because split attention would have made the learning process harder. But because I only use computers I control (my work laptop, a home desktop, my personal laptop), this flexibility loss doesn't matter.

The trade-off was worth it. For me.

---

## For Your Own Journey

This was my path. But you don't need to follow it completely. You may find your sweet spot at DeutschlandPlus a solution I used 15-year for good reason. Or you like the extra features of Spacemak or Anymak? And in the case you are even more willing than me to loose some compatability to standard keyboards you might choose a thumb-cluster keyboard.

Your keyboard is something you often use for hours every day. It can be worth to optimize your keyboard usage well. But "well" means different things to different people.


## What's Next?

If you're interested in exploring your own keyboard optimization, see the [steps](/) to different paths to your own "keyboard heaven."

