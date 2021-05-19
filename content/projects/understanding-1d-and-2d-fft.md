---
title: "Understanding 1D and 2D FFT"
date: 2021-05-17T14:22:03-04:00
tags: ["Image Processing"]
draft: false
---

Diagrams and explanations that helped me understand Fourier Transforms for signal and image analysis.

<!--more-->

- [FFT: A Pattern Based Perspective](#fft-a-pattern-based-perspective)
- [1D Fourier Transforms](#1d-fourier-transforms)
  - [Understand the 1D Math](#understand-the-1d-math)
  - [Read the 1D Frequency Plots](#read-the-1d-frequency-plots)
- [Generalizing to 2D Fourier Transform](#generalizing-to-2d-fourier-transform)
  - [Understand the 2D Math](#understand-the-2d-math)
  - [Read the 2D Plots](#read-the-2d-plots)
- [Exercises](#exercises)
- [Further Reading](#further-reading)

## FFT: A Pattern Based Perspective

Think of a sine wave.

You probably thought of a squiggly line with an amplitude of 1 and a period of $2\pi$. Or maybe a unit circle on the complex plane. If I took your sine wave and doubled the frequency or shifted it over by some amount, you could probably reverse engineer what I did with pretty little effort.

However, if I gave you a signal like this:

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/FFTOfNoisySignalExample_01.png" 
caption="image source: [MathWorks FFT Tutorial](https://www.mathworks.com/help/matlab/ref/fft.html)"
>}}

You would probably have a pretty hard time trying to reverse engineer what went into it. And that's a problem -- we need to do frequency analysis and frequency filtering all the time! WiFi, telephones, Photoshop, even battleship simulations (see this [Quora thread](https://www.quora.com/Why-are-Fourier-series-important-Are-there-any-real-life-applications-of-Fourier-series) for cool anecdotes) depend on it!

Fortunately, in the 1800s, Joseph Fourier figured out how to write periodic functions could as an infinite sum of sinusoids (the Fourier Series). His theory paved the way for the Fourier Transforms.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/time_frequency_explanation.png" 
caption="A 1D signal can be represented as a weighted sum of sinusoids. Took this image from [\"An Interactive Guide To The Fourier Transform\"](https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/), a really good post by BetterExplained"
>}}

The Fourier Transform is a projection that transforms functions that depend on space or time into functions that depend on spatial or temporal *frequency*. Representing functions in the frequency domain allows us to visualize and analyze patterns in the function. The 2D Fourier Transform has applications in image analysis, filtering, reconstruction, and compression.

## 1D Fourier Transforms

Let's first understand the one dimensional discrete Fourier Transform. This is the most commonly encountered version, as we typically capture and analyze digital, sampled signals.

### Understand the 1D Math

The Discrete Fourier Transform (DFT) turns a 1D array of *N* discrete, evenly spaced time points, $x$ into a set of coefficients $X$ that describe the weight placed onto N frequency components:

$$X_k = \sum_{n=0}^{N-1} x_n * e^{-i2\pi kn/N}$$

* $n$ specifies the index of the current time point, goes from 0 to *N-1*
* $k$ is a discrete set of *N* frequencies, goes from 0 Hertz up to *N-1* Hertz

The Inverse Fourier Transform allows us to project the frequency function back into the space or time domain without any information loss. It uses a normalization factor of $\frac{1}{N}$ to make the matrix [unitary](https://en.wikipedia.org/wiki/Unitary_matrix) so that it can use its conjugate transpose instead of calculating a matrix inversion.

$$x_k = \frac{1}{N} \sum_{n=0}^{N-1} X_k * e^{-i2\pi kn/N}$$

The complex exponential decomposes into sines and cosines via Euler’s formula:

$$e^{ix} = \cos(x) + i \sin(x)$$

These equations are a bit ugly. To understand them as a projection, let's rewrite them as a matrix multiplication. The DFT and its inverse both sum over all combinations of $e^{i2\pi kn/N}$. Since $k$ and $n$ are both *N* values long, the DFT projection can be written as $X = Wx$ where $x$ is the original signal, $X$ is the DFT coefficient array, and $W$ is a *N-by-N* square [DFT matrix](https://en.wikipedia.org/wiki/DFT_matrix) of $w = e^{-2\pi /N}$ elements raised to all combinations of exponents $k$ and $n$.

In $W$, each element $w = e^{-2\pi /N}$ is a primitive Nth root of unity. For any exponent *x* the Nth roots of unity repeat themselves with a period of *N*, so $w^{x} = w^{x \mod N}$. This leads to a pattern of symmetry along the upper left to lower right diagonal.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/8_point_DFT_equation.png" 
caption="eight-point DFT Matrix, colored by exponent values going from 0 to 7"
>}}

The DFT Matrix can be visualized by plotting the repetition pattern of each row. This representation allows us to see that the DFT Transform is really the weights X resulting from projecting x onto a set of sinusoidal frequencies. 

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/8_point_DFT.png" 
caption="DFT as a matrix multiplication. Real part (cosine) shown by a solid line, and the imaginary part (sine) by a dashed line. From [DFT matrix – Wikipedia](https://en.wikipedia.org/wiki/DFT_matrix)"
>}}

To make the frequency plots easier to read, shift the frequency components so that the zero frequency is in the center and higher frequencies are at the edges.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/8_point_DFT_shifted.png" 
caption="To center a 1D DFT, swap the left and right halves of X."
>}}

Currently, the DFT coefficients are in their complex form. Reading raw real and imaginary components isn't that useful. To analyze magnitude and phase, convert the coefficients into their polar form.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/polar_representation.png" 
caption="To center a 1D DFT, swap the left and right halves of X."
>}}

Finally, we can plot the magnitude and phase to see how they change over the different frequency components. The x-axis has the frequency component, and the y-axis has the DFT-coefficient magnitude or phase, depending on the plot.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/frequency_axis_units.png" 
caption="Different but equivalent units for the frequency components on the x-axis"
>}}

Based on your interpretation needs, the frequency components labels on the x-axis can be in index (0 to N-1), frequencies (Hz), or angular frequencies (radians per second). Frequency can be converted to angular frequency by multiplying by a constant $2\pi$.

### Read the 1D Frequency Plots

Let's compare the plots of some simple signals: 8 Hz sine, 16 Hz sine, and 8 Hz cosine.

{{< figure 
height=600 
src="/understanding-1D-and-2D-FFT/sine_waves_fft.png" 
caption="Centered phase and magnitude plots for a sine wave of 8 Hz, a sine wave of 16 Hz, and a sine wave of 8 Hz with a phase offset of π/2 (cosine wave)."
>}}

The phase plot tends to be noisy. This is because phase is computed from the ratio of imaginary part to real part of the FFT result. Even a small floating rounding error leads to relatively big values in the phase plot. In the above phase plots for simple sine and cosine waves, there should only be one dominant frequency, but rounding errors obscure the details.

The dominant frequencies can be identified from the magnitude plots. In the above plots, the magnitude plot of the 8 Hz sine wave has peaks at -8 Hz and +8 Hz, and the magnitude plot of the 16 Hz sine wave has peaks at -16 Hz and +16 Hz. The 8 Hz cosine wave has the same peaks as the 8 Hz sine wave, but its phase plot looks different (phase caries location information).

Plotting only the dominant frequencies, identified from the magnitude plot, gives a cleaner phase plot.

{{< figure 
height=400
src="/understanding-1D-and-2D-FFT/sine_waves_fft_clean.png" 
caption="Cleaned centered phase and magnitude plots for a Hz sine wave and 8 Hz cosine wave."
>}}

The cosine wave has a phase shift close to zero radians. The sine wave has a phase shift close to 1.5 radians (90 degrees). The plots do not show these values exactly due to aforementioned rounding errors. 

## Generalizing to 2D Fourier Transform

Images can be represented as 2D functions f(x, y) varying in spatial coordinates (x, y) in the image plane. Just like a 1D wave, every 2D image signal can be decomposed into a series of sine terms. 

To visualize the sinuous nature of image signals, we can plot the pixel intensities of the image in a surface plot and observe the top view of its contours. In this view, areas of sharp change, like edges, appear as narrow peaks, and swatches of uniform values, like the evenly colored sky, appear flat with no or low gradual peaks. These patterns have direct parallels with the sharp narrow peaks of high frequency waves and low gradual peaks of low frequency waves.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/cameraman_surface_plot.png" 
caption="The camera man, a famous image in image processing literature, and his corresponding image surface / Bed Sheet View. From [matlab meshCanopy](https://www.mathworks.com/matlabcentral/fileexchange/29485-meshcanopy)"
>}}

This type of plot is called a “Bed Sheet View” (developed by Dr. Steven Brunton at the University of Washington, source: [Fourier transforms and bed sheet view of images](https://towardsdatascience.com/fourier-transforms-and-bed-sheet-view-of-images-58ba34e6808a)). The name suggests that an image can be represented by a series of overlaid bedsheets. If we had an infinite number of bedsheets, held each one by its four corners, and oscillated it at one of the Fourier frequencies with the correct corresponding magnitude and phase, then the superposition of all the bedsheets would have creases that looked like the surface plot of an image!

### Understand the 2D Math

 The 2D discrete Fourier transform projects the *NxN* image signal $f$ onto a basis of 2D sine and cosine functions (think bedsheets) in order to get the *NxN* matrix of Fourier coefficients $F$. The 2D basis functions are ordered by increasing frequency so that the top left corner of the coefficient matrix, $F(0, 0)$, represents the DC-offset of the image, or average brightness, and the bottom right corner of the coefficient matrix, $F(N-1, N-1)$ represents the highest frequency. For a square image of size *NxN*, the two dimensional discrete Fourier transform is given by:

 $$F(k, l) = \sum_{i=0}^{N-1} \sum_{j=0}^{N-1} f(i, j) * e^{-i2\pi (\frac{ki}{N} + \frac{lj}{N})}$$

where
* $f(i, j)$ is the value of the pixel at row $i$ and column $j$
* $F(k, l)$ is the Fourier coefficient for each point $(k, l)$ in the Fourier space
* $N$ is the total number of pixels along the x and y direction

The Fourier image can be re-transformed to the spatial domain using the inverse Fourier transform. It uses a normalization factor of $\frac{1}{N^{2}}$ to make the matrix unitary:

 $$f(i, j) = \frac{1}{N^{2}} \sum_{k=0}^{N-1} \sum_{l=0}^{N-1} F(k, l) * e^{i2\pi (\frac{ki}{N} + \frac{lj}{N})}$$

 In practice, the number of calculations in the 2D Fourier Transform formulas are reduced by rewriting it as a 1D FFT in the x-direction followed by a 1D FFT in the-y direction.
* Mike X Cohen” has a nice animated explanation: [“How the 2D FFT works” YouTube](https://youtu.be/v743U7gvLq0)
* see [NYU online lecture slides 48-49](https://eeweb.engineering.nyu.edu/~yao/EL5123/lecture4_2DFT.pdf) for details of computational savings

Using the [Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform) method computes the 1D DFTs in $\log{2N}$ time. To support this fast, recursive. Some forms of the FFT restrict the size of the input image, often to $N = 2n$ where $n$ is an integer.

### Read the 2D Plots

The Fourier Transform math works by assuming that the given spatial image is just one period in an infinitely repeating spectrum. For example when it looks at the camera man image, it sees:

{{< figure 
height=400 
src="/understanding-1D-and-2D-FFT/cameraman_infinite_spectrum.png" 
caption="Repeating spectrum of the cameraman image"
>}}

Like the 1D Fourier Transform, its 2D counterpart also produces a complex output. You can turn the complex output into polar form to observe its magnitude and phase and plot the results. Just like the 1D plots, shift the frequency so that lowest frequencies are in the center (swap first and third quadrants, swap second and fourth quadrants).

{{< figure 
height=400 
src="/understanding-1D-and-2D-FFT/cameraman_explore_fft.png" 
caption="Cameraman FFT magnitude and phase"
>}}

As in the 1D plots, the phase plot is noisy. Dominant frequencies can be identified from the magnitude plots. 

When looking at the magnitude plot, a single bright dot (top left corner on magnitude plot, center on shifted magnitude plot) is visible. This point corresponds to the DC offset, or average value, of the image. It has a much greater magnitude than the other frequencies. In order to see more information in the plots, use a log transform to non-linearly scale the magnitude values so that the big values are compressed into a smaller range and more information is visible in the plot.

In the log of magnitude plot, several frequencies are show up. These frequencies describe patterns of repetition like:

{{< figure 
height=400 
src="/understanding-1D-and-2D-FFT/cameraman_spectrum_annotated.png" 
caption="Annotated spectrum of the cameraman image"
>}}

For example, the sky and ground create horizontal stripes in the infinite spectrum. Horizontal stripes can be represented by a horizontal 2D sine wave.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/example_fft2_sine_wave.png" 
caption="Horizontal sine wave stripes FFT magnitude plot has a pattern of vertical dots indicating the presences of a periodic pattern across the y-direction."
>}}

The horizontal stripes' shifted magnitude plot has three dots, the DC-offset in the center, and two points along the vertical axis. The two points correspond to the dominant frequencies of the image. Find the distance from the points to the center of the plot in order to identify the frequency component they correspond to.

The log transform compresses the range of the magnitudes to bring more more information into the visible image range. When analyzing the horizontal sine plot, its log magnitude plot reveals all of the lesser frequencies that are harmonic multiples of the original frequency.

## Exercises

To get a feel for reading patterns from 2D FFT centered magnitude plots, look at the following examples. For each image, think about the repeating unit in terms of shape, direction, and frequency of periodic repetition.

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/example_fft2_brick.png" 
caption="What causes the stripe pattern in the brick magnitude plot?"
>}}

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/example_fft2_mustard_seeds.png" 
caption="What causes the circular rings in the mustard seed magnitude plot?"
>}}

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/example_fft2_honeycomb.png" 
caption="What causes the hexagonal rings in the honeycomb magnitude plot? Compare it to the mustard seeds."
>}}

{{< figure 
height=200 
src="/understanding-1D-and-2D-FFT/example_fft2_flower.png" 
caption="What causes the vertical and horizontal lines in the flower magnitude plot?"
>}}

## Further Reading

[The Fundamentals of FFT-Based Signal Analysis and Measurement](https://www.sjsu.edu/people/burford.furman/docs/me120/FFT_tutorial_NI.pdf) by National Instruments does a nice job explaining basic signal analysis computations, and goes into detail about windowing and the different properties of different windowing functions.
 
These sites have nice examples and explanations for reading 2D magnitude plots, identifying and removing sources of artificial patterns, and filtering in the frequency domain:
* [Image Transforms - Fourier Transform](https://homepages.inf.ed.ac.uk/rbf/HIPR2/fourier.htm) by Hypermedia Image Processing Reference (HIPR2)
* [Introduction to the Fourier Transform](https://www.cs.unm.edu/~brayer/vision/fourier.html) by John M. Brayer, University of Mexico
* [Spatial Frequency Domain](https://www.cs.auckland.ac.nz/courses/compsci773s1c/lectures/ImageProcessing-html/topic1.htm) by the University of Auckland, New Zealand

Using FFT in Python:

[Fourier Transforms (scipy.fft) — SciPy v1.6.3 Reference Guide](https://docs.scipy.org/doc/scipy/reference/tutorial/fft.html) is Scipy’s overview for using its FFT library.
[General examples — skimage v0.18.0 docs](https://scikit-image.org/docs/stable/auto_examples/) is a gallery of examples for Scikit-Image Python image processing library. It provides helpful tutorials for thresholding, windowing, filtering, etc.































