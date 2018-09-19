+++
# Date this page was created.
date = 2016-04-27T00:00:00

# Project title.
title = "Creating simple filters for image processing"

# Project summary to display on homepage.
summary = "TODO"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "bubbles.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["image-processing"]


# Does the project detail page use math formatting?
math = true

highlight = true

# Optional featured image (relative to `static/img/` folder).
[header]
image = "headers/bubbles-wide.jpg"
caption = "My caption :smile:"

+++

## Low pass filters
Three commonly used low pass filters are:

1. The ideal low pass filter.
2. The butterworth low pass filter.
3. The gaussian low pass filter.


Their mathematical notations where 

* $D =$ Distance of point $(n_1, n_2)$ from the center of the image.
* $D_0, σ =$ Constants that define the "radius" of the low pass filter.

are:


| Filter            | Function                                                                                  |
| ------------------| ----------------------------------------------------------------------------------------- |
| `Ideal`           | $f(n_1,n_2) = \begin{cases} 1 & \text{if }D\leq D_0, \\\\0 & \text{if }D>D_0.\end{cases}$ |
| `Butterworth`     | $f(n_1,n_2) = \frac{1}{1+(\frac{D}{D_0})^n}$    |
| `Gaussian`        | $f(n_1,n_2) = \exp(-\frac{D^2}{2σ^2})$          |

The ideal filter in matlab will be:
```
function filtOut = myLowPassIdeal(cutoff, M)
%MYLOWPASSIDEAL Creates an ideal low pass filter (mxm)
%   Arguments:
%       cutoff  : the cutoff frequency
%       M       : the dimension of the square filter
%   Returns:
%       filtOut : the filter on the frequency domain

% center point in the middle
center = M / 2;

% ideal low pass filter
filtOut = zeros(M);
for i = 1 : M
    for j = 1 : M
        if (i-center)^2 + (j-center)^2 <= cutoff^2
            filtOut(i, j) = 1;
        end
    end
end


end
```