## {{Dodoni.MathLibrary.Native.ACML}}

#### 1. Overview
Provides wrapper for the [Core Math Library (ACML)](http://developer.amd.com/tools-and-sdks/cpu-development/amd-core-math-library-acml/), a library that provides a free set of thoroughly optimized and threaded math routines for HPC, scientific, engineering and related compute-intensive applications. The main native dll of the ACML Library should be renamed to {{libACML.dll}} to ensure that it can be found by {{Dodoni.MathLibrary.Native.ACML}}.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](http://en.wikipedia.org/wiki/Managed_Extensibility_Framework) (MEF) is used to dynamic link to this assembly. If specific features of this assembly are used which are not covered by the generic interface one has to add a reference to this assembly; in all other cases a reference to this assembly is optional. This is true for Fast Fourier Transformations only.

The following code snippets shows how random number can be generated as a simple example.
{code:C#}
int sampleSize = 100;
double a = 0.0;
double b = 1.0;
var library = new AcmlRandomNumberLibrary();
var sample = new double[sampleSize](sampleSize);

IRandomNumberStream randomStream = library.MT19937.Create(12);
// or: var randomStream = AcmlPseudoRandomNumberGenerator.MT19937.Create(12);
randomStream.NextNumberSequence.Uniform(sampleSize, sample, a, b);
{code:C#}

One may have a look in the unit test project of {{Dodoni.MathLibrary.Native.ACML}}; see API documentation for more information.

#### 4. Important remarks
AMD discontinued support for 32-bit ACML. Many tests have been made with ACML 4.4 which seems to be the latest 32-bit release. 


