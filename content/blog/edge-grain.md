+++
title = "Edge Grain"
date = 2025-11-11
taxonomies.tags = ["vfx", "math"]
+++


When merging a grain-free element on top of a grainy footage, we match the grain and that's it, right? Well not quite, by mixing 2 uncorrelated grains, we get weaker grain in the edges. Let's figure out why and how to fix this.

When we blend multiple noisy signals, the overall noise to signal ratio changes with `1 / sqrt(n)`, so for 2 signals, that is roughly 0.707. More generally, when combining two signals with different noise strengths, the resulting noise is:

`sqrt(s1 ^ 2 + s2 ^ 2)`

When linearly interpolating the 2 images, s1 represents the fg alpha, s2 its inverse (the remaining bg grain after the merge). So if we plug in `x` for s1 (the original alpha), `1-x` for s2 and plot it, we get a U-curve that showcases the problem.

<!-- TODO: plot img -->

But what if we modify `x` in some way that cancels out the U-curve and gives us a flat response? Let's call this modified alpha `x'`.

If we say that s1 is the desired strength based on the original alpha, s2 the inverted alpha and we set it to 1 (representing constant NSR), we get:

`1 = sqrt(x' ^ 2 + (1 - x) ^ 2)`

Solving for `x'`:

`x' = sqrt(x * (2 - x))`

Which is a circle! I just love these kinds of problems.

> So in practice, the solution is to add a grain node after the merge, and for the mask use the former function, where `x` is the fg alpha.

<!-- TODO:  add before/after img-->

If we want to apply the grain before the premult and merge, we simply divide it by the alpha to get:

`sqrt(x * (2 - x)) / x`

However, note that this form shoots off to infinity as it approaches `x=0`.
