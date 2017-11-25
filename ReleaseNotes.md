## Release notes

**1.0.0 preview 8** (01.07.2015)

* Dodoni.BasicMathLibrary: (stable, release candidate)
	* add interface structure for special functions (erf(x), 1F1(x) etc.) similar to BLAS, LAPACK etc., i.e. a external library can be used
	* improve interface for probability distribution functions (including estimators)
	* NormalDistribution and LogNormalDistribution moved to Dodoni.MathLibrary, instead a StandardNormalDistribution has been established, where the commulative distribution function etc. is taken from the special functions API
	* add interface structure for (one-dimensional) root finder
	* add cache for integration of curve interpolation

* Dodoni.ExternalMathLibrary.Yeppp:
	* this new assembly serves a wrapper for the Yeppp! library 

* Dodoni.CommonMathLibrary: (_Dodoni.MathLibrary_ has been renamed to _Dodoni.CommonMathLibrary_)
	* start to implement some special functions (erf(x), erfc(x), 1F1(x), W(x) etc.) which are needed for the improved implementation of (Log)NormalDistribution etc.
	* add further probability distributions 
etc.

**1.0.0 previewe 7** (11.09.2014)
* Dodoni.BasicMathLibrary:
	* improve LAPACK interface structure, add further LAPACK routines (further on demand)
	* improve/bugfixing DenseMatrix, GeneralBandMatrix etc.
	* improve interface structure for correlation matrix factory (Pseudo-Sqrt matrix)
	* improve interface structure for optimizer
	* further bugfixing and unit tests
* Dodoni.FinanceBasics:
	* add further Implied Black/Bachelier volatility algorithms
* Dodoni.MathLibrary:
	* add the following optimizer:
		* 1-dimensional: Brent, Golden Search, Simulated Annealing
		* Multi-dimensional: Nelder-Mead, Powell, PRAXIS
	* further bugfixing and unit tests
* Dodoni.ExternalMathLibrary.NLopt:
	* bugfixing
* Dodoni.XLBasicComponents:
	* upgrade to ExcelDNA 0.32
More than 8000 unit tests etc.

 **1.0.0 preview 6** (31.12.2013)
* Dodoni.BasicComponents: (stable, release candiate)
	* some bugfixing
* Dodoni.BasicLowLevelMathLibrary:
	* has been removed (reorganized, see BasicMathLibrary)
* Dodoni.BasicMathLibrary: (stable, some features are missing)
	* contains the _infastructure_ for BLAS, LAPACK, FFT but as well for numerical integration, interpolation, optimizer etc.
* Dodoni.MathLibrary.Native.ACML: (stable, release candidate)
* Dodoni.MathLibrary.Native.BLAS: (stable, release candidate)
* Dodoni.MathLibrary.Native.CBLAS: (stable, release candidate)
* Dodoni.MathLibrary.Native.FFTW: (stable, release candidate)
* Dodoni.MathLibrary.Native.MKL: (stable, release candidate)
* Dodoni.MathLibrary.Native.NLopt: (stable, release candidate)
* Dodoni.MathLibrary: (alpha)
	* contains some numerical integration routines etc.
* Dodoni.FinanceBasics: (beta)
* Dodoni.FinanceCommonMarketUsages: (alpha, contains a few holiday calendar etc. only)
* Dodoni.XLBasicComponents: (stable, release candidate)
* Dodoni.XLIntegrationGnuplot: (beta - it is a simple example for the use of Dodoni.XlBasicComponents only)
More than 7200 unit tests.

 **1.0.0 preview 5** (17.02.2013)

* Dodoni.BasicComponents:
	* bugfixing configuration file(s)

* Dodoni.BasicLowLevelMathLibrary:
	* managed implementation of BLAS established, unit test for level 1, 2, 3 and bugfixing of the native BLAS wrapper
	* start to establish a new LAPACK interface (not yet ready)
minor bugfixes in the other projects; especially the packaging into one xll file works now and is part of the distribution.


 **1.0.0 preview 4** (6.12.2012)
Mainly improvements on the Dodoni.BasicLowLevelMathLibrary project:

* Dodoni.BasicLowLevelMathLibrary (beta):
	* use System.Numerics.Complex instead of an individual implementation
	* add vector unit operations (support of MKL only)
	* Fast Fourier Transformation ready (including Fractional FFT); except the real FFT using AMD's ACML
	* BLAS level 1 includes a managed implementation [BLAS level 2, level 3 are untested, the LAPACK interfaces will be changed](BLAS-level-2,-level-3-are-untested,-the-LAPACK-interfaces-will-be-changed) 
	* add unit tests

* minor bugfixes on the other projects, upgrade to ExcelDNA 0.30 for the Excel interface, now more than 3.000 unit tests in total
 
 **1.0.0 preview 3** (03.10.2012) 
This pre-release represents the first release that contains some financial functionality. Establish further projects, add unit tests and do some bug fixing:

* Dodoni.BasicComponents (stable, release candidate)
* Dodoni.BasicLowLevelMathLibrary (alpha)
* Dodoni.FinanceBasics (beta)
* Dodoni.FinanceCommonMarketUsages (alpha, contains a few holiday calendar etc. only)
* Dodoni.XLBasicComponents (stable, release candidate)
* Dodoni.XLIntegrationGnuplot (alpha - but it is a simple example for the use of Dodoni.XlBasicComponents only)
The source code of the Dodoni.BasicMathLibrary project as well as the Dodoni.XlIntegration project that contains several Excel UDF's are not published yet. Both projects are quite incomplete and the API is still under construction. To allow the use of some functionality in Excel, the binaries are added to the distribution of Dodoni.net. If you want to compile the source code, there are a few dependencies to the Dodoni.BasicMathLibrary assembly only, mainly some constants and for example the distribution function of the normal distribution.

 **1.0.0 preview 2** (05.05.2012) 
Publish the project on codeplex.com, more precisely the generic Excel Add-In of the Dodoni.net project only, i.e. the following assemblies:
* Dodoni.BasicComponents,
* Dodoni.XLBasicComponents,
* Dodoni.XLGuidedTour (a simple example).

**1.0.0 preview 1** (21.08.2011)
First public release (binaries only) on www.dodoni-project.net.