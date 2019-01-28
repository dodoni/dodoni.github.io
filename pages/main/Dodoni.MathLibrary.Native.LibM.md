---
toc: false
title: Dodoni.MathLibrary.Native.LibM
sidebar: main_sidebar
permalink: MathLibrary.Native.LibM.html
---

#### 1. Overview
Provides wrapper for [LibM](https://developer.amd.com/amd-cpu-libraries/amd-math-library-libm/), a 64bit software library (AMD) containing a 
collection of basic math functions and vector functions. Copy the file(s) to the binary folder of your .net code and rename it to `libAMDLibM.dll` 
to ensure that it can be found by **Dodoni.MathLibrary.Native.LibM**. 

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](https://docs.microsoft.com/en-us/dotnet/framework/mef/index)(MEF) is used to dynamic link to this assembly. 
If specific features of this assembly are used which are not covered by the generic interface one has to add a reference to this assembly; in all other cases a reference to this assembly is optional. 

{% include tip.html content="One may have a look in the unit test project 
of **Dodoni.MathLibrary.Native.LibM**; see API documentation." %}


#### 4. Important remarks
* The LibM library is limited to 64 bit. 
* No support for Windows OS any more.

{% include important.html content="LibM does not support many Vector operations, even simple vector operations as for example element-wise addition, subtraction etc. are not supported. In this cases a (slow) managed implementation is taken as a fall back solution. For that reason it is recommended to not use LibM in the Dodoni.net framework yet." %}



