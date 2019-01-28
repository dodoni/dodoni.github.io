---
toc: false
title: Dodoni.MathLibrary.Native.ACML
sidebar: main_sidebar
permalink: MathLibrary.Native.ACML.html
---

#### 1. Overview
Provides wrapper for the [Core Math Library (ACML)](http://developer.amd.com/tools-and-sdks/cpu-development/amd-core-math-library-acml/), a library that 
provides a free set of thoroughly optimized and threaded math routines for HPC, scientific, engineering and related compute-intensive applications. 
The main native dll of the ACML Library should be renamed to `libACML.dll` to ensure that it can be found by  **Dodoni.MathLibrary.Native.ACML.dll**.

{% include note.html content="The ACML is an end-of-life product, i.e. no further updates etc." %}



#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](https://docs.microsoft.com/en-us/dotnet/framework/mef/index)(MEF) is used to dynamic link to this assembly. 
If specific features of this assembly are used which are not covered by the generic interface one has to add a reference to this assembly; 
in all other cases a reference to this assembly is optional (for example for Fast Fourier Transformations).

The following code snippet shows how random number can be generated:
``` csharp
int sampleSize = 100;
double a = 0.0;
double b = 1.0;
var library = new AcmlRandomNumberLibrary();
var sample = new double[sampleSize];

IRandomNumberStream randomStream = library.MT19937.Create(12);
// or: var randomStream = AcmlPseudoRandomNumberGenerator.MT19937.Create(12);
randomStream.NextNumberSequence.Uniform(sampleSize, sample, a, b);
```

{% include tip.html content="One may have a look in the unit test project 
of **Dodoni.MathLibrary.Native.ACML**; see API documentation." %}

#### 4. Important remarks
AMD discontinued support for 32-bit ACML. Many tests have been made with ACML 4.4 which seems to be the latest 32-bit release. 


