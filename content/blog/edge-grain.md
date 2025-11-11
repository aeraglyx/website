+++
title = "Edge Grain"
date = 2025-11-27
taxonomies.tags = ["vfx", "math"]
+++

When merging a grain-free element on top of a grainy footage, we match the grain and that's it, right? Not quite, by default you'll get weaker grain in the edges. Let's figure out why and how to fix this.

When we blend multiple noisy signals, the overall noise to signal ratio (NSR) changes with `1 / sqrt(n)`, so for 2 signals, that is roughly 0.707. More generally, when combining two signals with different noise strengths (which is essentially what blending 2 noisy images is), the noise is:

`sqrt(s1 ^ 2 + s2 ^ 2)`

If we say that s1 is the desired strength based on the original alpha, s2 the inverted alpha (noise that remains in the bg after the merge) and we set it to 1 (representing constant SNR), we get:

`1 = sqrt(a1 ^ 2 + (1 - a) ^ 2)`

Solving for a1:

`a1 = sqrt(a * (2 - a))`

Which is a circle! I just love these kinds of problems. So one solution is to add a grain node after merging the two images, and for the mask use the former function, where `a` is the fg alpha.

<!-- TODO:  add before/after img-->

If we want to apply the grain before the premult and merge, we simply divide it by the alpha to get:

`sqrt(a * (2 - a)) / a`

However, note that this form shoots off to infinity as it approaches a=0, which could cause problems.
