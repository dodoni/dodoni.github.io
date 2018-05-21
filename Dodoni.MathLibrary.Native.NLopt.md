## `Dodoni.MathLibrary.Native.NLopt`

#### 1. Overview
Provides wrapper for the [NLopt library](http://ab-initio.mit.edu/wiki/index.php/NLopt), a free/open-source library for nonlinear optimization, 
established by Steven G. Johnson. You need to download at least a precompiled release of the library. Copy the file(s) to the binary folder 
of your .net code and rename it to `libNLopt.dll`.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
One may have a look in the unit test project of `Dodoni.MathLibrary.Native.NLopt`; see API documentation for more information.

 **NLoptPtr**

The class `NLoptPtr` serves as wrapper for the internal opaque pointer of the algorithm of the [NLopt library](http://ab-initio.mit.edu/wiki/index.php/NLopt). 
Therefore you will find for almost all function of the API of the [NLopt library](http://ab-initio.mit.edu/wiki/index.php/NLopt) a counterpart in this class. 

``` csharp
 NLoptPtr ptr = new NLoptPtr(NLoptAlgorithm.LN_PRAXIS, 2);

 ptr.TrySetAbsoluteFValueTolerance(1E-6);
 ptr.TrySetRelativeFValueTolerance(1E-6);
 ptr.TrySetAbsoluteXTolerance(1E-6);
 ptr.TrySetRelativeXTolerance(1E-6);

 ptr.SetFunction((n, x, gradient, data) => { return x[0](0)(0) * x[0](0)(0) + x[1](1)(1) * x[1](1)(1) + 1.123; });

 var argMin = new double[2](2) { 1.0, 4.8 };
 double minimum; // expected: 1.123
 var errorCode = ptr.FindMinimum(argMin, out minimum);
```

 **NLoptMultiDimOptimizer**

In contrast to `NLoptPtr` the class `NLoptMultiDimOptimizer` implements the more common class `(Ordinary)MultiDimOptimizer` 
of [Dodoni.BasicMathLibrary](BasicMathLibrary) which serves as general infastructure for multi-dimensional optimization. 
Therefore one first create a specific `(NLopt)MultiDimOptimizer` object. 
Constraints and the representation of the objective function are specific for the [NLopt library](http://ab-initio.mit.edu/wiki/index.php/NLopt). 
Therefore one has to apply the specific factory for it. The following code snippet shows a simple example.

``` csharp
 var cobyla = new NLoptMultiDimOptimizer(NLoptAlgorithm.LN_COBYLA);
 var nloptBoxConstraint = cobyla.Constraint.Create(
                           MultiDimRegion.Interval.Create(dimension: 2, 
                           lowerBounds: new[]() { 1.0, 5.0 }, 
                           upperBounds: new[]() { 12.4, 34.2 }));

 var alg = cobyla.Create(nloptBoxConstraint);
 alg.Function = cobyla.Function.Create(2,x => x[0](0)(0)**x[0](0)(0) + x[1](1)(1)**x[1](1)(1) + 1.123);
// alternative: alg.SetFunction(x => x[0](0)(0)**x[0](0)(0) + x[1](1)(1)**x[1](1)(1) + 1.123);

 var argMin = new []() { 1.4, 5.8 };
 double minimum;
 var errorCode = alg.FindMinimum(argMin, out minimum);
```

Moreover a logging can be used via an optional argument of `NLoptMultiDimOptimizer`. You need more freedom in the adjustment of the specific algorithm? 
No problem, the constructors of `NLoptMultiDimOptimizer` provides an optional argument that is a delegate (function pointer) which 
will be applied to the internal `NLoptPtr` object. Therefore one can apply lambda calculus, for example to establish a local optimizer, 
set initial step size etc. 

``` csharp
 var cobyla = new NLoptMultiDimOptimizer(NLoptAlgorithm.LN_COBYLA,
      nloptPtr => {nloptPtr.SetInitialStepSize(new [](){1.0, 2.0}} );
```

**Keep in mind** that the lambda expression will be evaluated in the `Create` method of `NLoptMultiDimOptimizer`, in particular the dimension of the 
feasible set is unknown in the constructor of the `NLoptMultiDimOptimizer` class, but the `nloptPtr` object in the lambda expresion above contains 
a property with the specific dimension.

 **NLoptConfiguration**
If you need additional information of a specific NLopt algorithm, i.e. whether it is a local/global approach, required gradient etc. you 
can use the `Configuration` property of `NLoptMultiDimOptimizer`. Moreover you can create such a configuration separately, for example via

``` csharp
 var config = NLoptConfiguration.Create(NLoptAlgorithm.LN_COBYLA);
```


#### 4. Important Remarks
It has been tested with the 32-bit Windows dll, release 2.4.1.


