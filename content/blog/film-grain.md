+++
title = "Film Grain"
date = 2026-02-06
taxonomies.tags = ["vfx", "math"]
+++


It's relatively easy to mimic film grain using Voronoi [as demonstrated here](https://www.youtube.com/watch?v=OXHyQltrkZo), but that method is relying on the fact that a ray-tracer is averaging sampled values inside a pixel. And since it's typical to have many grains per pixel, I was wondering if there is a way to approximate this behavior in the compositor without any Monte Carlo shenanigans while preserving most of the nice characteristics.

## Overview

In theory, this is a solved problem, we could just sample from the Beta Distribution. It's not a straight forward function though. I tried a few approximations for its Inverse CDF (Wilson-Hilferty, Cornish-Fisher, Peizer-Pratt) but none of them were perfect.

So my thought was to start with a roughly normally distributed noise with mean at 0.5 and apply a power-like function to the noise so that the average output matches the input. There were a few challenges, but in the end I'm quite pleased with the result.

<!-- We could use whatever bell distribution with a decent [quantile function](https://en.wikipedia.org/wiki/Quantile_function) (inverse CDF), but for many reasons, the [Normal Distribution](https://en.wikipedia.org/wiki/Normal_distribution) seems like the obvious candidate. Here's its quantile function: -->
<!-- We'll use the [Hann function](https://en.wikipedia.org/wiki/Hann_function) - a nice, bounded bell distribution. Let's transform the uniform distribution using the Hann [CDF](https://en.wikipedia.org/wiki/Cumulative_distribution_function): -->

<!-- `0.5 + σ * sqrt(2) * inverf(2 * x - 1)` -->

## The Method

We could choose whatever bell distribution such as the [Normal Distribution](https://en.wikipedia.org/wiki/Normal_distribution), but in the end I settled for a distribution with this inverse CDF:

`x ^ g / (x ^ g + (1 - x) ^ g)`

Its PDF is unfortunately a bit different to a normal distribution, but at least it's easily computable and bound to 0-1. To more or less match the corresponding standard deviation, pass `2 * σ` into `g` (valid for small `σ`). This will control the final grain strength (more on this later).

<!-- If you chose a different bell curve, like the [Hann function](https://en.wikipedia.org/wiki/Hann_function), the inverse CDF would look like this (note the `0.1808` to normalize the standard deviation): -->

<!-- TODO: is the correction correct here? -->

<!-- `0.5 + asin(0.1808 * 2 * (x - 0.5) / σ) / π` -->

<!-- After applying this to a white noise, there will be more values at the center and less at the tails. We can scale this using the standard deviation `σ` to achieve different grain strengths (more on this later). -->

Moving the noise to the desired value while keeping it bound to 0-1 might sound like the perfect job for a power function, but it behaves differently near 0 and 1, so I chose this function instead:

`f_power(x, g) = x * g / (x * (g - 1) + 1)`

You might imagine choosing some `g` such that after applying the function to 0.5, the output will match the desired value. That's not difficult to solve, but we cannot assume that after bending the 0.5-centered noise to the desired value, the mean will also become that value.

<!-- To see what "power" we actually need for any `x` and `σ`, we can integrate the function below from 0 to 1 and invert it (setting g as the y-axis, x will be the desired mean): -->

<!-- `window_pdf(x, σ) * f_power(x, 1/g)` -->

<!-- For `σ=0`, we get `(1 - x) / x`, exactly what we would get by solving for `g` naively, which makes sense since all the samples equal the mean. For larger `σ` it's a bit different (definitely enough to notice), perhaps even without a closed form. I have approximated it by gradually exponentiating the simple form with some number based on `σ`: -->

For `σ=0`, we get `(1 - x) / x`, but for larger `σ` it's different, possibly without a closed form. I have approximated it by gradually exponentiating the simple form with some number based on `σ`:

`1 / g = ((1 - x) / x ) ^ func(σ)`

<!-- `((1 - x) / x ) ^ (1 + 0.46 * response(A * σ, B))` -->

<!-- The vaguely named `response()` is a Naka-Rushton function `x^p/(1+x^p)`, which is roughly what the relationship looks like. For the normal distribution, the coefficients `A` and `B` were professionally eye-balled as `A = 3.8` and `B = 2.6`. -->

<!-- Different distributions yield different coefficients, e.g. 4.2 and 3.1 for Hann (scaled so the std dev matches normal). -->

<!-- | Distribution | A | B | -->
<!-- | ------------ | - | - | -->
<!-- | Normal   | 3.8 | 2.6 | -->
<!-- | Hann     | 4.2 | 3.1 | -->
<!-- | Logistic | 3.7 | 2.2 | -->
<!-- | Welch    | 2.6 | 2.0 | -->

I put way too much effort into estimating the ideal power, but in the end I simply eye-balled good `func(σ)` values for various strengths that would work well across the input values, plotted it, and approximated it as:

`func(σ) = sqrt(4 * σ^2 + 0.25 * σ + 1)`

After plugging both the 0.5-centered noise and the reciprocal of `g` into `f_power()`, we get a pretty convincing film grain.

> For typical strengths there's virtually no change in the overall luminance, but do note that this method gets increasingly less accurate with large standard deviations (like `σ > 0.5`).

## Calculating the Standard Deviation

<!-- When sampling a large area of random boolean cells, the standard deviation should be roughly `0.5 * d`, where `d` is the grain size in pixels. To get grain size in pixels from the physical grain size: -->

Thanks to [Bernoulli Trial](https://en.wikipedia.org/wiki/Bernoulli_trial), when sampling a large area of random boolean cells, the standard deviation should be roughly `0.5 * d`, where `d` is the grain size in pixels. The final standard deviation will be:

`σ = 0.5 * grain_size_m * resolution_x / sensor_size`

Then we could either plug in the actual grain size (e.g. 1 µm), or approximate it from the ISO. In general, more sensitive film will have larger grains. To get the grain size in meters:

`0.05 * sqrt(ISO) * 1e-6`

Plug the standard deviation into the film grain and we are done.
