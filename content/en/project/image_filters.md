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


Their mathematical notations are: 


| Filter          | Function                                                                                  |
| ----------------| ----------------------------------------------------------------------------------------- |
| Ideal           | $f(n_1,n_2) = \begin{cases} 1 & \text{if }D\leq D_0, \\\\0 & \text{if }D>D_0.\end{cases}$ |
| Butterworth     | $f(n_1,n_2) = \frac{1}{1+(\frac{D}{D_0})^{2n}}$    |
| Gaussian        | $f(n_1,n_2) = \exp(-\frac{D^2}{2D_0^2})$          |

* $n_1, n_2 = 1, ..., M$ are the indices of the filter table's values.
* $D =$ Distance of point $(n_1, n_2)$ from the center of the image.
* $D_0$ Constant that defines the "radius" of the low pass filter.

{{% tabs%}}
{{% tab Ideal%}}
A function that returns an ideal ($M\times M$) low pass filter is:

{{% tabs%}}


{{% tab Python%}}
```python

def myLowPassIdeal(D0, M):
    center = M / 2
    filtOut = np.zeros((M, M))

    for n1 in range(0, M):
        for n2 in range(0, M):
            if (n1-center)**2 + (n2-center)**2 <= D0**2:
                filtOut[n1][n2] = 1

    return filtOut

```

Provided that **numpy** is imported as **np**.

{{% /tab %}}

{{% tab Matlab%}}
```matlab

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
{{% /tab %}}

{{% /tabs %}}
An ideal $100\times100$ low pass filter with $D_0 = 30$ looks like:

{{< svg "static/img/projects/image_filters/ideal_low.svg" >}}
{{% /tab %}}


{{% tab Butterworth%}}

{{% tabs %}}
{{% tab Python%}}
```python

def myLowPassButterworth(D0, n, M):
    center = M / 2
    filtOut = np.zeros( (M, M) )

    for n1 in range(0, M):
        for n2 in range(0, M):
            D_squared = (n1-center)**2 + (n2-center)**2
            filtOut[n1][n2] = 1 / (1 + (D_squared/D0**2)**n)
    return filtOut
```

{{% /tab %}}

{{% tab Matlab%}}
```matlab

function filtOut = myLowPassButterworth(cutoff, n, M)
%MYLOWPASSBUTTERWORTH Creates Butterworth low pass filter (mxm)
%   Arguments:
%       cutoff  : the cutoff frequency
%       n       : the filter's rank
%       M       : the dimension of the square filter
%   Returns:
%       filtOut : the filter on the frequency domain

% center point in the middle
center = M / 2;

% Butterworth low pass filter
filtOut = zeros(M);
for i = 1 : M
    for j = 1 : M
        D_squared = (i-center)^2 + (j-center)^2;
        filtOut(i, j) = 1 / (1 + (D_squared/cutoff^2)^n);
    end
end


end


```
{{% /tab %}}

{{% /tabs %}}
A butterworth $100\times100$ low pass filter with $D_0 = 30$ for varying $n$'s looks like:

{{< svg "static/img/projects/image_filters/butterworth_low.svg" >}}

As we can see, the "steepness" of the filter increases with n.
{{% /tab %}}


{{% tab Gauss%}}

{{% tabs %}}
{{% tab Python%}}
```python

def myLowPassGauss(D0, M):
    center = M / 2
    filtOut = np.zeros( (M, M) )

    for n1 in range(0, M):
        for n2 in range(0, M):
            D_squared = (n1-center)**2 + (n2-center)**2
            filtOut[n1][n2] = np.exp(-D_squared/(2*D0**2))

    return filtOut
```

{{% /tab %}}

{{% tab Matlab%}}
```matlab

function filtOut = myLowPassGauss(sigma, M)
%MYLOWPASSGAUSS Creates Gaussian low pass filter (mxm)
%   Arguments:
%       sigma   : the variance that specifies the cutoff frequency
%       M       : the dimension of the square filter
%   Returns:
%       filtOut : the filter on the frequency domain

% center point in the middle
center = M / 2;

% Gauss low pass filter
filtOut = zeros(M);
for i = 1 : M
    for j = 1 : M
        D_squared = (i-center)^2 + (j-center)^2;
        filtOut(i, j) = exp(-D_squared/(2*sigma^2));
    end
end

end


```
{{% /tab %}}

{{% /tabs %}}
Let's see how a $100\times100$ filter with $D_0 = 30$ looks like:

{{< svg "static/img/projects/image_filters/gauss_low.svg" >}}

{{% /tab %}}

{{% /tabs %}}

## High pass filters
The equivalent pass filters are:

1. The ideal high pass filter.
2. The butterworth high pass filter.
3. The gaussian high pass filter.


Their mathematical notations are: 


| Filter          | Function                                                                                  |
| ----------------| ----------------------------------------------------------------------------------------- |
| Ideal           | $f(n_1,n_2) = \begin{cases} 1 & \text{if }D\geq D_0, \\\\0 & \text{if }D<D_0.\end{cases}$ |
| Butterworth     | $f(n_1,n_2) = \frac{1}{1+(\frac{D_0}{D})^{2n}}$     |
| Gaussian        | $f(n_1,n_2) = 1-\exp(-\frac{D^2}{2D_0^2})$          |

* $n_1, n_2 = 1, ..., M$ are the indices of the filter table's values.
* $D =$ Distance of point $(n_1, n_2)$ from the center of the image.
* $D_0 =$ Constant that defines the "radius" of the low pass filter.

{{% tabs%}}
{{% tab Ideal%}}

{{% tabs%}}


{{% tab Python%}}
```python

def myHighPassIdeal(D0, M):
    center = M / 2
    filtOut = np.zeros((M, M))

    for n1 in range(0, M):
        for n2 in range(0, M):
            if (n1-center)**2 + (n2-center)**2 >= D0**2:
                filtOut[n1][n2] = 1

    return filtOut

```

Provided that **numpy** is imported as **np**.

{{% /tab %}}

{{% tab Matlab%}}
```matlab

    function filtOut = myHighPassIdeal(cutoff, M)
    %MYHIGHPASSIDEAL Creates an ideal high pass filter (mxm)
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
            if (i-center)^2 + (j-center)^2 >= cutoff^2
                filtOut(i, j) = 1;
            end
        end
    end

    end

```
{{% /tab %}}

{{% /tabs %}}
An ideal $100\times100$ high pass filter with $D_0 = 30$ looks like:

{{< svg "static/img/projects/image_filters/ideal_high.svg" >}}
{{% /tab %}}


{{% tab Butterworth%}}

{{% tabs %}}
{{% tab Python%}}
```python

def myHighPassButterworth(D0, n, M):
    center = M / 2
    filtOut = np.zeros( (M, M) )

    for n1 in range(0, M):
        for n2 in range(0, M):
            D_squared = (n1-center)**2 + (n2-center)**2
            if D_squared != 0:
                filtOut[n1][n2] = 1 / (1 + (D0**2/D_squared)**n)
    return filtOut
```

{{% /tab %}}

{{% tab Matlab%}}
```matlab

function filtOut = myHighPassButterworth(cutoff, n, M)
%MYHIGHPASSBUTTERWORTH Creates Butterworth high pass filter (mxm)
%   Arguments:
%       cutoff  : the cutoff frequency
%       n       : the filter's rank
%       M       : the dimension of the square filter
%   Returns:
%       filtOut : the filter on the frequency domain

% center point in the middle
center = M / 2;

% Butterworth low pass filter
filtOut = zeros(M);
for i = 1 : M
    for j = 1 : M
        D_squared = (i-center)^2 + (j-center)^2;
        if D_squared ~= 0
            filtOut(i, j) = 1 / (1 + (cutoff^2/D_squared)^n);
        end
    end
end


end


```
{{% /tab %}}

{{% /tabs %}}
A butterworth $100\times100$ high pass filter with $D_0 = 30$ for varying $n$'s looks like:

{{< svg "static/img/projects/image_filters/butterworth_high.svg" >}}

Once again, the "steepness" of the filter increases with n.
{{% /tab %}}


{{% tab Gauss%}}

{{% tabs %}}
{{% tab Python%}}
```python

def myHighPassGauss(D0, M):
    center = M / 2
    filtOut = np.zeros( (M, M) )

    for n1 in range(0, M):
        for n2 in range(0, M):
            D_squared = (n1-center)**2 + (n2-center)**2
            filtOut[n1][n2] = 1 - np.exp(-D_squared/(2*D0**2))

    return filtOut
```

{{% /tab %}}

{{% tab Matlab%}}
```matlab

function filtOut = myHighPassGauss(sigma, M)
%MYHIGHPASSGAUSS Creates Gaussian high pass filter (mxm)
%   Arguments:
%       sigma   : the variance that specifies the cutoff frequency
%       M       : the dimension of the square filter
%   Returns:
%       filtOut : the filter on the frequency domain

% center point in the middle
center = M / 2;

% Gauss high pass filter
filtOut = zeros(M);
for i = 1 : M
    for j = 1 : M
        D_squared = (i-center)^2 + (j-center)^2;
        filtOut(i, j) = 1 - exp(-D_squared/(2*sigma^2));
    end
end

end


```
{{% /tab %}}

{{% /tabs %}}
Let's see how a $100\times100$ filter with $D_0 = 30$ looks like:

{{< svg "static/img/projects/image_filters/gauss_high.svg" >}}

{{% /tab %}}

{{% /tabs %}}