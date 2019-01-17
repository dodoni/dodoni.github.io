## `Dodoni.BasicMathLibrary`

#### 1. Overview
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

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](https://docs.microsoft.com/en-us/dotnet/framework/mef/index)(MEF) is used to dynamic link some of the external mathematical libraries to the Dodoni.net framework. Therefore one has to apply the `Export` attribute of the MEF framework to an individual implementation, for example

``` csharp
 [Export(typeof(BLAS.ILibrary))]
```

in the case of a specific BLAS implementation. 

One should apply `LowLevelMathConfiguration.BLAS.Setup` to store the BLAS library in the configuration file which should be use. 
Do not forget to call `LowLevelMathConfiguration.WriteConfigFile` to update the configuration file. 
The same procedure holds for other libraries than BLAS (i.e. FFT, VectorUnit, SpecialFunction(s) etc.). One may have a look in the unit test project of 
Dodoni.BasicMathLibrary; see API documentation for more information.

 **BLAS**
Provides the Basic Linear Algebra Subprograms, see [http://www.netlib.org/blas](http://www.netlib.org/blas). 
The names of the methods are almost identical to the BLAS naming convention. We restrict to double precision and complex numbers. Moreover the infrastructure assumes the convention of the Fortran interface only, i.e. matrices for example are provided column-by-column.

``` csharp
  int n = 5;
  double[] x = ...
  BLAS.Level1.dscale(n,-1.0,x);  // x = (-1.0) * x
```

 **LAPACK**
Provides the Linear Algebra PACKage, see [http://www.netlib.org/lapack/index.html](http://www.netlib.org/lapack/index.html). 
The names of the methods are almost identical to the LAPACK naming convention. _Not all LAPACK functions are implemented yet._

``` csharp
 // Cholesky decomposition:
int n = ...
double[] a = 
LAPACK.LinearEquations.MatrixFactorization.dpotrf(BLAS.TriangularMatrixType.LowerTriangularMatrix, n, a); 
```

 **FFT** 
Serves as factory for (1-dimensional) Fast-Fourier transformations, i.e. 

$$H_n = a * \sum_{k=0}^{N-1} h_k * exp( -/+ 2 \pi * i * k * n * \alpha),$$ 
where \alpha=1/N for a ordinary (Fast) Fourier transformation and \alpha arbritrary for a Fractional (Fast) Fourier transformation. 

``` csharp
  int n = 16;
  var coefficients = new Complex[n];  // input
  
  var fft = FFT.OneDimensional.Create(n);
  fft.ForwardTransformation(coefficients);    // in-place
```

 **VectorUnit** 
Provides functions for Vector units, i.e. methods applied to arrays of floating point numbers, complex numbers etc. 

``` csharp
  int n = 7;
  var a = new double[n];  // input
  var b = new double[n];  // input
  var y = new double[n];  // output, i.e. y = a + b

  VectorUnit.Basics.Add(n,a,b,y);
```

 **SpecialFunction** 
Provides [Special functions](http://en.wikipedia.org/wiki/List_of_mathematical_functions), i.e.  mathematical functions with specific names, as for example erf(x) 
(error function), <sub>1</sub>F<sub>1</sub> (Hypergeometric function) etc. 

The assembly `Dodoni.BasicMathLibrary` does not contain implementations for special functions, except for the (inverse) cumulative distribution 
function of the Standard normal distribution which is accessible via the class `StandardNormalDistribution`. 
Use the [Managed Extensibility Framework](https://docs.microsoft.com/en-us/dotnet/framework/mef/index)(MEF) as described above to dynamically 
link to some external mathematical library that implements `SpecialFunction.ILibrary`. 


``` csharp
 SpecialFunction.PrimitiveIntegral.Erf(x);
```

 **Curve construction** 
The class `GridPointCurve` serves as factory for the curve construction. One has to enter the interpolation approach as well as the extrapolation approaches or the curve parametrization. Moreover a specific label can be added for each grid point argument (x-value). This label can be an arbritrary type, for example a `string`. In the following example no specific label is provided - one should replace `GridPointCurve` by its generic class, for example `GridPointCurve<string>` to incoporate string labels. 
**important**: One has to call the `Update` method before fetching values and after adding or changing grid points.

``` csharp
var gridPointCurve = GridPointCurve.Create(
         GridPointCurve.Interpolator.Linear, 
         GridPointCurve.Extrapolator.Linear.SlopeOfFirstTwoGridPoints, 
         GridPointCurve.Extrapolator.Constant.Last);

gridPointCurve.Add(1.5, 2.0);
gridPointCurve.Add(0.5, 1.25);
gridPointCurve.Add(5.75, 9.75);
gridPointCurve.Add(4.0, 12.25);
gridPointCurve.Add(8.0, 7.5);

gridPointCurve.Update();
var value = gridPointCurve.GetValue(3.75);
```

 **Surface construction** 
The class `GridPointSurface2d` serves as factory for two-dimensional surface construction. Often one has a matrix of points with some missing values. Therefore one first has to create a `LabelMatrix` object which can fill missing values.

``` csharp
var matrix = new double[]{ 1, 2, 3, 4, Double.NaN, 6, 7, 8, 9 };
// matrix is provided column-by-column
 int rowCount = 3;
 int columnCount = 3;
 var xLabels = new double[]{ 1, 2, 3 };
 var yLabels = new double[]{ 1, 2, 3 };

 var labelMatrix = LabelMatrix.Create(
                              rowCount, 
                              columnCount, 
                              matrix, 
                              xLabels, 
                              yLabels,
                              LabelMatrix.MissingValueReplenishment.WeightedNearestGridPoints.xAxis.Linear, 
                              orderOfInput: LabelMatrix.OrderOfInput.DisorderedHorizontalLabels);

 var surface = GridPointSurface2d.Create(
                          labelMatrix, 
                          GridPointCurve.Interpolator.Linear, 
                          GridPointCurve.Extrapolator.Constant.First, 
                          GridPointCurve.Extrapolator.Constant.Last, 
                          GridPointCurve.Interpolator.Linear, 
                          GridPointCurve.Extrapolator.Constant.First, 
                          GridPointCurve.Extrapolator.Constant.Last,
                          GridPointSurface2d.ConstructionOrder.HorizontalVertical);
         
double value = surface.GetValue(1.5, 1.5);
```

 **Polynomial**
The class `Polynomial` serves as factory for polynomials with complex or real coefficients. Therefore it can be used to calculate (complex or real) roots of a specific polynomial.

``` csharp
var polynomial = Polynomial.Complex.Create(degree, coefficients);
var roots = new List<Complex>();
polynomial.GetRoots(roots, Polynomial.RootFinder.EigenvalueApproach);
```

If roots can be computed analytically, it is _not_ necessary to create a `IPolynomial` object first, `IRealPolynomial` respectively:

``` csharp
var roots = new Complex[4];
int rootCount = Polynomial.RootFinder.Analytical.GetRoots(
                     absoluteCoefficient, firstOrderCoefficient, 
                     secondOrderCoefficient, thirdOrderCoefficient, 
                     fourthOrderCoefficient, 
                     out roots[0], out roots[1], out roots[2], out roots[3]);
```

 **OneDimOptimizer**
Provides the infrastructure for 1-dimensional optimization problems, but no specific implementations for it. 

``` csharp
var opt = new BrentOptimizer(); // from Dodoni.CommonMathLibrary
var optAlgorithm = optimizer.Create(Interval.Create(lowerBound, upperBound));

optAlgorithm.Function = opt.Function.Create(x => (x - 1.0) * (x - 1.0));
// or: optAlgorithm.SetFunction(x => (x - 1.0) * (x - 1.0));

var state = optAlgorithm.FindMinimum(initialGuess, out double actualArgMin, out double actualMinimum);
```
`OneDimOptimizer` serves as factory for `IOneDimOptimizerAlgorithm` objects that encapsulates the algorithm with respect to a specific constraint. 

 **MultiDimOptimizer**
Provides the infrastructure for optimization for multi-dimensional optimization problems, but no specific implementations for it. 

`MultiDimOptimizer` is the abstract base class for
* `OrdinaryMultiDimOptimizer`: min<sub>x</sub> f(x), where f is a real-valued function,
* `MultivariateOptimizer`: min<sub>x</sub> &#124;&#124;f(x)&#124;&#124;<sup>2</sup>, where f(x) = (f<sub>1</sub>(x),...,f<sub>m</sub>(x)) is a multivariate function,
* `QuadraticProgram`: min<sub>x</sub> 1/2 * x' * A * x + b' * x.

An object of the above type contains factories for constraints, objective functions as well as for `IMultiDimOptimizerAlgorithm` objects. 
The latter represents the algorithm itself. The internal representation of objective functions and constraints could be different 
for each implementation, therefore an individual factory is required. Some extension methods have been added, 
for example `SetFunction`, which allows an more intiutive use. 

``` csharp
var optimizer = new GoldfarbIdanaQuadraticProgram(); // from Dodoni.CommonMathLibrary
var algorithm = optimizer.Create(2);

var A = new DenseMatrix(2, 2, new[] { 4.0, -2.0, -2.0, 4 - 0 });  // = (4 & -2 \\ -2 & 4)
var b = new[] { 6.0, 0.0 };

algorithm.Function = optimizer.Function.Create(A, b);

var argMin = new double[2];
var state = algorithm.FindMinimum(argMin, out double actualMinimum);

```

`MultiDimRegion` and `Interval` are factories for _generic_ regions that should be converted into the specific `Multi/OneDimOptimizer.IConstraint` representation 
in a way similar to the following code snippet: 

``` csharp
 var optimizer = new NelderMeadOptimizer(NelderMeadOptimizer.StandardAbortCondition, MultiDimOptimizerConstraintProvider.BoxTransformation);  // from Dodoni.CommonMathLibrary
 var constraint = optimizer.Constraint.Create(MultiDimRegion.Interval.Create(2, new[] { -2.0, -2.0 }, new[] { 2.0, 2.0 }));
 var optimizerAlgorithm = optimizer.Create(constraint);
 optimizerAlgorithm.Function = optimizer.Function.Create(2, z => // Goldstein Price function
   {
    var x = z[0];
    var y = z[1];
    return (1.0 + Math.Pow(x + y + 1.0, 2) * (19.0 - 14.0 * x + 3 * x * x - 14.0 * y + 6.0 * x * y + 3.0 * y * y)) * (30.0 + Math.Pow(2.0 * x - 3.0 * y, 2) * (18.0 - 32.0 * x + 12.0 * x * x + 48.0 * y - 36 * x * y + 27 * y * y));
   });

/* take an initial guess which is not extremly fare away from the argMin: */
var argMin = new[]{ 0.25, -0.7};
var state = optimizerAlgorithm.FindMinimum(argMin, out double minimum);

```
The framework should be able to cover arbitrary optimization algorithms.

 **OneDimNumericalConstAbscissaIntegrator vs. OneDimNumericalIntegrator**
`OneDimNumericalIntegrator` and `OneDimNumericalConstAbscissaIntegrator` serves as abstract basis class for numerical integration. 

The latter assumes a constant set of abscissa to evaluate the specified function. This can be used to accelerate the calculation in the 
case that one has to apply the numerical integration to almost the same function several times - one can cache part of the calculation. 
One example are Gaussian quadrature formulas with a specific order. 
Adaptive approaches are not well suited for this approach. The assembly [Dodoni.CommonMathLibrary](CommonMathLibrary) contains implementations for both approaches.

 **RandomNumberLibrary**
Moreover, the assemly `Dodoni.BasicMathLibrary` provides the infrastructure for random number generation etc., but no specific implementations for it. 

``` csharp

int sampleSize = 100;
var sample = new double[sampleSize];
IRandomNumberStream randomStream = GetRandomStream();

randomStream.NextNumberSequence.Uniform(sampleSize, sample, 0.0, 1.0);

```
