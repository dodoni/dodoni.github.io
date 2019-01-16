## 1. Introduction
**Dodoni.net** is a free/open-source library with the aim to provide an object-oriented framework for 
* numerical computations, i.e. Linear Algebra operations, Numerical Integration, Fast-Fourier-Transformation, 1-/n-dimensional Optimization, 
curve/surface fitting, special functions etc. 
* quantitative finance (pricing and risk management).
* actuarial science.

Moreover the Excel interface of Dodoni.net contains a part that serves as a high-level extension of [Excel-DNA](https://excel-dna.net), 
i.e. it can be used as building block for individual Excel Add-In's independent of other features of Dodoni.net.

* Dodoni.net is free for all use, and distributed under [MIT license](https://github.com/dodoni/dodoni.net/LICENSE) that also allows commercial use. 
* The project [Roadmap](Roadmap) provide an overview of progress made towards releasing a version. 

**[Release notes](https://github.com/dodoni/dodoni.net/ReleaseNotes.md)**

## 2. Preliminaries
Dodoni.net is developed under .net Standard 2.0. 

* The Excel interface is based on .NET framework (4.7.x) and can be used on a 32- and 64 bit operation system.

* [NUnit](www.nunit.org) test projects are developed under .net core.

The library has not been tested under Linux environment.


The complete 
feature set of Dodoni.net requires external libraries for [BLAS](http://www.netlib.org/blas/), [LAPACK](http://www.netlib.org/lapack/), 
Random Number Generators, Fast-Fourier-Transformations (FFT) etc. 
Some assemblies contains _unsafe_ code (for native code wrapper), thus the library must run in a full trust environment; in general this is no restriction at all.

**... for the Excel Add-In:**
The Excel interface is based on the [Excel-DNA](https://excel-dna.net) project, an intuitive open source .NET library for using .NET functions in Excel, 
designed by Govert van Drimmelen. The Add-In has been tested with
* Microsoft Excel 2007 and Excel 2010 (32- and 64 bit).

It should work with other MS Excel versions as well. Macro running must be enable, at least for the `.XLL` file of the Dodoni.net Add-In, 
change the security level if required. Moreover one should have the right to change (configuration) files in the directory where the .XLL` file is located. 

## 3. Installation
The Dodoni.net library is avaiable in the [NuGet Gallery](https://www.nuget.org). Moreover you can download an Excel-AddIn, i.e. a `.XLL` file. The Excel Add-In does not support 
all features of the Dodoni.net framework. 

The complete feature set of Dodoni.net requires external (native) libraries for
* [BLAS](http://www.netlib.org/blas/), 
* [LAPACK](http://www.netlib.org/lapack/), 
* Random Number Generators, 
* Fast-Fourier-Transformations (FFT), optimizer etc. 

Due to license restrictions some external library should be downloaded separatly and one has to copy the corresponding 
libraries (dll's) into the specified binary folder. Some managed code is available as fallback implementation. 


## 4. Structure of the framework
The Dodoni.net project is divided into several assemblies (packages). Each of it is related to a specific purpose:
 
 **[Dodoni.BasicComponents](BasicComponents)**: 
Contains basic classes and methods needed for the project. This assembly is _not_ directly related to mathematical problems, 
mathematical finance, Excel interface etc.

 **[Dodoni.BasicMathLibrary](BasicMathLibrary)**: 
Provides _mainly the infrastructure_ and some basic implementations for mathematical operations, for example:
* [BLAS](http://www.netlib.org/blas/) library (interface structure + a managed fallback implementation), 
* [LAPACK](http://www.netlib.org/lapack/) library (interface structure), 
* Fast-Fourier-Transformations (FFT) (interface structure + a dummy managed fallback implementation),
* Vector operations (interface structure + a managed fallback implementation),
* [Special functions](http://en.wikipedia.org/wiki/List_of_mathematical_functions) (interface structure),
* Interpolation and parametrization of curves and surfaces (interface structure  + some basic implementations: linear, spline etc.)
* numerical integration, Random Number Generators, Optimization etc. (interface structure)
* implementation for root finding algorithms for polynomials etc.

This enables to incorporate the functionality of 3rd party mathematical libraries, as for example [Math Kernel Library](http://en.wikipedia.org/wiki/Math_Kernel_Library) (MKL), 
[AMD Core Math Library](http://en.wikipedia.org/wiki/AMD_Core_Math_Library) (ACML), [Fastest Fourier Transform in the West](http://www.fftw.org/) (FFTW), 
[NLopt](http://ab-initio.mit.edu/wiki/index.php/NLopt), [Yeppp!](http://www.yeppp.info/) etc. 
See 
> Dodoni.MathLibary.Native.<Name of 3rd party Library>

for a specific wrapper. 

The [Managed Extensibility Framework](https://docs.microsoft.com/en-us/dotnet/framework/mef/index)(MEF) is used to dynamic link external  
libraries to the Dodoni.net framework. The following assemblies provides wrapper for 
specific (native) 3rd Party Libraries to enable the use of these Libraries within the Dodoni.net framework:

**Assembly** | **Library** | **Remarks** |
-------------|-------------|---------------|
**[Dodoni.MathLibrary.Native.ACML](Dodoni.MathLibrary.Native.ACML)**| [AMD Core Math Library](http://en.wikipedia.org/wiki/AMD_Core_Math_Library) (ACML) Library | The ACML is an end-of-life product.
**[Dodoni.MathLibrary.Native.BLAS](Dodoni.MathLibrary.Native.BLAS)**| [BLAS](http://www.netlib.org/blas/) Library (Fortran interface)
**[Dodoni.MathLibrary.Native.CBLAS](Dodoni.MathLibrary.Native.CBLAS)**| C-Interface of the [BLAS](http://www.netlib.org/blas/) Library (CBLAS)
**[Dodoni.MathLibrary.Native.FFTW](Dodoni.MathLibrary.Native.FFTW)**| [FFTW](http://www.fftw.org/) Library
**[Dodoni.MathLibrary.Native.LibM](Dodoni.MathLibrary.Native.LibM)**| [LibM](https://developer.amd.com/amd-cpu-libraries/amd-math-library-libm/), a 64bit software library containing a collection of basic math functions and vector functions | No support for Windows anymore?
**[Dodoni.MathLibrary.Native.MKL](Dodoni.MathLibrary.Native.MKL)**| [Math Kernel Library](http://en.wikipedia.org/wiki/Math_Kernel_Library) (MKL) Library
**[Dodoni.MathLibrary.Native.NLopt](Dodoni.MathLibrary.Native.NLopt)**| [NLopt](http://ab-initio.mit.edu/wiki/index.php/NLopt), a free/open-source library for nonlinear optimization
**[Dodoni.MathLibrary.Native.Yeppp](Dodoni.MathLibrary.Native.Yeppp)**| [Yeppp!](https://bitbucket.org/MDukhan/yeppp),  a high-performance SIMD-optimized mathematical library for x86, ARM, and MIPS processors on Windows, Android, Mac OS X, and GNU/Linux systems | Not under active development?

 **[Dodoni.CommonMathLibrary](CommonMathLibrary)**: 
Contains some managed implementation of mathematikcal functions, for example:
* numerical integration algorithms (for example Gauss-Kronrod-Patterson etc.),
* some curve/surface interpolation approaches,
* some [Special functions](http://en.wikipedia.org/wiki/List_of_mathematical_functions) (for example `erf(x), erfc(x), LambertW(x)` etc.),
* some 1-/n-dimensional optimization algorithm (Brent, Powell, PRAXIS etc.) etc.

**[Dodoni.FinanceBasics](FinanceBasics)**: 
Provides _interfaces_ for 
* day count conventions, 
* business day conventions, 
* holiday calendars, 
* Market convention templates etc. 
Moreover it contains some Tenor arithmetic, Compounding rules, date factory etc. as well as an implementation of the Black-Scholes and the Bachelier (=normal Black) model etc. 
It does not contain an engine for pricing or risk management for financal instruments etc. This is located in a separate assembly.


**[Dodoni.FinanceCommonMarketUsages](FinanceCommonMarketUsages)**: 
Contains market usages, i.e. implementations for day count conventions, business day conventions, holiday calendars etc.

_**... for the Excel Add-In:**_

**[Dodoni.XLBasicComponents](XLBasicComponents)**: 
Provide functionality for a generic Excel Add-In, i.e. extends [Excel-DNA](exceldna.codeplex.com) by _high-level_ methods, for example an object pool easy accessible in Excel, property/value Excel range queries etc.

**Dodoni.XLIntegration**: 
Serves as Excel Add-In for the Dodoni.net project that provides UDF's (user defined functions) for parts of the functionality of the Dodoni.net project. 
The source code is _not yet published_.

## 5. Further documentation
* A documentation of the API of the Dodoni.net framework is part of the distribution. 
* The unit tests of the Dodoni.net project serves as _living documentation_.

_**... for the Excel Add-In:**_
* A documentation of the UDF's (user defined functions) provided by the Excel Add-In of the Dodoni.net project and several example Excel sheets is part of the distribution. 
* _The name of each user defined function (UDF) of the Add-In starts with 'do' which can be interpreted as **do** for doing or as short-name for **Do**doni.net._

## 6. Howto
* **....write _individual_  Excel Add-Ins with [Excel-DNA](exceldna.codeplex.com) and Dodoni.net?**

Create a new .net project. Add references at least to the following assemblies of the distribution of [Excel-DNA](exceldna.codeplex.com) and Dodoni.net: 
* Dodoni.BasicComponents, 
* Dodoni.XLBasicComponents, 
* ExcelDna.Integration.

Copy the 32- or 64-bit XLL file of [Excel-DNA](https://excel-dna.net) to the directory of the binaries of your .net project and store it under 
a file name of your choice. Create a text file with the same file name and suffix .dna with the following content:

```xml
<DnaLibrary RuntimeVersion="v4.0">
  <ExternalLibrary Path="Dodoni.XLBasicComponents.dll" ExplicitExports="true" Pack="true"/>
  <ExternalLibrary Path="NameOfYourProject.dll" ExplicitExports="true" Pack="true"/>
  <Reference AssemblyPath="Dodoni.BasicComponents.dll" Pack="true"/>
</DnaLibrary>
```

Of course the above file represents the simplest case only (the entry Pack="true" is optional). If necessary one has to add further references in the .dna file. 
One may have a deeper look in the documentation of the [Excel-DNA](https://excel-dna.net) project. 
For the features of the [Dodoni.XLBasicComponents](XLBasicComponents) assembly we refer to the documentation of the Dodoni.net API. Moreover the distribution contains a simple example project in the repository.

* **...extend the Excel Add-In of the Dodoni.net project?**

If you like to add further UDF (user defined functions) to the Excel Add-In `Dodoni.XLIntegration`, create a new .net project and add references to `ExcelDna.Integration` of the 
distribution of [Excel-DNA](exceldna.codeplex.com) and to the required assemblies of the Dodoni.net project, i.e. _at least_
* Dodoni.BasicComponents, 
* Dodoni.XLBasicComponents, 
* Dodoni.XLIntegration.

Copy the XLDodoni.dna file of the distribution of the Dodoni.net project to the directory of the binaries of your .net project and store it under a 
file name of your choice. Modify the .dna file in a way that it refers to your .net project as well. Copy the 32- or 64-bit XLL file 
of [Excel-DNA](exceldna.codeplex.com) to the same directory and store it under the same name as the .dna file. This XLL should make available your individual UDF's.

* **...use a specific logging?**
The logging is mainly based on [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/). Write an individual implementation for 
the ILogger interface and mark it with the Export attribute of the  Managed Extensibility Framework (MEF), i.e.

``` csharp
[Export(typeof(ILogger))]
```

Create or modify the configuration file of the Dodoni.net project or of your individual project in the following way; for more information see [BasicComponents](BasicComponents):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="LoggingSetting" type="Dodoni.BasicComponents.Logging.Configuration.LoggingConfigurationFileSection, Dodoni.BasicComponents"/>
  </configSections>
  <LoggingSetting typeName="YourNamespace.YourClass, YourAssemblyName" />
</configuration>
```

* **...create an individual user interface with a separate implementation for Holiday calendar, business day conventions etc.? For example how to link the Dodoni.net 
framework to a trading or riskmanagement system?**

Perhaps instead of the Excel Add-In you may use functionality of the Dodoni.net library in a different program, for example a trading system or riskmanagement system. 
In this case you should ignore the assemblies of the Dodoni.net framework which are connected to the Excel Add-In, i.e. 
* Dodoni.XLBasicComponents and 
* Dodoni.XLIntegration. 

In general a trading system already contains implementations for Holiday calendar, business day conventions etc. 
Therefore you should replace Dodoni.FinanceCommonMarketUsages by a new assembly that wraps these functions and 
structures to the infastructure of Dodoni.net.
