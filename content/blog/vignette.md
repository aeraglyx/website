+++
title = "Vignette Compensation"
date = 2026-01-05
taxonomies.tags = ["vfx", "math"]
+++


What if you don't have a reference vignette plate? No problem! If we know the sensor size and focal length (or at least the rough FOV), we have everything necessary for a decent approximation.

> We'll be talking about the soft natural vignette here, not the optical or mechanical vignette (where parts of the lens are actually obstructing the view).

---

Natural vignette is given by:

`v = cos(a)^4`

Where `a` is the angle from optical center. To calculate this angle, we first use the normalized screen coordinates `x` and `y` to make a radial gradient that goes from 0 in the center to 0.5 at the sides (taking aspect ratio into account):

`d = (x - 0.5)^2 + ((y - 0.5) * h/w)^2`

If we then multiply this gradient by sensor size, we get a physical distance from the sensor center. We can now imagine a right angle triangle where this distance is the opposite edge, focal length the adjacent edge and `a` our unknown angle:

`a = arctan(sensor_size * d / focal_length)`

The units of `sensor_size` and `focal_length` cancel out, so if we want to, we can use millimeters for both. This cancellation also means, that we don't care about the actual sensor size and focal length, more like the resulting FOV. E.g. if we don't know the sensor, but it's possible to 3D track the footage, that's also enough.

> This is all assuming rectilinear projection. So either apply the vignetting to undistorted footage, or distort the vignette.

Plug `a` into the main equation `v = cos(a)^4`, and we're done. To get rid of vignette you would divide the footage by `v`, to simulate vignette you would multiply by `v`.

> The footage needs to be linearized for this to work.

If necessary, the inner `a` can be multiplied up or down to compensate for any real world inconsistencies.

---

Hope this was useful. And please, unless you are dealing with some mechanical vignette, no more blurred ellipses from now on! Although to be fair, anamorphic lenses would produce horizontally stretched vignetting.

I might make a vignette `.fuse` someday, but for now here's at least a `CustomTool` for Fusion (just copy-paste into the node tree):

```lua
{
    Tools = ordered() {
        Vignette = Custom {
            CtrlWZoom = false,
            Inputs = {
                NumberIn1 = Input { Value = 50.0, },
                NumberIn2 = Input { Value = 36.0, },
                Intermediate1 = Input { Value = "(x-0.5)^2+((y-0.5)*h/w)^2", },
                Intermediate2 = Input { Value = "atan(n2*i1/n1)", },
                Intermediate3 = Input { Value = "cos(i2)^4", },
                RedExpression = Input { Value = "i3", },
                GreenExpression = Input { Value = "i3", },
                BlueExpression = Input { Value = "i3", },
                AlphaExpression = Input { Value = "1", },
                NameforNumber1 = Input { Value = "Focal Length", },
                NameforNumber2 = Input { Value = "Sensor Size", },
                ShowNumber3 = Input { Value = 0, },
                ShowNumber4 = Input { Value = 0, },
                ShowNumber5 = Input { Value = 0, },
                ShowNumber6 = Input { Value = 0, },
                ShowNumber7 = Input { Value = 0, },
                ShowNumber8 = Input { Value = 0, },
                ShowPoint1 = Input { Value = 0, },
                ShowPoint2 = Input { Value = 0, },
                ShowPoint3 = Input { Value = 0, },
                ShowPoint4 = Input { Value = 0, },
            },
            ViewInfo = OperatorInfo { Pos = { 0, 0 } },
        }
    },
    ActiveTool = "Vignette"
}
```

> Fusion expects degrees instead of radians for trigonometric functions, but since we just pass the angle from `atan()` to `cos()`, we don't have to worry about that.
