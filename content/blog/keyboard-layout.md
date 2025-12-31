+++
title = "Keyboard Layout Journey"
date = 2025-11-11
taxonomies.tags = ["typing", "workflow"]
+++


A retrospection on my switch to a modified Gallium alternative keyboard layout for English and Czech typing.


## How I Got Here?

A while back, my wrists started hurting, so I got a Microsoft "ergo" keyboard. After a few years of basically no improvement, I got a Moonlander and learned Colemak-DH. I could definitely see the benefits, but the productivity hit was real (especially working in Blender). I tried at least 2 more times, but it was just too much, so I went back to a normie keyboard and QWERTY :(

Earlier this year I had a few weeks to spare, so I knew it was the time to do it right.


## The Layout

From the earlier attempts, I still had some Colemak muscle memory, so that was the first candidate. However, I knew that newer and better layouts exist, such as Gallium/Graphite, Pine v4 or the various Hands Down variants. Making a custom layout was intriguing as well.

My requirements were roughly this - 70% English, 30% Czech, probably more alternating than rolling, and then basically what everyone else would want (low SFBs, low scissors, "comfort", etc.)

So naturally, I wrote my own [layout generator](https://github.com/aeraglyx/whelk) - only to abandon it again. Originally I wanted to make a layout without the inner index keys (kinda like engram) but it would be difficult to nail in the first try. I do want to "finish" it someday, however. It's in Julia (first time trying this language).

A Gallium-style layout seemed like a better fit for my requirements than Colemak, so I tried modifying it for better Czech stats. Turns out, someone going by *Brys* on the *akl* Discord server already made a layout catered towards Czech called `corymbia-cz-eng`. Whatever I tried, my version was approaching something similar, so I kinda went with it.

The plan was to keep iterating on it, but at this point I was already practicing the homerow on [keybr](https://www.keybr.com/). What gave me some peace of mind was comparing it to Colemak or QWERTY on [cyanophage](https://cyanophage.github.io/) using my own generated corpus. "Even if there was something better, it will be a huge win", I thought. So I stuck with it.

Here's my version of the layout ([QMK keymap here](https://github.com/aeraglyx/qmk_userspace/blob/main/keyboards/grooovebob/dasbob/keymaps/aeraglyx/keymap.c)):
```
b l d w x  k p u o y
n r t s g  f h e a i
q j m c z  ' v , . ?
     spc    rep
```

Main differences compared to regular Graphite/Gallium consist of the `ae` column swap, `y` on pinky and `v` on the right side. One way to look at it is a Gallium base + some Pine v4 elements on the right side.

Over half a year later, here are my thoughts:


### The Good

- Left hand is overall fantastic, `nrts` home-row is goated. Interestingly, when I'm focusing on accuracy, I'm mentally more aware of the left hand. Possibly because the load of the left side is more spread out between all the keys.

- Alternation. This is of course a personal preference, but I definitely enjoy the rhythmic feel more than Colemak's rolling.

- Most of the QWERTY left-side letters remains on the left side. I work in programs like Blender, which usually means only one hand on the keyboard and a ton of shortcuts, so it's desirable to keep them on the same side. For example, Blender's GRS transforms are all on the left-side home-row. For the few important ones on the right side, I have a mirrored layer under one of the thumb keys.

- Repeat Key. What a fantastic concept, double letter words are now a joy. This extends to vim shortcuts like `dd` as well! I still keep noticing places where I'm not using this enough.


### The Bad

- In the beginning, `b` / `y` kinda bothered me, especially in Czech words like `kdyby` and `bylo`. Now I don't really mind it. The English `yi` also doesn't bother me as much as I thought it would (probably from all the years of pinky conditioning). However, the trigram `you` still feels a bit funky. Worth noting, the kb geometry could help with top row pinkies quite a bit, I believe most boards don't have these keys enough to the side, which would be a more natural direction...

- I don't love how `p`, `k` and `f` interact with the repeat key and vowels. One possible remedy could be to move the `x` and `'` keys to a layer and physically nudge the inner columns half a key to reduce the distances to `k` and `z`. Or I could try a different index arrangement, but tbh none of those seem great.

- Punctuation could be better, for example typing `o` or `y` on top row and then `.` or `?` on bottom row. Makes me wonder if the *Hands Down* solution could work better, but fixing the overloaded pinky in Czech seemed hard.

- Sometimes I'm conscious of how the pinky side mostly reaches out, while the index side curls in, creating a subtle discrepancy.

But just to be clear, it's easy to point out all the subtleties of a new layout and forget how much better it is compared to QWERTY.


### The Unknown

- Why not a thumb alpha layout? At one point I was experimenting with a `y` thumb, which felt decent, but a bit wasted. Using a more typical letter like `r` would require some more involved changes (skill issue), and I'd rather keep `r` on the left side (with space on the left side, that is the law). Having a Repeat Key in that convenient position makes me a bit less sad about the missed opportunities.

- Magic Keys? Skill issue. Maybe I'll look into it some day, but for now I'm quite satisfied with the Repeat Key alone.

- Was the `ae` column swap necessary? What if I could fix `y` some other way? (`y` on the left index had its own problems though)

- It's top heavy. Which seems to be a standard among alt layouts, but is that a good idea? In theory, curling might be a bit more natural?

- Svalboard - while pretty expensive at the moment, it could be the future. What if the current layouts end up being suboptimal on this form factor? Here's [Ben Vallack's video](https://www.youtube.com/watch?v=-Lz_FNoYHNM) about it.


### Fun Facts

- The bottom row features `cz`, which is quite fitting 🇨🇿
- `heavy` is a "heavy" one-handed word on this layout :)


## Learning

I wish I tracked my progress better. Started on [keybr](https://www.keybr.com/) to engrain the main letters to my brain, but soon I transitioned to [monkeytype](https://monkeytype.com/) (I already knew most of the positions from tweaking the layout). It was hard. 20 WPM, 30.. at 40-50 it was starting to feel like something, at 60 I was like OK, I can kinda type without feeling like a complete fool.

It's mentally taxing for a while though. Sometimes (especially under stress) it didn't matter I was at a certain WPM on monkeytype - I almost forget how to type for a second, and have to really concentrate.

On a QWERTY keyboard, my typing speed was 80+ WPM. At the time of writing, I'm at 70+ WPM on monkeytype quotes. Not sure if it's something to be ashamed of, but it took me more than half a year to get there.

> It ain't much but it's honest work.

The obvious goal is to match my old speed but with less pain.

Also, note that the learning process itself *can* be physically painful. Of course the end goal is a more comfortable, pain-free typing, but to get there, you have to put in the hours, which can put a toll on your hands. Mentioning just in case you are switching solely because of pain - take it easy, and be prepared it will take some time (probably months). If you are in a lot of pain, maybe look into lighter switches etc. first.


## Conclusion

I'm glad I switched. Typing is now really creamy and it's a decision from which I'll benefit my entire life. If you are looking into a Gallium-style layout, I can recommend it. Most of the small issues I encountered had something to do with the Czech adjustments, and even then, it seems to perform quite well in both languages.

Thanks Brys and the AKL community for pushing this niche research forward and thank you for reading my 1st blog post!

And remember, QWERTY bad! 🟡


## Useful Links

- [cyanophage.github.io](https://cyanophage.github.io/)
- [the layout doc v3](https://docs.google.com/document/d/1W0jhfqJI2ueJ2FNseR4YAFpNfsUM-_FlREHbpNGmC2o)
- [monkeytype.com](https://monkeytype.com/)
- [keybr.com](https://www.keybr.com/)
- [getreuer.info](https://getreuer.info/posts/keyboards/alt-layouts/index.html)
