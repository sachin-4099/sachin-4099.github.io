---
layout: post
title: GSoC 2020 - Final Report
comments: true
tags: [ gsoc, SymPy]
---

This report summarises the work I have done during GSoC 2020 for SymPy. The links to the PRs are in chronological order. For following the progress made during GSoC, see my [blog](https://sachin-4099.github.io/) for the weekly posts.

## About Me

I am Sachin Agarwal, a third-year undergraduate student majoring in Computer Science and Engineering from the Indian Institute of Information Technology, Guwahati.

## Project Synopsis

My project involved fixing and amending functions related to series expansions and limit evaluation. For further information, my [proposal](https://drive.google.com/file/d/1OgbnWLzQzaLfmmSM-fK09TCJmUzJ6tq4/view) for the project can be referred. 

## Pull Requests

This section describes the actual work done during the coding period in terms of **merged PRs**.

#### Phase 1

- [#19292](https://github.com/sympy/sympy/pull/19292) : This PR added a condition to `limitinf` method of `gruntz.py` resolving incorrect limit evaluations.

- [#19297](https://github.com/sympy/sympy/pull/19297) : This PR replaced `xreplace()` with `subs()` in `rewrite` method of `gruntz.py` resolving incorrect limit evaluations.

- [#19369](https://github.com/sympy/sympy/pull/19369) : This PR fixed `_eval_nseries` method of `mul.py`.

- [#19432](https://github.com/sympy/sympy/pull/19432) : This PR added a functionality to the `doit` method of `limits.py` which uses `is_meromorphic` for limit evaluations.

- [#19461](https://github.com/sympy/sympy/pull/19461) : This PR corrected the `_eval_as_leading_term` method of `tan` and `sec` functions.

- [#19508](https://github.com/sympy/sympy/pull/19508) : This PR fixed `_eval_nseries` method of `power.py`.

- [#19515](https://github.com/sympy/sympy/pull/19515) : This PR added `_eval_rewrite_as_factorial` and `_eval_rewrite_as_gamma` methods to `class subfactorial`.


#### Phase 2

- [#19555](https://github.com/sympy/sympy/pull/19555) : This PR added `cdir` parameter to handle `series expansions` on `branch cuts`.

- [#19646](https://github.com/sympy/sympy/pull/19646) : This PR rectified the `mrv` method of `gruntz.py` and `cancel` method of `polytools.py` resolving `RecursionError` and `Timeout` in limit evaluations.

- [#19680](https://github.com/sympy/sympy/pull/19680) : This PR added `is_Pow` heuristic to `limits.py` to improve the limit evaluations of `Pow objects`.

- [#19697](https://github.com/sympy/sympy/pull/19697) : This PR rectified `_eval_rewrite_as_tractable` method of `class erf`.

- [#19716](https://github.com/sympy/sympy/pull/19716) : This PR added `_singularities` to `LambertW` function.

- [#18696](https://github.com/sympy/sympy/pull/18696) : This PR fixed errors in assumptions when rewriting `RisingFactorial` / `FallingFactorial` as `gamma` or `factorial`.


#### Phase 3

- [#19741](https://github.com/sympy/sympy/pull/19741) : This PR reduced `symbolic multiples of pi` in `trigonometric functions`.

- [#19916](https://github.com/sympy/sympy/pull/19916) : This PR added `_eval_nseries` method to `sin` and `cos`.

- [#19963](https://github.com/sympy/sympy/pull/19963) : This PR added `_eval_is_meromorphic` method to the `class BesselBase`.

- [#18656](https://github.com/sympy/sympy/pull/18656) : This PR added `Raabe's Test` to the `concrete` module.

- [#19990](https://github.com/sympy/sympy/pull/19990) : This PR added `_eval_is_meromorphic` and `_eval_aseries` methods to `class lowergamma`, 
  `_eval_is_meromorphic` and `_eval_rewrite_as_tractable` methods to `class uppergamma` and rectified the `eval` method of `class besselk`.

- [#20002](https://github.com/sympy/sympy/pull/20002) : This PR fixed `_eval_nseries` method of `log`.


## Miscellaneous Work

This section contains some of my PRs related to miscellaneous issues.

- [#19447](https://github.com/sympy/sympy/pull/19447) : This PR added some required testcases to `test_limits.py`.

- [#19537](https://github.com/sympy/sympy/pull/19537) : This PR fixed a minor `performance` issue.

- [#19604](https://github.com/sympy/sympy/pull/19604) : This PR fixed `AttributeError` in limit evaluation.


## Reviewed Work

This section contains some of the PRs which were reviewed by me.

- [#19546](https://github.com/sympy/sympy/pull/19546)
- [#19660](https://github.com/sympy/sympy/pull/19660)
- [#19730](https://github.com/sympy/sympy/pull/19730)
- [#19737](https://github.com/sympy/sympy/pull/19737)
- [#19743](https://github.com/sympy/sympy/pull/19743)
- [#19771](https://github.com/sympy/sympy/pull/19771)
- [#19805](https://github.com/sympy/sympy/pull/19805)
- [#19824](https://github.com/sympy/sympy/pull/19824)
- [#19876](https://github.com/sympy/sympy/pull/19876)
- [#19922](https://github.com/sympy/sympy/pull/19922)
- [#19974](https://github.com/sympy/sympy/pull/19974)

## Issues Opened

This section contains some of the issues which were opened by me.

- [#19670](https://github.com/sympy/sympy/issues/19670) : `Poly(E**100000000)` is slow to create.
- [#19752](https://github.com/sympy/sympy/issues/19752) : `gammasimp` can be improved for `integer` variables.

## Examples

This section describes the bugs fixed and the new features added during GSoC.

#### Fixed Limit Evaluations

```py
>>> from sympy import limit, limit_seq

>>> n = Symbol('n', positive=True, integer=True)
>>> limit(factorial(n + 1)**(1/(n + 1)) - factorial(n)**(1/n), n, oo)
exp(-1)  # Previously produced 0

>>> n = Symbol('n', positive=True, integer=True)
>>> limit(factorial(n)/sqrt(n)*(exp(1)/n)**n, n, oo)
sqrt(2)*sqrt(pi)  # Previously produced 0 

>>> n = Symbol('n', positive=True, integer=True)
>>> limit(n/(factorial(n)**(1/n)), n, oo)
exp(1)  # Previously produced oo 

>>> limit(log(exp(3*x) + x)/log(exp(x) + x**100), x, oo)
3  # Previously produced 9

>>> limit((2*exp(3*x)/(exp(2*x) + 1))**(1/x), x, oo)
exp(1)  # Previously produced exp(7/3)

>>> limit(sin(x)**15, x, 0, '-')
0  # Previously it hanged

>>> limit(1/x, x, 0, dir="+-")
zoo  # Previously raised ValueError

>>> limit(gamma(x)/(gamma(x - 1)*gamma(x + 2)), x, 0)
-1  # Previously it was returned unevaluated

>>> e = (x/2) * (-2*x**3 - 2*(x**3 - 1) * x**2 * digamma(x**3 + 1) + 2*(x**3 - 1) * x**2 * digamma(x**3 + x + 1) + x + 3)
>>> limit(e, x, oo)
1/3  # Previously produced 5/6

>>> a, b, c, x = symbols('a b c x', positive=True)
>>> limit((a + 1)*x - sqrt((a + 1)**2*x**2 + b*x + c), x, oo)
-b/(2*a + 2)  # Previously produced nan

>>> limit_seq(subfactorial(n)/factorial(n), n)
1/e  # Previously produced 0

>>> limit(x**(2**x*3**(-x)), x, oo)
1  # Previously raised AttributeError

>>> limit(n**(Rational(1, 1e9) - 1), n, oo)
0  # Previously it hanged

>>> limit((1/(log(x)**log(x)))**(1/x), x, oo)
1  # Previously raised RecursionError

>>> e = (2**x*(2 + 2**(-x)*(-2*2**x + x + 2))/(x + 1))**(x + 1)
>>> limit(e, x, oo)
exp(1)  # Previously raised RecursionError

>>> e = (log(x, 2)**7 + 10*x*factorial(x) + 5**x) / (factorial(x + 1) + 3*factorial(x) + 10**x)
>>> limit(e, x, oo)
10  # Previously raised RecursionError

>>> limit((x**2000 - (x + 1)**2000) / x**1999, x, oo)
-2000  # Previously it hanged 

>>> limit(((x**(x + 1) + (x + 1)**x) / x**(x + 1))**x, x, oo)
exp(exp(1))  # Previously raised RecursionError

>>> limit(Abs(log(x)/x**3), x, oo)
0  # Previously it was returned unevaluted

>>> limit(x*(Abs(log(x)/x**3)/Abs(log(x + 1)/(x + 1)**3) - 1), x, oo)
3  # Previously raised RecursionError 

>>> limit((1 - S(1)/2*x)**(3*x), x, oo)
zoo  # Previously produced 0

>>> d, t = symbols('d t', positive=True)
>>> limit(erf(1 - t/d), t, oo)
-1  # Previously produced 1

>>> s, x = symbols('s x', real=True)
>>> limit(erf(s*x)/erf(s), s, 0)
x  # Previously produced 1

>>> limit(erfc(log(1/x)), x, oo)
2  # Previously produced 0

>>> limit(erf(sqrt(x)-x), x, oo)
-1  # Previously produced 1

>>> a, b = symbols('a b', positive=True)
>>> limit(LambertW(a), a, b) 
LambertW(b)  # Previously produced b

>>> limit(uppergamma(n, 1) / gamma(n), n, oo)
1  # Previously produced 0

>>> limit(besselk(0, x), x, oo)
0  # Previously produced besselk(0, oo)
```

#### Rewrote Mul._eval_nseries()

```py
>>> e = (exp(x) - 1)/x
>>> e.nseries(x, 0, 3)
1 + x/2 + x**2/6 + O(x**3)  # Previously produced 1 + x/2 + O(x**2, x)

>>> e = (2/x + 3/x**2)/(1/x + 1/x**2)
>>> e.nseries(x, n=3)
3 - x + x**2 + O(x**3)  # Previously produced 3 + O(x)
```

#### Rewrote Pow._eval_nseries()

```py
>>> e = (x**2 + x + 1) / (x**3 + x**2)
>>> series(e, x, oo)
x**(-5) - 1/x**4 + x**(-3) + 1/x + O(x**(-6), (x, oo))  
# Previously produced x**(-5) + x**(-3) + 1/x + O(x**(-6), (x, oo))

>>> e = (1 - 1/(x/2 - 1/(2*x))**4)**(S(1)/8)
>>> e.series(x, 0, n=17)
1 - 2*x**4 - 8*x**6 - 34*x**8 - 152*x**10 - 714*x**12 - 3472*x**14 - 17318*x**16 + O(x**17)  
# Previously produced 1 - 2*x**4 - 8*x**6 - 34*x**8 - 24*x**10 + 118*x**12 - 672*x**14 - 686*x**16 + O(x**17) 
```

#### Added Series Expansions and Limit evaluations on Branch-Cuts

```py
>>> asin(I*x + I*x**3 + 2)._eval_nseries(x, 3, None, 1)
-asin(2) + pi - sqrt(3)*x/3 + sqrt(3)*I*x**2/9 + O(x**3)

>>> limit(log(I*x - 1), x, 0, '-')
-I*pi
```

#### Rectified ff._eval_rewrite_as_gamma() and rf._eval_rewrite_as_gamma()

```py
>>> n = symbols('n', integer=True)
>>> combsimp(RisingFactorial(-10, n))
3628800*(-1)**n/factorial(10 - n)  # Previously produced 0

>>> ff(5, y).rewrite(gamma)
120/Gamma(6 - y)  # Previously produced 0
```

#### Added Raabe's Test

```py
>>> Sum(factorial(n)/factorial(n + 2), (n, 1, oo)).is_convergent()
True  # Previously raised NotImplementedError

>>> Sum((-n + (n**3 + 1)**(S(1)/3))/log(n), (n, 1, oo)).is_convergent()
True  # Previously raised NotImplementedError
```

#### Rewrote log._eval_nseries()

```py
>>> f = log(x/(1 - x))
>>> f.series(x, 0.491, n=1).removeO()
-0.0360038887560022  # Previously raised ValueError
```

## Future Work

- Refactoring high level functions like `series`, `nseries`, `lseries` and `aseries`.
- Add `_eval_is_meromorphic` method or `_singularities` to as many special functions as possible.
- Work can be done to resolve the issues opened by me (listed above).


## Conclusion

This summer has been a great learning experience and has helped me get a good exposure of test-driven development. I plan to continue contributing to SymPy and will also try to help the new contributors. I am grateful to my mentors, [Kalevi Suominen](https://github.com/jksuom) and [Sartaj Singh](https://github.com/leosartaj) for reviewing my work, giving me valuable suggestions, and being readily available for discussions.
