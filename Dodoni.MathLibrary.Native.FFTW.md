## {{Dodoni.MathLibrary.Native.FFTW}}

#### 1. Overview
Provides wrapper for the [FFTW](http://www.fftw.org), a C subroutine library for computing the discrete Fourier transform (DFT) in one or more dimensions, of arbitrary input size, and of both real and complex data (as well as of even/odd data, i.e. the discrete cosine/sine transforms or DCT/DST) library that provides a free set of thoroughly optimized and threaded math routines for HPC, scientific, engineering and related compute-intensive applications. The FFTW package was developed at MIT by [Matteo Frigo](http://www.fftw.org/~athena/) and [Steven G. Johnson](http://math.mit.edu/~stevenj/). 

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](http://en.wikipedia.org/wiki/Managed_Extensibility_Framework) (MEF) is used to dynamic link to this assembly. If specific features of this assembly are used which are not covered by the generic interface one has to add a reference to this assembly; in all other cases a reference to this assembly is optional. One may have a look in the unit test project of {{Dodoni.MathLibrary.Native.FFTW}}; see API documentation for more information.

