## {{Dodoni.FinanceBasics}}

#### 1. Overview
Provides interfaces for day count conventions, business day conventions, holiday calendars, Market convention templates etc. Moreover it contains some Tenor arithmetic, Compounding rules, date factory etc. as well as an implementation of the Black-Scholes and the Bachelier (=normal Black) model etc. This assembly does not contain market instruments or specific models.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
One may have a look in the unit test project of Dodoni.FinanceBasics; see API documentation for more information.

 **TenorTimeSpan**
 
{code:c#}
 var tenorTimeSpan = new TenorTimeSpan("1Y6M");
 var date = DateTime.Today.AddTenorTimeSpan(tenorTimeSpan);
{code:c#}

{anchor:Compounding}
 **Compounding**

{code:c#}
double continuouslyZeroRate = 0.05;

double semiAnnuallyzeroRate = Compounding.GetConvertedZeroRate(
                                              continuouslyZeroRate,
                                              Compounding.ZeroRate.Continuously,
                                              Compounding.ZeroRate.SemiAnnually);
{code:c#}

{code:c#}
double monthlyInterestRate = 0.05; // i.e. Interest = Notional * (1.0 + r_in /12)^{12 * t}

double quarterlyInterestRate = Compounding.GetConvertedInterestRate(
                                           continuouslyInterestRate, 
                                           Compounding.InterestRate.Monthly, 
                                           Compounding.InterestRate.Quarterly, 
                                           interestPeriodLength
                                        ); // i.e. Interest = Notional * (1.0 + r_out/4)^{4 * t}
{code:c#}

{anchor:Conventions}
 **Conventions**

