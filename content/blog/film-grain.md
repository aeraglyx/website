+++
title = "Film Grain Approximation"
date = 2026-02-07
taxonomies.tags = ["vfx", "math"]
+++


It's relatively easy to mimic film grain using Voronoi [as demonstrated here](https://www.youtube.com/watch?v=OXHyQltrkZo), but that method is relying on the fact that a ray-tracer is averaging sampled values inside a pixel. And since it's typical to have many grains per pixel, I was wondering if there is a way to approximate this behavior in the compositor without any Monte Carlo shenanigans while preserving most of the nice characteristics.


## Overview

In theory, this is a solved problem, we could just sample the Beta Distribution. It's not a straight forward function though, I tried a few approximations for its Inverse CDF (Wilson-Hilferty, Cornish-Fisher, Peizer-Pratt) but none of them were perfect.

So my thought was to start with a white noise, convert it to a roughly normally distributed 0.5-centered noise with some variance to set the grain strength, and finally apply a power-like function to the noise so that the average output matches the input.


## The Method

You could start with the [Normal Distribution](https://en.wikipedia.org/wiki/Normal_distribution), but since its [Inverse CDF](https://en.wikipedia.org/wiki/Quantile_function) is unbound, you would get disproportionally many "fireflies" and it will be impossible to recover some values near the bounds, so it might be wise to pass it through some sigmoid like `tanh()`. Soft-clamped inverse CDF:

`0.5 + 0.5 * tanh(2 * σ * sqrt(2) * inverf(2 * U - 1) - 1)`

Where `U` is uniformly distributed noise in the range 0-1. In the end however I settled for a distribution with this inverse CDF:

`U ^ s / (U ^ s + (1 - U) ^ s)`

Sadly it's a bit different to normal distribution, but at least it's easily computable and already bound to 0-1. To more or less match the corresponding standard deviation, pass `2 * σ` into `s` (valid for small `σ`). This will control the final grain strength (more on this later).

Moving the noise to the desired value while keeping it bound to 0-1 might sound like the perfect job for a power function, but it behaves differently near 0 and 1, so instead I chose this:

`f_power(x, g) = x * g / (x * (g - 1) + 1)`

You might imagine choosing some `g` such that after applying the function to 0.5, the output will match the desired value. That's not difficult to solve, but we cannot assume that after bending the 0.5-centered noise to the desired value, the final mean will also become that value.

For `σ=0`, we get `x / (1 - x)`, but for larger `σ` it's different, possibly without a closed form. I have approximated it by gradually exponentiating the simple form with some number based on `σ`:

`g = (x / (1 - x)) ^ get_pow(σ)`

I put way too much effort into estimating the ideal power, but in the end I simply eye-balled good `get_pow()` values for various strengths that would work well across the input values, plotted it, and approximated it as:

`get_pow(σ) = sqrt(4 * σ^2 + 0.25 * σ + 1)`

This is dependent on the distribution used. At extreme powers, the noise loses some apparent strength, so to roughly compensate, I multiplied `σ` by:

`1 + 3 * (x - 0.5) ^ 2`

Now after plugging both the 0.5-centered noise and the power `g` into `f_power()`, we get a pretty convincing film grain.

> For typical strengths there's virtually no change in the overall luminance, but do note that this method gets increasingly less accurate with large standard deviations (like `σ > 0.5`).


## Calculating the Standard Deviation

Thanks to [Bernoulli Trial](https://en.wikipedia.org/wiki/Bernoulli_trial), when sampling a large area of random boolean cells, the standard deviation should be roughly `0.5 * d`, where `d` is the grain size in pixels. The final standard deviation will be:

`σ = 0.5 * grain_size_m * resolution_x / sensor_size`

Then we could either plug in the actual grain size (e.g. 1 µm), or approximate it from the ISO. In general, more sensitive film will have larger grains. To get the grain size in meters:

`0.05 * sqrt(ISO) * 1e-6`

Plug the standard deviation into the film grain and we are done.
