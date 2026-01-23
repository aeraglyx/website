+++
title = "Filmic Curves"
date = 2026-01-30
taxonomies.tags = ["vfx", "math"]
draft = true
+++


> Color theory is a vast field, for the sake of convenience, I'll be calling "filmic" some sort of function that creates a similar look to physical film, like toe, shoulder and desaturated highlights.

## Tone Mapping

Let's start with one of the most basic tone-mappers - Reinhard:

`x / (1 + x)`

To control the amount of tone-mapping, we can modify it as such:

`x / (1 + x^(1/c))^c`

Now we get sharp clamping as `c` approaches 0, Reinhard at `c=1` and anything in between (or beyond).

We don't have unlimited precision, so low `c` together with high `x` can produce visual artifacts. At least in Blender, there seems to be a clear relationship, so we can for example give `c` a lower bound of roughly `0.00785 * log2(x)`, or set the output to 1.0 when `c` is lower than said threshold.

If we want to pin the curve at [0.5, 0.5], we can multiply the original `x` by some factor `q`:

`q = 1 / (1 - 0.5^(1/c))^c`

To figure this out, I took the inverse of the current tone-mapping function and evaluated it at 0.5, which in comparison to 0.5 tells us by how much to stretch `x`. Abstracting `q` to get a "pivot" parameter is left as an exercise for the reader.

However, this compensation might complicate reversing the final function.

## S-Curve

Next up, S-curve. I really like this function (by Luca Rood on Twitter):

`x^g / (x^g + (1 - x)^g)`

Unlike smoothstep, we can easily control the "strength" with `g`. For convenience, we can calculate it from `a` using some sigmoid function like `tan()` to compress the range:

`g = tan(0.125 * tau * (a + 1))`

Even though this is a lovely S-curve, we don't actually need it. Turns out, applying a power function before Reinhard is equivalent to Reinhard and then this S-curve! I believe it's the same thing as the Naka-Rushton and Michaelis-Menten functions.


## Highlight Desaturation

Applying tone-mapping per channel kinda sucks and you'll get The Notorious Six. A somewhat better approach is desaturating highlights first with:

`saturation = 1.0 - 0.5 ^ (luminance * strength)`


---

There is still a lot I don't know about colors, I'm not saying this is the best way, but compared to some horrendous stuff I've seen people do, it's usually good enough if you want some artistic control.


## Useful Links

- [The Hitchhiker's Guide to Digital Colour](https://hg2dc.com/)
