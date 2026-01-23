+++
title = "Filmic Curves"
date = 2026-02-11
taxonomies.tags = ["vfx", "math"]
draft = true
+++


Set of functions for tone-mapping, contrast and highlight rolloff to more or less imitate the "film look". We'll be both expecting and outputting "linear light", you or your DCC software should apply the ODTF as usual.


## Tone Mapping

Let's start with one of the most basic tone-mappers - Reinhard:

`x / (1 + x)`

To control the amount of tone-mapping, we can modify it as such:

`x / (1 + x ^ (1 / c)) ^ c`

Now we get sharp clamping as `c` approaches 0, Reinhard at `c=1` and anything in between (or beyond).

We don't have unlimited precision, so low `c` together with high `x` can produce visual artifacts. At least in Blender, there seems to be a clear relationship, so we can for example give `c` a lower bound of roughly `0.00785 * log2(x)`, or set the output to 1.0 when `c` is lower than the said threshold.

If we want to pin the curve at [0.5, 0.5], we can multiply the original `x` by some factor `q`:

`q = 1 / (1 - 0.5 ^ (1 / c)) ^ c`

To figure this out, I took the inverse of the current tone-mapping function and evaluated it at 0.5, which in comparison to 0.5 tells us by how much to stretch `x`. Abstracting `q` to get a "pivot" parameter is left as an exercise for the reader.

However, this compensation might complicate reversing the final function, if that's something you require. Perhaps the pinning is not worth it.


## Contrast

You could think of contrast as an S-curve. I really like this well-behaved function (first saw it mentioned by Luca Rood on Twitter):

`x^g / (x^g + (1 - x)^g)`

Unlike smoothstep, we can easily control the "strength" with `g`. For convenience, we can calculate it from `a` using some inverse sigmoid function like `tan()` to compress the range:

`g = tan(0.125 * tau * (a + 1))`

Even though this is a lovely S-curve, we don't actually need it. Turns out, applying a power function before Reinhard is equivalent to Reinhard and then this S-curve! I believe it's the same thing as the Naka-Rushton and Michaelis-Menten functions.

To set some pivot when pre-applying the power function (to avoid darkening the range 0-1 when doing `x^g`), you can modify it as:

`p * (x / p) ^ g`

You can even set different powers per channel (for example based on some temp/tint) to easily achieve effects like the *Teal and Orange* look.


## Highlight Falloff

Applying the above per channel kinda sucks and you'll get the *Notorious Six*. A somewhat better approach is desaturating the highlights first with:

`saturation = 1.0 - 0.5 ^ (luminance * strength)`


## Blackpoint and Whitepoint

Probably not even worth mentioning, but you can remap the 0-1 channel outputs to some other range to for example make highlights approach yellow instead of white, lift the blacks etc.


## Final Notes

There is still a lot more to be explored. I'm not saying this is the best way, it's certainly not very scientific (you might want to look into AgX for that), but compared to some horrendous stuff I've seen people do, it's usually good enough if you just need some artistic control.


## Useful Links

- [The Hitchhiker's Guide to Digital Colour](https://hg2dc.com/)
