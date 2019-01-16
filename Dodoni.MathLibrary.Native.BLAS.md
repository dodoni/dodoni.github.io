## `Dodoni.MathLibrary.Native.BLAS`

#### 1. Overview
Provides wrapper for the [BLAS](http://www.netlib.org/blas/) Library (Fortran interface). The native dll 
should be named `libBLAS.dll` to ensure that it can be found by `Dodoni.MathLibrary.Native.BLAS`.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](http://en.wikipedia.org/wiki/Managed_Extensibility_Framework)(MEF) is used to dynamic link to this assembly. 
If specific features of this assembly are used which are not covered by the generic interface one has to add a reference to this assembly; 
in all other cases a reference to this assembly is optional. One may have a look in the unit test project 
of `Dodoni.MathLibrary.Native.BLAS`; see API documentation for more information.


