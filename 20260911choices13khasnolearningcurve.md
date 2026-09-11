---
layout: default
title: "choices13k Has No Learning Curve"
description: "The Block column looks like an experience trajectory. It isn't one."
date: 2026-09-11 09:00:00 +0800
---

# choices13k Has No Learning Curve

*The dataset ships a column called `Block` that runs from 1 to 5. It looks like an
experience trajectory. It isn't one — and the difference matters if you're modelling
learning, adaptation, or anything that accumulates over trials.*

**Pulakesh Upadhyaya** · 11 September 2026 · ~6 min

---

choices13k is the largest public dataset of human decisions under risk — 13,006 gamble
pairs, roughly 16 participants each, collected by Peterson, Bourgin, Agrawal, Reichman and
Griffiths for their 2021 *Science* paper. It has become a default benchmark for models of
risky choice, and it is a genuinely excellent resource.

It also contains a trap I walked straight into, and I suspect I'm not the only one. This is
a short note on what the `Block` column is, what it is not, and how to check the difference
in about ten lines of code.

> Among feedback problems, `Block` is assigned **uniformly at random** from {2, 3, 4, 5}.
> It is a label, not a position in a learning sequence. Participants did not accumulate
> experience across it, and the data confirm they didn't.

## 1. Why the column looks like a trajectory

The trap is inherited, not invented. choices13k follows the presentation format of the 2015
and 2018 Choice Prediction Competitions, and in *those* designs the block parameter meant
exactly what you'd expect: participants played five successive blocks of five trials with
feedback, and the block index recorded how much experience they had accumulated. Block 5
really was later than block 2.

choices13k reduced that to two blocks — one without feedback, one with. But the column name
and its 1-to-5 range survived the reduction. So you get a variable called `Block`, spanning
the same values it spans in the parent literature, that no longer carries the same meaning.

To the dataset's credit this is documented. The README states plainly that for problems with
no feedback `Block` is always 1, and otherwise it "was sampled uniformly at random from
{2, 3, 4, 5}." The information is right there. It's just easy to read past when the column
has a familiar name and your analysis wants a time axis.

## 2. What the data say

Random assignment has one happy consequence: it makes the check clean. If `Block` carried
experiential content, later blocks should show better choices. Because assignment is random,
any difference would be causal. So just look.

The natural outcome measure is accuracy: the rate at which participants pick the gamble with
the higher expected value.

<svg viewBox="0 0 640 352" role="img" style="width:100%;height:auto;max-width:660px;font-family:ui-monospace,SFMono-Regular,Menlo,monospace" aria-label="Choice accuracy by block. Blocks 2 through 5 sit flat at about 0.63 with overlapping confidence intervals, while the no-feedback baseline sits distinctly lower at 0.607.">
  <g stroke="#D4DCDE" stroke-width="1">
    <line x1="92" y1="260" x2="600" y2="260"/>
    <line x1="92" y1="205" x2="600" y2="205"/>
    <line x1="92" y1="150" x2="600" y2="150"/>
    <line x1="92" y1="95" x2="600" y2="95"/>
    <line x1="92" y1="40" x2="600" y2="40"/>
  </g>
  <g fill="#78868B" font-size="11" text-anchor="end">
    <text x="82" y="264">0.58</text>
    <text x="82" y="209">0.60</text>
    <text x="82" y="154">0.62</text>
    <text x="82" y="99">0.64</text>
    <text x="82" y="44">0.66</text>
  </g>
  <line x1="92" y1="186.6" x2="600" y2="186.6" stroke="#B07A17" stroke-width="2" stroke-dasharray="6 4"/>
  <circle cx="92" cy="186.6" r="5" fill="#B07A17"/>
  <text x="104" y="178" fill="#B07A17" font-size="11.5" font-weight="500">No feedback (Block 1) &#183; 0.607</text>
  <g stroke="#47555A" stroke-width="1" fill="none">
    <path d="M614 186.6 L622 186.6 L622 121.6 L614 121.6"/>
  </g>
  <text x="627" y="154" fill="#47555A" font-size="10.5" transform="rotate(90 627 154)" text-anchor="middle">+0.024</text>
  <g>
    <line x1="165" y1="99.4" x2="165" y2="135.7" stroke="#12605F" stroke-width="2"/>
    <line x1="158" y1="99.4" x2="172" y2="99.4" stroke="#12605F" stroke-width="2"/>
    <line x1="158" y1="135.7" x2="172" y2="135.7" stroke="#12605F" stroke-width="2"/>
    <circle cx="165" cy="117.6" r="6" fill="#12605F" stroke="#FFFFFF" stroke-width="2"/>
    <text x="165" y="84" fill="#101619" font-size="11.5" text-anchor="middle">.6318</text>

    <line x1="293" y1="112.6" x2="293" y2="148.4" stroke="#12605F" stroke-width="2"/>
    <line x1="286" y1="112.6" x2="300" y2="112.6" stroke="#12605F" stroke-width="2"/>
    <line x1="286" y1="148.4" x2="300" y2="148.4" stroke="#12605F" stroke-width="2"/>
    <circle cx="293" cy="130.5" r="6" fill="#12605F" stroke="#FFFFFF" stroke-width="2"/>
    <text x="293" y="97" fill="#101619" font-size="11.5" text-anchor="middle">.6271</text>

    <line x1="421" y1="101.0" x2="421" y2="136.8" stroke="#12605F" stroke-width="2"/>
    <line x1="414" y1="101.0" x2="428" y2="101.0" stroke="#12605F" stroke-width="2"/>
    <line x1="414" y1="136.8" x2="428" y2="136.8" stroke="#12605F" stroke-width="2"/>
    <circle cx="421" cy="118.9" r="6" fill="#12605F" stroke="#FFFFFF" stroke-width="2"/>
    <text x="421" y="85" fill="#101619" font-size="11.5" text-anchor="middle">.6313</text>

    <line x1="549" y1="102.4" x2="549" y2="137.1" stroke="#12605F" stroke-width="2"/>
    <line x1="542" y1="102.4" x2="556" y2="102.4" stroke="#12605F" stroke-width="2"/>
    <line x1="542" y1="137.1" x2="556" y2="137.1" stroke="#12605F" stroke-width="2"/>
    <circle cx="549" cy="119.8" r="6" fill="#12605F" stroke="#FFFFFF" stroke-width="2"/>
    <text x="549" y="86" fill="#101619" font-size="11.5" text-anchor="middle">.6310</text>
  </g>
  <line x1="92" y1="260" x2="600" y2="260" stroke="#B9C5C8" stroke-width="1"/>
  <g fill="#47555A" font-size="11.5" text-anchor="middle">
    <text x="165" y="283">Block 2</text>
    <text x="293" y="283">Block 3</text>
    <text x="421" y="283">Block 4</text>
    <text x="549" y="283">Block 5</text>
  </g>
  <text x="92" y="312" fill="#78868B" font-size="10.5">linear trend +0.00019 per block, 95% CI [&#8722;0.0027, +0.0031], p = 0.895 &#183; ANOVA F = 0.43, p = 0.730</text>
  <text x="92" y="330" fill="#78868B" font-size="10.5">y-axis truncated to 0.58&#8211;0.66, which exaggerates any block difference rather than hiding it</text>
</svg>

*P(choose higher-EV gamble), 95% CI, n = 12,188 feedback problems. The four feedback blocks
are indistinguishable. The one thing that does move is the presence of feedback at all: the
no-feedback baseline sits 0.024 below the feedback cluster, roughly five times the widest
block-to-block difference of 0.005.*

| Condition | n problems | P(higher EV) | 95% CI | Mean bRate |
|---|---:|---:|---:|---:|
| **No feedback** | 2,380 | 0.6067 | ± 0.0075 | 0.5170 |
| Block 2 | 3,006 | 0.6318 | ± 0.0066 | 0.5115 |
| Block 3 | 3,039 | 0.6271 | ± 0.0065 | 0.5186 |
| Block 4 | 3,077 | 0.6313 | ± 0.0065 | 0.5220 |
| Block 5 | 3,066 | 0.6310 | ± 0.0063 | 0.5232 |

## 3. How flat is flat?

"No effect" is only a claim if you say how large an effect you could have detected. The
linear trend in accuracy is +0.00019 per block with a 95% interval of [−0.0027, +0.0031].
Taking the top of that interval and running it across the full span from block 2 to block 5
gives a ceiling of about 0.009.

Set that against the effect that *is* in the data. Comparing the same problem with and
without feedback — 1,562 problems appear in both conditions — gives a gain of +0.029. So the
largest block drift the data can hide is under a third of the description-to-experience
effect measured on the very same problems.

| Quantity | Value |
|---|---:|
| Accuracy trend per block | **+0.0002** (p = 0.895) |
| Largest drift compatible with the data, blocks 2→5 | **0.009** |
| Within-problem description → experience effect | **+0.029** |
| Ratio of the first to the second | **0.32×** |

Choice extremity tells the same story: the trend in \|bRate − 0.5\| is −0.00098, p = 0.338.
A one-way ANOVA across the four blocks on accuracy gives F = 0.43, p = 0.730. There is
nothing there.

> **One honest caveat.** Raw `bRate` — the unsigned rate of choosing gamble B — does show a
> weak upward trend across blocks: +0.0038 per block, p = 0.035. It is one marginal result
> among three tests, it does not survive as accuracy or extremity, and gamble B is an
> arbitrary label rather than a normatively better option, so the direction carries no
> interpretation. I mention it because I'd want to know about it, not because I think it's
> real.

## 4. What to do instead

None of this makes choices13k less useful. It makes one specific design unavailable, and
points at the one that works.

1. **Don't** treat `Block` as time, trial count, or accumulated experience. Don't fit
   learning curves over it, and don't use it as an ordinal predictor in a model of
   adaptation.
2. **Do** use the feedback contrast instead. 1,562 problems appear in both the no-feedback
   and feedback conditions, which gives a clean within-problem description-to-experience
   comparison with every problem characteristic held fixed.
3. **Do** pool blocks 2–5 when you want the feedback condition. Since assignment is random
   and the outcome is flat, pooling costs you nothing and quadruples your cell sizes.
4. **Go elsewhere** for genuine trial-level dynamics. CPC18 publishes raw data with real
   within-subject block sequences. choices13k aggregates over roughly 16 participants × 5
   trials, so even subject-level heterogeneity in learning rate is averaged away before you
   see it.

## 5. Check it yourself

The whole thing is one groupby. If you're using choices13k for anything that accumulates
over trials, this is worth thirty seconds.

```python
import pandas as pd, scipy.stats as st

d  = pd.read_csv("c13k_selections.csv")
fb = d[d.Feedback]

# bRate is the rate of choosing gamble B; flip it where A is the higher-EV option
# (evA / evB computed from c13k_problems.json)
print(fb.groupby("Block").pmax.agg(["mean", "sem", "size"]))

r = st.linregress(fb.Block, fb.pmax)
print(r.slope, r.slope - 1.96*r.stderr, r.slope + 1.96*r.stderr, r.pvalue)
# +0.00019   -0.00270   +0.00309   0.895
```

---

**Data.** choices13k, Peterson, Bourgin, Agrawal, Reichman & Griffiths, *Science* 372(6547),
2021. The parent design is Erev, Ert, Plonsky, Cohen & Cohen, *Psychological Review* 124(4),
2017. Both datasets are public.

This came out of a larger project testing whether efficient coding predicts how risk
attitudes drift as people re-adapt to a changing payoff environment. I had built that test on
the block structure before checking whether the block structure was real. It wasn't, so here
is the check, separated out.
