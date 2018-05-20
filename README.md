### Project Description
**Dodoni.net** is a free/open-source library with the aim to provide a object-oriented framework for 
* numerical computations, i.e. Linear Algebra operations, Numerical Integration, Fast-Fourier-Transformation, 1-/n-dimensional Optimization, 
curve/surface fitting, special functions etc. 
* quantitative finance (pricing and risk management).

The Excel interface of Dodoni.net contains a part that can be seen as a high-level extension of the [Excel-DNA library](https://excel-dna.net), 
i.e. it serves as building block for individual Excel Add-In's.

### Introduction
#### The logo and project name
The name _Dodoni_ (also written _Dodona_) goes back to the oracle situated in northwestern Greece of the same name. The shrine of Dodona was regarded as the oldest Hellenic oracle, see [ http://en.wikipedia.org/wiki/Dodona]( http://en.wikipedia.org/wiki/Dodona). 
Priestesses and priests in the sacred grove interpreted the rustling of the oak (or beech) leaves to determine the correct actions to be taken. 

![The Dodoni.net logo](Home_DodoniLogo.jpg) In addition, the logo of the Dodoni.net project contains a stylized picture of the theater of Dodona 
which can be interpreted as **C** (for the language C#) as well.

#### General
Dodoni.net is developed using .NET, and users have to install the freely available .NET Framework runtime. 
One useful by-product of the Dodoni.net project is a high-level extension of the [Excel-DNA library](https://excel-dna.net) which allowes the development of Excel-AddIns. 
The complete feature set of Dodoni.net requires external libraries for [BLAS](http://www.netlib.org/blas/), [LAPACK](http://www.netlib.org/lapack/), 
Random Number Generators, Fast-Fourier-Transformations etc. See [Documentation](Documentation) for further information.

* The Dodoni.net runtime is free for all use, and distributed under a permissive open-source [license](license) that also allows commercial use.

#### Schedule/Roadmap
The source code (as well as the binaries) of the Dodoni.net project is separated into several assemblies (i.e. dll's). Each assembly is related to a specific purpose. See [Documentation](Documentation) for further information. It is planned to establish the Dodoni.net project in several steps, starting from a generic Excel Add-In to (financial) mathematical functions, supply finance market instruments etc.:
* [Roadmap](Roadmap),
* [Release notes](ReleaseNotes).

#### Third-party libraries
For the following third-party libraries some functionality is provided (as a .net wrapper) by the Dodoni.net project:
* [BLAS](http://www.netlib.org/blas/)  (Level 1-3; Fortran and C interface).
* [LAPACK](http://www.netlib.org/lapack/)  (large subset only).
* [FFTW (Fastest Fourier Transform in the West)](http://www.fftw.org/)  (1-dimensional only).
* [Intel Math Kernel Library (MKL)](http://en.wikipedia.org/wiki/Math_Kernel_Library)  (1-dimensional Fourier Transformation, Random Number Generators, Vector unit operations, Data Fitting etc.).
* [AMD Core Math Library (ACML)](http://en.wikipedia.org/wiki/AMD_Core_Math_Library)  (1-dimensional Fourier Transformation, Random Number Generators etc.).
* [NLopt](http://ab-initio.mit.edu/wiki/index.php/NLopt) is a free/open-source library for nonlinear optimization.
* [LibM](https://developer.amd.com/amd-cpu-libraries/amd-math-library-libm/) (from AMD) is a 64bit  software library containing a collection of basic math functions and vector functions.
* [Yeppp!](http://www.yeppp.info) is a high-performance SIMD-optimized mathematical library for x86, ARM, and MIPS processors on Windows, Android, Mac OS X, and GNU/Linux systems.

### Related Projects
**... for User interface:**
* [Excel-DNA](https://excel-dna.net) is an independent project to integrate .NET into Excel.

**... for Testing:**
* [NUnit](http://www.nunit.org) a unit-testing framework for all .Net languages.
* [NSubstitute](http://nsubstitute.github.io/) a friendly substitute for .NET mocking frameworks.
* [Keisan Online Calculator](http://keisan.casio.com/) provides High Accuracy results and useful calculators for daily life, learning, business and research.
* [Wolfram Functions Site](http://functions.wolfram.com) contains the world's most encyclopedic collection of information about mathematical functions.  

**... other:** 
* [Boost](http://www.boost.org/) provides free peer-reviewed portable C++ source libraries.
* [QuantLib](http://quantlib.org) is a free/open-source project aimed at providing a comprehensive software framework for quantitative finance, written in C++.
* [QlNet](https://github.com/amaggiulli/qlnet) is a .NET port of the QuantLib written in C#.
* [Math.NET Numerics](https://numerics.mathdotnet.com/) provide methods and algorithms for numerical computations in science, engineering and every day use.
