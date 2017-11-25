## {{Dodoni.BasicMathLibrary}}

#### 1. Overview
Provides _mainly the infrastructure_ and some basic implementations for mathematical operations, for example:
* [BLAS](http://www.netlib.org/blas/) library (interface structure + a managed fallback implementation), 
* [LAPACK](http://www.netlib.org/lapack/) library (wrapper for native code), 
* generic interface for Fast-Fourier-Transformations (FFT) (+ a dummy managed fallback implementation),
* Vector operations (interface structure + a managed fallback implementation),
* interfaces for [Special functions](http://en.wikipedia.org/wiki/List_of_mathematical_functions),
* interfaces for interpolation and parametrization of curves and surfaces with some basic implementations (linear, spline etc.)
* interfaces for numerical integration, Random Number Generators, Optimization etc.
* implementation for root finding algorithms for polynomials etc.
This enables to incorporate the functionality of 3th party mathematical libraries, as for example [Math Kernel Library](http://en.wikipedia.org/wiki/Math_Kernel_Library) (MKL), [AMD Core Math Library](http://en.wikipedia.org/wiki/AMD_Core_Math_Library) (ACML), [Fastest Fourier Transform in the West](http://www.fftw.org/) (FFTW), [NLopt](http://ab-initio.mit.edu/wiki/index.php/NLopt), [Yeppp!](http://www.yeppp.info/) etc. See namespace 
:: {{Dodoni.MathLibary.Native.<Name of 3th party Library>}} 
for a specific wrapper. The [Managed Extensibility Framework](http://en.wikipedia.org/wiki/Managed_Extensibility_Framework) (MEF) is used to dynamic link some of the external mathematical libraries to the Dodoni.net framework.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](http___en.wikipedia.org_wiki_Managed_Extensibility_Framework) (MEF) is used to dynamic link some of the external mathematical libraries to the Dodoni.net framework. Therefore one has to apply the {{Export}} attribute of the MEF framework to an individual implementation, for example

{code:c#}
 [Export(typeof(BLAS.ILibrary))](Export(typeof(BLAS.ILibrary)))
{code:c#}

in the case of a specific BLAS implementation. Moreover one should derive from class
{code:c#}
MarshalByRefObject
{code:c#}

The method {{LowLevelMathConfiguration.BLAS.Libraries.GetValues}} returns all available BLAS libraries and one should apply {{LowLevelMathConfiguration.BLAS.Setup}} to store the BLAS library in the configuration file which should be use. Do not forget to call {{LowLevelMathConfiguration.WriteConfigFile}} to update the configuration file. The same procedure holds for other libraries than BLAS (i.e. FFT, VectorUnit, SpecialFunction(s) etc.). One may have a look in the unit test project of Dodoni.BasicMathLibrary; see API documentation for more information.

 **BLAS**
Provides the Basic Linear Algebra Subprograms, see [http://www.netlib.org/blas](http://www.netlib.org/blas) for further information.The names of the methods are almost identical to the BLAS naming convention. We restrict to double precision and complex numbers. Moreover the infrastructure assumes the convention of the Fortran interface only, i.e. matrices for example are provided column-by-column.

{code:c#}
  int n = 5;
  double[]() x = ...
  BLAS.Level1.dscale(n,-1.0,x);  // x = (-1.0) * x
{code:c#}

 **LAPACK**
Provides the Linear Algebra PACKage, see [http://www.netlib.org/lapack/index.html](http://www.netlib.org/lapack/index.html) for further information. The names of the methods are almost identical to the LAPACK naming convention. _Not all LAPACK functions are implemented yet._

{code:c#}
 // Cholesky decomposition:
int n = ...
double[]() a = 
LAPACK.LinearEquations.MatrixFactorization.dpotrf( BLAS.TriangularMatrixType.LowerTriangularMatrix, n, a); 
{code:c#}

 **FFT** {anchor:FFT}
Serves as factory for (1-dimensional) Fast-Fourier transformations, i.e. 

:: H,,n,, = a * \sum_{k=0}^{N-1} h,,k,, * exp( -/+ 2 \pi * i * k * n * \alpha), 
where \alpha=1/N for a ordinary (Fast) Fourier transformation and \alpha arbritrary for a Fractional (Fast) Fourier transformation. 

{code:c#}
  int n = 16;
  var coefficients = new Complex[n](n);  // input
  
  var fft = FFT.OneDimensional.Create(n);
  fft.ForwardTransformation(coefficients);    // in-place
{code:c#}

 **VectorUnit** {anchor:VectorUnit}
Provides functions for Vector units, i.e. methods applied to arrays of floating point numbers, complex numbers etc. 

{code:c#}
  int n = 7;
  var a = new double[n](n);  // input
  var b = new double[n](n);  // input
  var y = new double[n](n);  // output, i.e. y = a + b

  VectorUnit.Basics.Add(n,a,b,y);
{code:c#}

 **SpecialFunction** {anchor:SpecialFunction}
Provides [Special functions](http://en.wikipedia.org/wiki/List_of_mathematical_functions), i.e.  mathematical functions with specific names, as for example erf(x) (error function), ,,1,,F,,1,, (Hypergeometric function) etc. The assembly {{Dodoni.BasicMathLibrary}} does not contain implementations for special functions, except for the (inverse) cumulative distribution function of the Standard normal distribution which is accessible via the class {{StandardNormalDistribution}} only. Use the [Managed Extensibility Framework](http___en.wikipedia.org_wiki_Managed_Extensibility_Framework) (MEF) as described above to dynamically link to some external mathematical library that implements {{SpecialFunction.ILibrary}}. The assembly [Dodoni.MathLibrary](MathLibrary) contains a partial implementation of this interface.

{code:c#}
 SpecialFunction.PrimitiveIntegral.Erf(x);
{code:c#}

 **Curve construction** {anchor:CurveConstruction}
The class {{GridPointCurve}} serves as factory for the curve construction. One has to enter the interpolation approach as well as the extrapolation approaches or the curve parametrization. Moreover a specific label can be added for each grid point argument (x-value). This label can be an arbritrary type, for example a {{string}}. In the following example no specific label is provided - one should replace {{GridPointCurve}} by its generic class, for example {{GridPointCurve<string>}} to incoporate string labels. 
**important**: One has to call the {{Update}} method before fetching values and after adding or changing grid points.

{code:c#}
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
{code:c#}

 **Surface construction** {anchor:SurfaceConstruction}
The class {{GridPointSurface2d}} serves as factory for two-dimensional surface construction. Often one has a matrix of points with some missing values. Therefore one first has to create a {{LabelMatrix}} object which can fill missing values.

{code:c#}
 double[]()() matrix = new double[]()() { 1, 2, 3, 4, Double.NaN, 6, 7, 8, 9 };
// matrix is provided column-by-column
 int rowCount = 3;
 int columnCount = 3;
 var xLabels = new double[]() { 1, 2, 3 };
 var yLabels = new double[]() { 1, 2, 3 };

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
{code:c#}

 **Polynomial**
The class {{Polynomial}} serves as factory for polynomials with complex or real coefficients. Therefore it can be used to calculate (complex or real) roots of a specific polynomial.

{code:c#}
var polynomial = Polynomial.Complex.Create(degree, coefficients);
var roots = new List<Complex>();
polynomial.GetRoots(roots, Polynomial.RootFinder.EigenvalueApproach);
{code:c#}

If roots can be computed analytically, it is _not_ necessary to create a {{IPolynomial}} object first, {{IRealPolynomial}} respectively:

{code:c#}
var roots = new Complex[4](4);
int rootCount = Polynomial.RootFinder.Analytical.GetRoots(
                     absoluteCoefficient, firstOrderCoefficient, 
                     secondOrderCoefficient, thirdOrderCoefficient, 
                     fourthOrderCoefficient, 
                     out roots[0](0), out roots[1](1), out roots[2](2), out roots[3](3));
{code:c#}

 **MultiDimOptimizer, OneDimOptimizer**
Provides the infrastructure for optimization for 1- and multi-dimensional problems, but no specific implementations for it. 

{code:c#}
var opt = new BrentOptimizer(); // from Dodoni.MathLibrary
var optAlgorithm = optimizer.Create(Interval.Create(lowerBound, upperBound));

optAlgorithm.Function = opt.Function.Create(x => (x - 1.0) * (x - 1.0));
// or: optAlgorithm.SetFunction(x => (x - 1.0) * (x - 1.0));

double actualArgMin, actualMinimum;
var state = optAlgorithm.FindMinimum(initialGuess, out actualArgMin, out actualMinimum);
{code:c#}
For the 1-dimensional case {{OneDimOptimizer}} serves as factory for {{IOneDimOptimizerAlgorithm}} objects that encapsulates the algorithm with respect to a specific constraint. In the multi-dimensional case it is rather complex: {{MultiDimOptimizer}} is the abstract base class for
* {{OrdinaryMultiDimOptimizer}}: min,,x,, f(x), where f is a real-valued function,
* {{MultivariateOptimizer}}: min,,x,, ||f(x)||^^2^^, where f(x) = (f,,1,,(x),...,f,,m,,(x)) is a multivariate function,
* {{QuadraticProgram}}: min,,x,, 1/2 * x' * A * x + b' * x.
A object of the above type contains factories for constraints, objective functions as well as to {{IMultiDimOptimizerAlgorithm}} objects. The latter represents the algorithm itself. The internal representation of objective functions and constraints could be different for each implementation, therefore a individual factory is required. Some extension methods have been added, for example {{SetFunction}}, which allows a intiutive use. {{MultiDimRegion}} and {{Interval}} are factories for _generic_ regions that should be converted into the specific {{Multi/OneDimOptimizer.IConstraint}} representation in a way similar seen in the above code snippet. The framework should be able to cover arbitrary optimization algorithms.

 **OneDimNumericalConstAbscissaIntegrator vs. OneDimNumericalIntegrator**
{{OneDimNumericalIntegrator}} and {{OneDimNumericalConstAbscissaIntegrator}} serves as abstract basis class for numerical integration. The latter assumes a constant set of abscissa to evaluate the specified function. This can be used to accelerate the calculation in the case that one has to apply the numerical integration to almost the same function several times - one can cache part of the calculation. One example are Gaussian quadrature formulas with a specific order. Adaptive approaches are not well suited for this approach. The assembly [Dodoni.MathLibrary](MathLibrary) contains some implementations for both approaches.

 **RandomNumberLibrary**
Moreover, the assemly {{Dodoni.BasicMathLibrary}} provides the infrastructure for random number generation etc., but no specific implementations for it. 


