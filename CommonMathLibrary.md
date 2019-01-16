## `Dodoni.CommonMathLibrary`

#### 1. Overview
The assembly `Dodoni.CommonMathLibrary` contains managed implementations of several mathematikcal operations, for example:

*  numerical integration algorithms,
*  some curve/surface interpolation approaches,
*  some optimization algorithm etc.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
One may have a look in the unit test project of `Dodoni.CommonMathLibrary`; see API documentation for more information.

 **Numerical integrator**
The following code snippet shows how to calculate numerically a specific integral with the 
[https://en.wikipedia.org/wiki/Gauss%E2%80%93Kronrod_quadrature_formula](Gauss-Kronrod-Patterson) approach. 
The default constructor takes some default values for the abort condition.

``` csharp
var gaussKronrodPatterson = new GaussKronrodPatterson255Integrator();
var integrator = gaussKronrodPatterson.Create();

var lowerBound = 1.0;
var upperBound = 10.0;

integrator.TrySetBounds(lowerBound, upperBound);
integrator.FunctionToIntegrate = x => x * x;

var value = integrator.GetValue();
```


 **Optimizer**
The general infastructure for (1- and n-dimensional) optimization algorithms can be found in 
[Dodoni.BasicMathLibrary](BasicMathLibrary). The following code snippets shows a simple example how a 1-dimensional optimizer can be used:

``` csharp
var optimizer = new GoldenSectionSearchOptimizer();
var algorithm = optimizer.Create(Interval.Create(lowerBound, upperBound));

agorithm.Function = optimizer.Function.Create(x => (x - 1.0) * (x - 1.0));

var state = optimizerAlgorithm.FindMinimum(initialGuess, out double actualArgMin, out double actualMinimum);
```

n-dimensional optmization algorithms are more complex, especially for constraints, gradient etc. We present an example with the 
[https://en.wikipedia.org/wiki/Test_functions_for_optimization](Goldstein Price function) only:
``` csharp
var optimizer = new NelderMeadOptimizer(
       NelderMeadOptimizer.StandardAbortCondition,
       MultiDimOptimizerConstraintProvider.BoxTransformation);

var constraint = optimizer.Constraint.Create(
                          MultiDimRegion.Interval.Create(2, 
                                new[]{ -2.0, -2.0 }, new[] { 2.0, 2.0 }));

var optimizerAlgorithm = optimizer.Create(constraint);

optimizerAlgorithm.Function = optimizer.Function.Create(2, z =>
        {
          var x = z[0];
          var y = z[1];
          return (1.0 + Math.Pow(x + y + 1.0, 2) * (19.0 - 14.0 * x + 3 * x * x - 14.0 * y + 6.0 * x * y + 3.0 * y * y)) * (30.0 + Math.Pow(2.0 * x - 3.0 * y, 2) * (18.0 - 32.0 * x + 12.0 * x * x + 48.0 * y - 36 * x * y + 27 * y * y));
         });

/* take an initial guess which is not extremly fare away from the argMin: */
var argMin = new[]{0.25, -0.7};

double minimum;  // expected: 3; argMin = {0.0, -1.0}
var state = optimizerAlgorithm.FindMinimum(argMin, out minimum);
```



