---
title: "Keyboard Layout Analyzers Explained: What They Can (and Can't) Do"
description: "Keyboard layout analyzers are powerful tools—but they have significant blind spots. Learn what they can and can't tell you about keyboard optimization, and how to get most out of them."
date: 2026-01-11
lastmod: 2026-03-17  # Updates to current date on edit
cover:
  image: "keyboard-analyzer.png"  # Relative to static/ or page bundle
  alt: "Keyboard Analyzers"
  caption: ""
  relative: true  # For page bundles

weight: 1

draft: true
---


## What Layout Analyzers Do Well

Layout analyzers are powerful tools. They can save many hours of manual work and help to get a good grasp of a keyboard layout much faster than without. They provide objective data of keyboard layout characteristics, such as bigram distributions, which would take weeks to calculate by hand. Some offer additional visual insights such as showing finger paths, heatmaps of finger use, and hand alternation charts. They let you compare dozens of layouts against consistent metrics. They've helped discover genuinely useful layouts like AdNW, Enthium, KOY and Graphite.

Several analyzers are commonly used in the community. [opt](https://509.ch/opte.htm) is particularly well-documented and supports extensive customization, including finger-path visualizations. [Cyanophage's playground](https://cyanophage.github.io/playground.html) is a web-based tool useful for quickly exploring layouts and identifying hard-to-type words. [Jalo](https://github.com/tiagowright/jalo) offers another perspective with its own metric set. [Dario Görtz's analyzer](https://github.com/dariogoetz/keyboard_layout_optimizer) focuses on layout optimization with configurable weights. [Aba](https://github.com/mohoaz1348-rgb/layout_bigrams_analyzer) rounds out the field with a bigram-focused approach. Each has different strengths and blind spots — which is itself worth keeping in mind.

**But analyzers have significant limitations.** Understanding these limitations is essential before trusting an analyzer's conclusion that one layout is "better" than another.

## How Analyzers Score a Layout

All analyzers try to score how comfortable or easy certain finger movements are. They either provide numbers for some kind of "effort" (where lower is better) or for metrics considered desirable (where higher is better). Key position and same-finger repetition are typical effort factors; hand alternation and inward rolls are examples of the desirable kind. A general discussion of how to rate a keyboard layout will be covered in a forthcoming article on keyboard design principles.

## The Three Core Limitations of Layout Analyzers

### Limitation 1: Not All "Bad Motions" Are Equal, But Analyzers Treat Them As Equal

Same-finger bigrams (SFBs) are a good example. An SFB on a strong finger (index or middle finger) from the top row back to the home row (like E-D in QWERTY) is fundamentally different from an SFB on a weak finger (ring finger or pinky) from the home row down to the bottom row (like S-Z in QWERTY).

**Why the difference?** When you press E-D, your finger leaves the home position briefly and returns naturally as part of the next keystroke. The motion still flows. Depending on the keycaps you might even be able to "rake" the finger from one key to the other. In contrast, S-Z requires your weak finger to stretch down and return to home position — a much more disruptive motion.

**What analyzers typically do:** Count both as "1 SFB" with equal weight.

**What they miss:** These motions have different real ergonomic costs. A layout with a moderate count of SFBs might actually feel *better* if most of those SFBs are the harmless top-to-home variety on strong fingers.

**Real example:** For English, anymak:END has a higher SFB percentage than some modern layouts (like Graphite). Graphite has 1.1% SFBs (according to opt with standard corpus) while anymak:END has 1.7% SFBs. But most of anymak:END's SFBs occur between the top row and home row on strong fingers (index/middle) — the comfortable type. The raw numbers say Graphite is better in this regard. That is true by counting each SFB equally. But the actual finger effort is not described with enough granularity, so you cannot conclude from that single number alone whether or by how much one layout will feel better in practice.

**The same issue applies to scissors** (two-finger twisting motions). Q-S in QWERTY is worse than A-W, but most analyzers count all scissors equally. A layout with more scissors might feel better if they're all low-impact combinations. Some scissors that look bad on paper actually feel fine in practice — E-F in QWERTY is one example.

**The solution analyzers miss:** A proper model would need a table rating all possible finger-pair combinations by their actual ergonomic cost. This is theoretically doable, but validating it would require controlled experiments with real typists.

### Limitation 2: Metrics Are Combined Without Understanding Their Relative Weight

Layout analyzers typically measure multiple metrics: same-finger bigrams, hand alternation, inward rolls, outward rolls, scissors, redirects (sequences where your fingers reverse direction on the same hand mid-word), distance traveled, and more. These are often combined into a single "effort score."

**The problem:** We don't know how much each metric actually contributes to how good a layout feels. Nor is it well understood how combinations of finger motions across bigrams and trigrams interact.

**Example:** Analyzer X weights SFBs at 30%, hand alternation at 25%, scissors at 20%, and other factors at 25%. But who decided on those weights? Were they tested with real typists, or were they a reasonable-sounding guess?

**The mathematical consequence:** Imagine an analyzer says Layout A has 2.3% total effort and Layout B has 2.1% effort. The conclusion seems clear: Layout B is better by 0.2%. But if the underlying uncertainty in measuring these metrics is, say, several times larger than that gap — a plausible scenario given that the weights are not empirically validated — then that 0.2% difference is noise, not signal. It's like measuring a door's width to 0.1 mm precision when your tape measure has ±1 mm error.

**Why we don't know the weights:** Determining them would require controlled experiments with both trained and untrained typists. Do typists notice a 0.5% reduction in same-finger bigrams? A 1% improvement in hand alternation? By how much? These are empirical questions, not mathematical ones, and the research largely hasn't been done.

**The practical consequence:** When two layouts are numerically close, don't try to pick a winner based on the combined score. Compare individual metrics separately to understand the trade-offs, and then test both by typing real text. The combined "effort score" difference may be below measurement uncertainty. What you *can* reliably do is compare layouts on a single metric at a time and get a basic sense of how they differ on that one dimension — nothing more, nothing less.

### Limitation 3: Layer Switches Are Completely Ignored but They Are Crucial

Layout analyzers examine the character positions of the base layer (where all the letters live). They mostly ignore one of the biggest ergonomic factors: **where your modifier and layer-switch keys are placed, and how they're implemented.**

**Why this matters:** In typing:
- **Shifted characters** (capitals, symbols) require a layer key (Shift)
- **Language-specific characters** (umlauts in German, accents in French) often require a layer key or dead key
- **One-shot vs held** layer keys (tap vs hold) change the ergonomic cost dramatically
- **Auto-Shift, Combos** or other timed approaches to switch to layers are not considered

**Example 1: Shift placement and one-shot vs held modifiers**

In QWERTY, Right-Shift lives on the pinky at the bottom corner — exactly where your pinky has to stretch quite far and you possibly need to twist your hand uncomfortably. For English, this is annoying already. But for German, where capitalizing nouns is mandatory, Shift is in heavy use and the pinky strain adds up considerably. On an ANSI keyboard, Left-Shift is much easier to reach by curling the pinky inward, and with nicely sculpted keycaps that motion can even feel comfortable. The position of the Shift key matters and should be weighed accordingly.

A unique characteristic of **anymak:END is moving Shift to easily reachable positions**. By placing Shift on the /-key (QWERTY position), this problem is resolved. In addition, Shift is a one-shot key on both hands, which minimizes SFBs between character keys and the Shift key.

When you hold Shift while typing a capital letter, your hand is in a constrained position and can't move naturally. When you use one-shot Shift — tap Shift, and the next keystroke is capitalized — your hand is free between each keystroke. This is more comfortable and feels faster. Both the better finger placement and the reduction in SFBs from one-shot behavior are real-world advantages, but they don't appear in most analyzer output because analyzers neither examine layer-key placement nor know whether you use one-shot switching.

**Example 2: Language-specific costs**

For **English**, Shift is used occasionally. Optimizing its placement matters but is less critical than for other languages.

For **German**, where capitalizing nouns means Shift is in constant use, its placement becomes a major ergonomic factor. A layout optimized for English may feel noticeably worse for a German typist.

For **Hungarian**, with heavy use of diacritics and multiple layer-switch keys, the position of *all* layer keys is crucial. An analyzer that ignores layer placement will produce a layout that feels awkward in practice.

**The current gap:** Layer-key placement represents a large optimization space that most analyzers completely ignore. It's not a minor issue; it has a serious impact on everyday feel, particularly for non-English typists.

**Example 3: Magic keys and context-dependent optimization**

Some layouts like **Magic Sturdy** use "magic keys" — special keys that produce different output based on the surrounding typing context. Unlike layer keys, magic keys *optimize specific keystroke sequences* by generating an alternative character based on what was typed before, often to eliminate an SFB or awkward redirect.

Analyzers can be configured to account for magic keys by pre-computing their likely outputs and folding those patterns into the analysis. If properly set up, an analyzer *can* confirm that magic keys are actually reducing SFBs or improving alternation as intended.

What the analyzer still can't measure is the cognitive side of the trade-off. Magic keys require learning when they activate and trusting that they'll behave as expected. Some typists find this becomes natural with practice and genuinely enjoy the optimization; others find the conditional behavior unpredictable and mentally tiring, even when the physical metrics look better. It's a genuine trade-off between physical ergonomics — which analyzers can measure — and cognitive ergonomics, which they can't. Whether that trade-off is worthwhile depends on the individual, and the only way to find out is to test the layout with real text.

## Why Limitations Exist: The Structure of Analyzers

These limitations don't exist in isolation. They come on top of several structural challenges:

### The Corpus Problem

An analyzer needs a text corpus (a sample of writing) to analyze. But different corpora produce different "optimal" layouts:

- **Typing test corpus** (common words, balanced): Produces layouts good for general typing
- **Programming corpus** (lots of symbols, brackets, semicolons): Produces layouts that prioritize symbol access
- **German corpus with many capitals**: Produces layouts where Shift/layer placement becomes more important
- **Prose corpus**: May de-prioritize symbols and focus on letter frequency

**The consequence:** An optimizer produces a layout that's optimal for that specific corpus — not necessarily for your actual typing. If you code 50% of the time and write messages the other 50%, but the analyzer trained only on prose, the result might feel wrong for your real use.

### The Hardware Variability Problem

Analyzers often assume a standard ANSI or ISO keyboard with row-staggered keys. But keyboards vary considerably:

- **Row-staggered** (standard): Key columns are offset by rows
- **Column-staggered** (ergonomic split keyboards): Key columns are aligned vertically; the amount of stagger varies significantly between models
- **3×5 vs 4×6 + thumb keys**: Ergonomic splits offer different key counts and thumb cluster sizes — which keys carry characters, and which handle layer switching?
- **Keycaps, key size and switches**: Thumb key dimensions, spacing (MX vs Choc), and keycap profile all affect which keys are comfortable and precise to use
- **Nontraditional keyboards**: Svalboard and similar devices are difficult to map meaningfully to standard analyzer assumptions
- **Hand size variation**: What's comfortable for one person may require stretching for another

**The consequence:** A layout optimized for one hardware type may feel less natural on another. The "optimal" reach map differs between keyboard designs.

**anymak:END's approach:** It was specifically designed to work on both standard row-staggered and split columnar-staggered keyboards with moderate pinky stagger. Avoiding the ANSI B-key position was a deliberate hardware-compatibility requirement to keep the same fingering on both keyboard types. Even so, an analyzer won't fully reflect actual finger effort unless it's configured with the exact key positions and reach characteristics for the specific user and hardware.

### The Search Algorithm Problem

Analyzers use algorithms — usually simulated annealing or genetic algorithms — to search the space of possible layouts. This search has inherent limits:

- **Local optima:** The algorithm may find a good layout that isn't the *best* one. Like a hiker who climbs a tall hill and declares it the peak without seeing the taller mountain nearby.
- **Search space is enormous:** With 47 keys and tens of thousands of possible bigrams, perfect optimization is computationally infeasible.

Optimizer-generated layouts are usually good starting points, but they're not guaranteed to be globally optimal.

## How Context Factors Interact: A Practical Example

Let's trace how these factors play out across two different layouts:

**Graphite:**
- **Corpus:** Optimized for modern English typing
- **Hardware:** Works on standard ANSI keyboards
- **Metrics:** Low SFBs (1.1%), excellent hand alternation, good rolls
- **Layer consideration:** Standard Shift position
- **Result:** Feels good for English typing; less ideal for heavy capital use or multiple languages

**anymak:END:**
- **Corpus:** Optimized on equal weighting of English, German, and Dutch
- **Hardware:** Designed to work identically on both row-staggered and column-staggered keyboards
- **Metrics:** More SFBs than Graphite (1.7%), but mostly the harmless top-to-home type on strong fingers; excellent hand alternation; strong-finger prioritization
- **Layer consideration:** One-shot layer keys in comfortable positions, symmetrical left-right placement, B-key position (QWERTY) deliberately unused for hardware compatibility
- **Result:** Comfortable for multi-language typing; Shift-heavy use (German) feels better due to one-shot placement; works on any keyboard; more SFBs on paper but less actual finger effort due to avoiding harder-to-reach keys

No analyzer can capture all these factors simultaneously. A layout's quality depends on how well it matches *your* corpus, *your* hardware, *your* layer use, and *your* typing patterns.

## How to Use Analyzers Wisely

Given these limitations, here's how to get genuine value from layout analyzers:

**1. Use analyzers for exploration, not gospel**

Run an analyzer on several layout candidates. Use the **graphical output** — finger paths, heatmaps, hand alternation charts — to build an intuitive picture of the differences. Don't fixate on the raw effort score. The finger-path charts from opt are a particularly useful complement to the numbers.

**2. Compare metrics separately, not as a combined score**

Instead of trusting a single "effort score," look at individual metrics in isolation:
- How many SFBs, and where are they? (Top-row-to-home SFBs are less of a concern than home-to-bottom.)
- What's the hand alternation? (Higher is generally better.)
- How many inward rolls? (Higher is better.)
- How many scissors, and of what type? (Pinky scissors are worse than index-finger scissors; which finger is up and which is down matters.)
- How many redirects, especially on weak fingers?

Keep these separate. A layout strong in one area will often be weaker in another, and that trade-off might suit your typing well. The goal isn't the perfect layout — which can't exist — but the right balance for your needs.

**3. Match the corpus to your actual typing**

If you write code, use a programming corpus. If you write primarily in German, use German text with proper capitalization and umlauts. Make the corpus large enough — the opt documentation has guidance on how to check this.

**4. Factor in layer-key placement manually**

Most analyzers ignore this, but you shouldn't. Ask yourself:
- Where are my Shift and layer keys?
- Are they one-shot or held? Or do you use auto-shift, combos, or home-row mods?
- Are they on weak fingers (pinkies) or strong ones (index/middle), or on thumbs? How comfortable are they to reach?
- If you type heavily in a language with many capitals or diacritics, is layer placement considered?

This is a significant portion of the optimization space that analyzers routinely miss.

**5. Test numerically similar layouts by actually typing**

When an analyzer shows two layouts as nearly equivalent, don't try to pick based on small score differences. Instead:
- Try both with a layout simulator that remaps your current layout to the candidate
- Type real text that reflects your actual use. Frequently used words are a good start; it's also worth identifying "ugly" words — sequences that play to a layout's weak spots. Cyanophage's analyzer is useful for finding these.
- Notice which layout feels more natural across a range of words. Your hands will tell you things the metrics won't.

**6. Use analyzers to understand trade-offs, not to declare a winner**

When you have a shortlist of 2–3 layouts, use the analyzer to characterize what each one prioritizes:
- Layout A optimizes for hand alternation but has more SFBs
- Layout B minimizes SFBs but uses more same-hand patterns
- Layout C prioritizes inward rolls at the cost of distance

This framing helps you predict which layout will suit your hands and typing patterns. The numerical winner may not be the best choice for you specifically.

**7. Consider what hardware the layout was designed for**

If a layout was optimized for one keyboard type, some of its advantages may not carry over to yours. Where possible, configure the analyzer to reflect your actual physical key arrangement so the evaluation is meaningful.

**8. Verify how metrics are weighted**

If a layout optimizer uses guessed weights, its combined effort score is less reliable than it appears. Check the per-key effort values and adjust them to reflect your own assessment of which keys are harder or easier to reach on your specific hardware.

## Analyzer Limitations in Practice: Real Examples

### Example 1: Graphite vs anymak:END

An analyzer might show:
- Graphite: Fewer SFBs (1.1%), excellent hand alternation, optimized for English
- anymak:END: More SFBs (1.7%), similar hand alternation, optimized for 3 languages

**Raw verdict:** Graphite is slightly better on SFBs; roughly equivalent on hand alternation.

**But what the analyzer misses:**
- Graphite's Shift is standard (pinky on ANSI keyboard); anymak:END's Shift is in a comfortable one-shot position on both hands
- Graphite was optimized for English; anymak:END was optimized for equal English-German-Dutch use and works well with French and Spanish
- anymak:END works on both standard and columnar-staggered keyboards with identical fingering
- If you type German 40% of the time with frequent capitals, anymak:END's Shift optimization is a practical advantage that raw metrics from most analyzers won't capture

Analyzer metrics are comparative but not absolute. They show relative trade-offs within a narrow context, not which layout you'll actually prefer given your real usage.

### Example 2: Enthium (Thumb-Character Layout)

Enthium is an unusual layout that places common characters on thumb keys rather than finger keys.

**What an analyzer sees:**
- Lower distance traveled (thumb keys are accessible)
- Different SFB/alternation patterns
- Potentially lower overall "effort"

**What the analyzer misses:**
- Thumb keys require hardware support (split keyboard with thumb cluster) — the layout can't be used on a standard keyboard or laptop
- Learning curve is high; thumb-based placement is fundamentally different from standard finger typing
- Thumb ergonomics are not the same as finger ergonomics; thumb-heavy use has its own characteristics and some people develop discomfort with thumb-intensive layouts

The analyzer says Enthium is efficient; in practice, suitability depends entirely on your hardware and willingness to relearn from scratch. Layouts still require human judgment that analyzers can't fully replace.

## What Makes a Good Analyzer?

A more complete analyzer would:

1. **Weight metrics empirically.** Conduct controlled experiments with real typists to determine how much each metric actually affects comfort and speed.
2. **Support multiple corpora.** Let users train on their actual typing patterns rather than generic text.
3. **Account for hardware.** Allow optimization for different keyboard geometries, or even custom reach maps based on hand size.
4. **Include modifier placement.** Treat Shift, layer keys, and one-shot vs held as part of the optimization, not afterthoughts.
5. **Estimate uncertainty.** Report a confidence range around scores — "Layout A is better with ±2% uncertainty" is more useful than "Layout A is 0.5% better."
6. **Provide trade-off analysis.** Show where each layout wins and loses individually rather than combining everything into one opaque number.

No current analyzer does all of these. opt comes closest in terms of documentation depth and configurability, but it still carries many of the limitations described here.

## Conclusion: Analyzers Are Guides, Not Oracles

Layout analyzers are genuinely valuable tools for exploring the keyboard-layout space. They save massive amounts of time and reliably catch obvious problems like a layout riddled with same-finger bigrams.

But they're not oracles. They can't tell you which layout you'll actually prefer. They can't account for your specific hardware, your real typing patterns, your layer-use intensity, or your language needs. And they usually ignore layer-key ergonomics entirely.

Analyzers are one tool among several. The design principles behind a layout, its graphical finger-motion patterns, and hands-on testing with a layout simulator are all worth combining with analyzer output — future articles on this site cover each of those approaches in more depth.

For now: use analyzers to explore and understand trade-offs, look beyond the combined score to individual metrics, and then trust your hands. The best layout is the one that works best for your typing — and only you can determine that.