+++
title = "Keyboard Layout Journey"
date = 2025-11-15
taxonomies.tags = ["typing", "workflow"]
+++

A retrospection on my switch to a modified Gallium alternative keyboard layout for English and Czech typing.


## How I Got Here?

To keep it quick - Hands started hurting, got a Microsoft "ergo" keyboard. After a few years my hands still hurt, got a Moonlander and learned Colemak-DH. I could definitely see the benefits, but the productivity hit was real. At work, I still used a normie keyboard, so I switched back. Tried at least 2 more times, but it was just too much.

Earlier this year I had a few weeks to spare, so I knew it was the time to do it right.


## The Layout

From the earlier attempts, I still had some Colemak muscle memory, so that was the first candidate. However, I knew that newer and better layouts exist, such as Gallium/Graphite, Pine v4 or the various Hands Down variants. Making a custom layout was intriguing as well.

My requirements were roughly this - 70% English, 30% Czech, probably more alternating than rolling, and then basically what everyone else would want (low SFBs, low scissors, "comfort", etc.)

So naturally, I wrote my own layout generator. Well, I made it a while back, but this was a perfect excuse to get back to it. The thing is, I am an incredibly undecisive creature, so instead of figuring out the perfect algorithm and settings, I once again abandoned it.. It's in Julia (first time trying this language).

A Gallium-style layout seemed like a much better fit for my requirements than Colemak, so I tried modifying it for better Czech stats. Turns out, someone going by *Brys* on the *akl* Discord server already made a layout catered towards Czech called `corymbia-cz-eng`. Whatever I tried, my version was approaching something similar, so I kinda went with it.

The plan was to keep iterating on it, but at this point I was already practicing the homerow on keybr. What gave me some peace of mind was comparing it to Colemak or QWERTY on cyanophage.github.io using my own corpus. "Even if it's suboptimal, it will be a huge win", I thought. So I stuck with it.

Here's my version of the layout:
```
b l d w x  k p u o y
n r t s g  f h e a i
q j m c z  ' v , . ?
     spc    rep
```

Main differences compared to regular Graphite/Gallium is the `ae` column swap, `y` on pinky and `v` on the right side. One way to look at it is Gallium base + Pine v4 elements on the right side.

After nearly a year, here are my thoughts:

### The Good

- Left hand is overall fantastic, `nrts` home-row is goated. Interestingly, when I'm focusing on accuracy, I'm mentally more aware of the left hand. Possibly because the load of the left side is more spread out between all the keys.

- Alternation. This is of course a personal preference, but I definitely enjoy the rhythmic feel more than Colemak's rolling.

- Most of the qwerty left-side letters remains on the left side. I work in programs like Blender, which usually means only one hand on the keyboard and a ton of shortcuts, so keeping them on the same side is desirable. For example, Blender's GRS transforms are all on the left-side home-row. For the few important ones on the right side, I have a mirrored layer ready under one of the thumb keys.

- Repeat Key. What a fantastic concept, double letter words are now a joy. This extends to vim shortcuts like `dd` as well! I still keep noticing places where I'm not using this enough.


### The "Bad"

- In the beginning, `b` / `y` kinda bothered me, especially in Czech words like `kdyby` and `bylo`. Now I don't really mind it. The English `yi` also doesn't bother me as much as I thought it would (probably from all the years of pinky conditioning). However, the trigram `you` still feels a bit funky. Worth noting, the kb geometry could help with `b` / `y` quite a bit, I believe most boards don't have these keys enough to the side, which would be a more natural direction...

- I don't love how `p`, `k` and `f` interact with the repeat key and vowels. One possible remedy could be to move the `x` and `'` keys to a layer and physically nudge the inner columns half a key to reduce the distances to `k` and `z`. Or I could try a different index arrangement, but tbh none of those seems better. TODO repetition:

- Punctuation could be better, for example typing `o` or `y` on top row and then `.` or `?` on bottom row. Makes me wonder if the *Hands Down* solution could work better, but fixing the overloaded pinky in Czech seemed hard.

- Sometimes I'm conscious of how the pinky side mostly reaches out, while the index side curls in, creating a subtle discrepancy.

- Now bear in mind, it's easy to point out all the subtleties of a new layout and forget how much better it is compared to qwerty.


### The Unknown

- Why not a thumb alpha layout? Honesty, I'm not sure. At one point I was experimenting with a `y` thumb, which felt decent, but very wasted. Using a more typical letter like `r` would require some more involved changes (skill issue), and also I wanted to keep those on the left side (space must remain on the left side, that is the law). Having the Repeat Key in that convenient position makes me less sad about the missed opportunities.

- Magic Keys? Skill issue. Maybe I'll look into it some day, but for now I'm quite satisfied with the Repeat Key alone.

- Was the `ae` swap neccessary? What if I could fix the `y` some other way? (`y` on the left index had its own problems though)

- It's top heavy. Which seems to be a standard among alt layouts, but is that a good idea? Deep down I know I probably prefer curling. A counter-thought is that what if I ever switch to a super minimal 2-row layout, then I could basically just drop the bottom.

- Svalboard - while very expensive at the moment, it could be the future. What if the current layouts end up being suboptimal on this form factor?


> I almost envy people who write in just one language, they can pretty much go with whatever modern layout they vibe with and be done with it.


### Fun Facts

- The bottom row features `cz`, which is quite fitting for a layout decent at Czech.
- `heavy` is a "heavy" one-hand word in this layout :)


## Learning

I wish I tracked my progress better. Started on key.br to engrain the main letters to my brain, but soon I started combining it with monkeytype (I mostly knew all the positions from tweaking the layout). It was hard. 20 WPM, 30.. at 40-50 it was starting to feel like something, at 60 I was like OK, I can kinda type without feeling like a complete fool.

It's mentally taxing for a while though. Sometimes (especially under stress) it didn't matter I was at a certain WPM on monkeytype - I almost forget how to type for a second, and have to really concentrate.

On a qwerty keybord, my typing speed was 80+ WPM. Right now, I'm at 75 WPM on monkeytype quotes. Not sure if it's something to be ashamed of, but it took me 7 months of somewhat inconsistent practice to get there.

> It ain't much but it's honest work.

Also, note that the learning process itself *can* be physically painful. Of course the end goal is a more comfortable, pain-free typing, but to get there, you have to put in the hours, which can put a toll on your hands. Mentioning just in case you are switching solely because of pain - take it easy, and be prepared it will take some time (probably months). If you are in a lot of pain, maybe look into lighter switches, split keebs and tenting first.

My long term goal would be 100 WPM, but we'll see. Sometimes it's not all about the speed I suppose.


## Vroom, Vroom

On a normie qwerty keybord, my typing speed was 80+ WPM. Right now, I'm at 70+ WPM on monkeytype quotes. The obvious goal is to match my old speed but on this more comfortable layout.

Long term goal would be 100 WPM, but we'll see. Sometimes it's not all about the speed I suppose.


## Conclusion

I'm glad I switched. Typing is now really creamy and it's a decision from which I might benefit my entire life. If you are looking into a Gallium-style layout, I can recommend it. Most of the small issuess I encountered had something to do with the Czech adjustments.

<!-- All this might sound negative, but to be clear, I'm overall quite pleased! It's easy to point out all the subtleties of a new layout and forget how much better it is compared to qwerty. -->

Thanks Brys, and thanks to the AKL community for pushing this niche research forward.


Thanks for reading my 1st blog post! And remember, qwerty bad.


## Useful Links

- [layout doc v3](https://docs.google.com/document/d/1W0jhfqJI2ueJ2FNseR4YAFpNfsUM-_FlREHbpNGmC2o)
- [cyanophage.github.io](https://cyanophage.github.io/)

