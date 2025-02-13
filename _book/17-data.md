# Data

There are multiple ways to categorize data. For example,

-   Qualitative vs. Quantitative:

+--------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------+
| Qualitative                                                                                                        | Quantitative                                                                               |
+====================================================================================================================+============================================================================================+
| in-depth interviews, documents, focus groups, case study, ethnography. open-ended questions. observations in words | experiments, observation in words, survey with closed-end questions, structured interviews |
+--------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------+
| language, descriptive                                                                                              | quantities, numbers                                                                        |
+--------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------+
| Text-based                                                                                                         | Numbers-based                                                                              |
+--------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------+
| Subjective                                                                                                         | Objectivity                                                                                |
+--------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------+

## Cross-Sectional

## Time Series

$$
y_t = \beta_0 + x_{t1}\beta_1 + x_{t2}\beta_2 + ... + x_{t(k-1)}\beta_{k-1} + \epsilon_t
$$

Examples

-   Static Model

    -   $y_t=\beta_0 + x_1\beta_1 + x_2\beta_2 - x_3\beta_3 - \epsilon_t$

-   Finite Distributed Lag model

    -   $y_t=\beta_0 + pe_t\delta_0 + pe_{t-1}\delta_1 +pe_{t-2}\delta_2 + \epsilon_t$
    -   **Long Run Propensity (LRP)** is $LRP = \delta_0 + \delta_1 + \delta_2$

-   Dynamic Model

    -   $GDP_t = \beta_0 + \beta_1GDP_{t-1} - \epsilon_t$

<br>

[Finite Sample Properties] for [Time Series]:

-   A1-A3: OLS is unbiased
-   A1-A4: usual standard errors are consistent and [Gauss-Markov Theorem] holds (OLS is BLUE)
-   A1-A6, A6: Finite Sample [Wald Test] (t-test and F-test) are valid

[A3][A3 Exogeneity of Independent Variables] might not hold under time series setting

-   Spurious Time Trend - solvable
-   [Strict][A3 Exogeneity of Independent Variables] vs Contemporaneous Exogeneity - not solvable

In time series data, there are many processes:

-   Autoregressive model of order p: AR(p)
-   Moving average model of order q: MA(q)
-   Autoregressive model of order p and moving average model of order q: ARMA(p,q)
-   Autoregressive conditional heteroskedasticity model of order p: ARCH(p)
-   Generalized Autoregressive conditional heteroskedasticity of orders p and q; GARCH(p.q)

### Deterministic Time trend

Both the dependent and independent variables are trending over time

**Spurious Time Series Regression** 

$$
y_t = \alpha_0 + t\alpha_1 + v_t
$$ 

and x takes the form 

$$
x_t = \lambda_0 + t\lambda_1 + u_t
$$

-   $\alpha_1 \neq 0$ and $\lambda_1 \neq 0$
-   $v_t$ and $u_t$ are independent
-   there is no relationship between $y_t$ and $x_t$

If we estimate the regression, 

$$
y_t = \beta_0 + x_t\beta_1 + \epsilon_t
$$ 

so the true $\beta_1=0$

-   Inconsistent: $plim(\hat{\beta}_1)=\frac{\alpha_1}{\lambda_1}$
-   Invalid Inference: $|t| \to^d \infty$ for $H_0: \beta_1=0$, will always reject the null as $n \to \infty$
-   Uninformative $R^2$: $plim(R^2) = 1$ will be able to perfectly predict as $n \to \infty$

We can rewrite the equation as 

$$
y_t=\beta_0 + \beta_1x_t+\epsilon_t \\
\epsilon_t = \alpha_1t + v_t
$$ 

where $\beta_0 = \alpha_0$ and $\beta_1=0$. Since $x_t$ is a deterministic function of time, $\epsilon_t$ is correlated with $x_t$ and we have the usual omitted variable bias.\
Even when $y_t$ and $x_t$ are related ($\beta_1 \neq 0$) but they are both trending over time, we still get spurious results with the simple regression on $y_t$ on $x_t$

**Solutions to Spurious Trend**

1.  Include time trend t as an additional control

    -   consistent parameter estimates and valid inference

2.  Detrend both dependent and independent variables and then regress the detrended outcome on detrended independent variables (i.e., regress residuals $\hat{u}_t$ on residuals $\hat{v}_t$)

    -   Detrending is the same as partialling out in the [Frisch-Waugh-Lovell Theorem]

        -   Could allow for non-linear time trends by including t $t^2$, and exp(t)
        -   Allow for seasonality by including indicators for relevant "seasons" (quarters, months, weeks).

[A3][A3 Exogeneity of Independent Variables] does not hold under:

-   [Feedback Effect]

    -   $\epsilon_t$ influences next period's independent variables

-   [Dynamic Specification]

    -   include last time period outcome as an explanatory variable

-   [Dynamically Complete]

    -   For finite distrusted lag model, the number of lags needs to be absolutely correct.

### Feedback Effect

$$
y_t = \beta_0 + x_t\beta_1 + \epsilon_t
$$ 

[A3][A3 Exogeneity of Independent Variables] 

$$
E(\epsilon_t|\mathbf{X})= E(\epsilon_t| x_1,x_2, ...,x_t,x_{t+1},...,x_T)
$$ 

will not equal 0, because $y_t$ will likely influence $x_{t+1},..,x_T$

-   [A3][A3 Exogeneity of Independent Variables] is violated because we require the error to be uncorrelated with all time observation of the independent regressors (**strict exogeneity**)

### Dynamic Specification

$$
y_t = \beta_0 + y_{t-1}\beta_1 + \epsilon_t
$$

$$
E(\epsilon_t|\mathbf{X})= E(\epsilon_t| y_1,y_2, ...,y_t,y_{t+1},...,y_T)
$$

will not equal 0, because $y_t$ and $\epsilon_t$ are inherently correlated

-   [A3][A3 Exogeneity of Independent Variables] is violated because we require the error to be uncorrelated with all time observation of the independent regressors (**strict exogeneity**)
-   [Dynamic Specification] is not allowed under [A3][A3 Exogeneity of Independent Variables]

### Dynamically Complete

$$
y_t = \beta_0 + x_t\delta_0 + x_{t-1}\delta_1 + \epsilon_t
$$

$$
E(\epsilon_t|\mathbf{X})= E(\epsilon_t| x_1,x_2, ...,x_t,x_{t+1},...,x_T)
$$ 

will not equal 0, because if we did not include enough lags, $x_{t-2}$ and $\epsilon_t$ are correlated

-   [A3][A3 Exogeneity of Independent Variables] is violated because we require the error to be uncorrelated with all time observation of the independent regressors (strict exogeneity)
-   Can be corrected by including more lags (but when stop? )

Without [A3][A3 Exogeneity of Independent Variables]

-   OLS is biased
-   [Gauss-Markov Theorem]
-   [Finite Sample Properties] are invalid

then, we can

-   Focus on [Large Sample Properties]
-   Can use [A3a] instead of [A3][A3 Exogeneity of Independent Variables]

[A3a] in time series become 

$$
A3a: E(\mathbf{x}_t'\epsilon_t)= 0
$$

only the regressors in this time period need to be independent from the error in this time period (**Contemporaneous Exogeneity**)

-   $\epsilon_t$ can be correlated with $...,x_{t-2},x_{t-1},x_{t+1}, x_{t+2},...$
-   can have a dynamic specification $y_t = \beta_0 + y_{t-1}\beta_1 + \epsilon_t$

Deriving [Large Sample Properties] for Time Series

-   Assumptions [A1][A1 Linearity], [A2][A2 Full rank], [A3a]

-   [Weak Law] and [Central Limit Theorem] depend on [A5][A5 Data Generation (random Sampling)]

    -   $x_t$ and $\epsilon_t$ are dependent over t
    -   without [Weak Law] or [Central Limit Theorem] depend on [A5][A5 Data Generation (random Sampling)], we cannot have [Large Sample Properties] for [OLS][Ordinary Least Squares]
    -   Instead of [A5][A5 Data Generation (random Sampling)], we consider [A5a]

-   Derivation of the Asymptotic Variance depends on [A4][A4 Homoskedasticity]

    -   time series setting introduces **Serial Correlation**: $Cov(\epsilon_t, \epsilon_s) \neq 0$

under [A1][A1 Linearity], [A2][A2 Full rank], [A3a], and [A5a], [OLS estimator][Ordinary Least Squares] is **consistent**, and **asymptotically normal**

### Highly Persistent Data

If $y_t, \mathbf{x}_t$ are not weakly dependent stationary process\
\* $y_t$ and $y_{t-h}$ are not almost independent for large h \* [A5a] does not hold and OLS is not **consistent** and does not have a limiting distribution. \* Example + Random Walk $y_t = y_{t-1} + u_t$ + Random Walk with a drift: $y_t = \alpha+ y_{t-1} + u_t$

**Solution** First difference is a stationary process 

$$
y_t - y_{t-1} = u_t
$$

-   If $u_t$ is a weakly dependent process (also called integrated of order 0) then $y_t$ is said to be difference-stationary process (integrated of order 1)
-   For regression, if $\{y_t, \mathbf{x}_t \}$ are random walks (integrated at order 1), can consistently estimate the first difference equation

$$
\begin{aligned}
y_t - y_{t-1} &= (\mathbf{x}_t - \mathbf{x}_{t-1}\beta + \epsilon_t - \epsilon_{t-1}) \\
\Delta y_t &= \Delta \mathbf{x}\beta + \Delta u_t
\end{aligned}
$$

**Unit Root Test** 

$$
y_t = \alpha + \alpha y_{t-1} + u_t
$$ 

tests if $\rho=1$ (integrated of order 1)\

-   Under the null $H_0: \rho = 1$, OLS is not consistent or asymptotically normal.
-   Under the alternative $H_a: \rho < 1$, OLS is consistent and asymptotically normal.
-   usual t-test is not valid, will need to use the transformed equation to produce a valid test.

**Dickey-Fuller Test** $$
\Delta y_t= \alpha + \theta y_{t-1} + v_t
$$ where $\theta = \rho -1$\

-   $H_0: \theta = 0$ and $H_a: \theta < 0$
-   Under the null, $\Delta y_t$ is weakly dependent but $y_{t-1}$ is not.
-   Dickey and Fuller derived the non-normal asymptotic distribution. If you reject the null then $y_t$ is not a random walk.

Concerns with the standard Dickey Fuller Test\
1. Only considers a fairly simplistic dynamic relationship

$$
\Delta y_t = \alpha + \theta y_{t-1} + \gamma_1 \Delta_{t-1} + ..+ \gamma_p \Delta_{t-p} +v_t
$$

-   with one additional lag, under the null $\Delta_{y_t}$ is an AR(1) process and under the alternative $y_t$ is an AR(2) process.
-   Solution: include lags of $\Delta_{y_t}$ as controls.

2.  Does not allow for time trend $$
    \Delta y_t = \alpha + \theta y_{t-1} + \delta t + v_t
    $$

-   allows $y_t$ to have a quadratic relationship with t
-   Solution: include time trend (changes the critical values).

**Adjusted Dickey-Fuller Test** $$
\Delta y_t = \alpha + \theta y_{t-1} + \delta t + \gamma_1 \Delta y_{t-1} + ... + \gamma_p \Delta y_{t-p} + v_t
$$ where $\theta = 1 - \rho$\

-   $H_0: \theta_1 = 0$ and $H_a: \theta_1 < 0$
-   Under the null, $\Delta y_t$ is weakly dependent but $y_{t-1}$ is not
-   Critical values are different with the time trend, if you reject the null then $y_t$ is not a random walk.

##### Newey West Standard Errors

If [A4][A4 Homoskedasticity] does not hold, we can use Newey West Standard Errors (HAC - Heteroskedasticity Autocorrelation Consistent)

$$
\hat{B} = T^{-1} \sum_{t=1}^{T} e_t^2 \mathbf{x'_tx_t} + \sum_{h=1}^{g}(1-\frac{h}{g+1})T^{-1}\sum_{t=h+1}^{T} e_t e_{t-h}(\mathbf{x_t'x_{t-h}+ x_{t-h}'x_t})
$$

-   estimates the covariances up to a distance g part

-   downweights to insure $\hat{B}$ is PSD

-   How to choose g:

    -   For yearly data: g = 1 or 2 is likely to account for most of the correlation
    -   For quarterly or monthly data: g should be larger (g = 4 or 8 for quarterly and g = 12 or 14 for monthly)
    -   can also take integer part of $4(T/100)^{2/9}$ or integer part of $T^{1/4}$

**Testing for Serial Correlation**

1.  Run OLS regression of $y_t$ on $\mathbf{x_t}$ and obtain residuals $e_t$

2.  Run OLS regression of $e_t$ on $\mathbf{x}_t, e_{t-1}$ and test whether coefficient on $e_{t-1}$ is significant.

3.  Reject the null of no serial correlation if the coefficient is significant at the 5% level.

    -   Test using heteroskedastic robust standard errors
    -   can include $e_{t-2},e_{t-3},..$ in step 2 to test for higher order serial correlation (t-test would now be an F-test of joint significance)

## Repeated Cross Sections

For each time point (day, month, year, etc.), a set of data is sampled. This set of data can be different among different time points.

For example, you can sample different groups of students each time you survey.

Allowing structural change in pooled cross section

$$
y_i = \mathbf{x}_i \beta + \delta_1 y_1 + ... + \delta_T y_T + \epsilon_i
$$ 

Dummy variables for all but one time period

-   allows different intercept for each time period
-   allows outcome to change on average for each time period

Allowing for structural change in pooled cross section

$$
y_i = \mathbf{x}_i \beta + \mathbf{x}_i y_1 \gamma_1 + ... + \mathbf{x}_i y_T \gamma_T + \delta_1 y_1 + ...+ \delta_T y_T + \epsilon_i
$$ 

Interact $x_i$ with time period dummy variables

-   allows different slopes for each time period
-   allows effects to change based on time period (**structural break**)
-   Interacting all time period dummies with $x_i$ can produce many variables - use hypothesis testing to determine which structural breaks are needed.

### Pooled Cross Section

$$
y_i=\mathbf{x_i\beta +x_i \times y1\gamma_1 + ...+ x_i \times yT\gamma_T + \delta_1y_1+...+ \delta_Ty_T + \epsilon_i}
$$ 

Interact $x_i$ with time period dummy variables

-   allows different slopes for each time period

-   allows effect to change based on time period (structural break)

    -   interacting all time period dummies with $x_i$ can produce many variables - use hypothesis testing to determine which structural breaks are needed.

## Panel Data

Detail notes in R can be found [here](https://cran.r-project.org/web/packages/plm/vignettes/plmPackage.html#robust)

Follows an individual over T time periods.

Panel data structure is like having n samples of time series data

**Characteristics**

-   Information both across individuals and over time (cross-sectional and time-series)

-   N individuals and T time periods

-   Data can be either

    -   Balanced: all individuals are observed in all time periods
    -   Unbalanced: all individuals are not observed in all time periods.

-   Assume correlation (clustering) over time for a given individual, with independence over individuals.

**Types**

-   Short panel: many individuals and few time periods.
-   Long panel: many time periods and few individuals
-   Both: many time periods and many individuals

**Time Trends and Time Effects**

-   Nonlinear
-   Seasonality
-   Discontinuous shocks

**Regressors**

-   Time-invariant regressors $x_{it}=x_i$ for all t (e.g., gender, race, education) have zero within variation
-   Individual-invariant regressors $x_{it}=x_{t}$ for all i (e.g., time trend, economy trends) have zero between variation

**Variation for the dependent variable and regressors**

-   Overall variation: variation over time and individuals.
-   Between variation: variation between individuals
-   Within variation: variation within individuals (over time).

+------------------+-----------------------------------------------------------------------------------------------------------------------------+
| Estimate         | Formula                                                                                                                     |
+==================+=============================================================================================================================+
| Individual mean  | $\bar{x_i}= \frac{1}{T} \sum_{t}x_{it}$                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------+
| Overall mean     | $\bar{x}=\frac{1}{NT} \sum_{i} \sum_t x_{it}$                                                                               |
+------------------+-----------------------------------------------------------------------------------------------------------------------------+
| Overall Variance | $s _O^2 = \frac{1}{NT-1} \sum_i \sum_t (x_{it} - \bar{x})^2$                                                                |
+------------------+-----------------------------------------------------------------------------------------------------------------------------+
| Between variance | $s_B^2 = \frac{1}{N-1} \sum_i (\bar{x_i} -\bar{x})^2$                                                                       |
+------------------+-----------------------------------------------------------------------------------------------------------------------------+
| Within variance  | $s_W^2= \frac{1}{NT-1} \sum_i \sum_t (x_{it} - \bar{x_i})^2 = \frac{1}{NT-1} \sum_i \sum_t (x_{it} - \bar{x_i} +\bar{x})^2$ |
+------------------+-----------------------------------------------------------------------------------------------------------------------------+

**Note**: $s_O^2 \approx s_B^2 + s_W^2$

Since we have n observation for each time period t, we can control for each time effect separately by including time dummies (time effects)

$$
y_{it}=\mathbf{x_{it}\beta} + d_1\delta_1+...+d_{T-1}\delta_{T-1} + \epsilon_{it}
$$

**Note**: we cannot use these many time dummies in time series data because in time series data, our n is 1. Hence, there is no variation, and sometimes not enough data compared to variables to estimate coefficients.

**Unobserved Effects Model** Similar to group clustering, assume that there is a random effect that captures differences across individuals but is constant in time.

$$
y_it=\mathbf{x_{it}\beta} + d_1\delta_1+...+d_{T-1}\delta_{T-1} + c_i + u_{it}
$$

where

-   $c_i + u_{it} = \epsilon_{it}$
-   $c_i$ unobserved individual heterogeneity (effect)
-   $u_{it}$ idiosyncratic shock
-   $\epsilon_{it}$ unobserved error term.

### Pooled OLS Estimator

If $c_i$ is uncorrelated with $x_{it}$

$$
E(\mathbf{x_{it}'}(c_i+u_{it})) = 0
$$

then [A3a] still holds. And we have Pooled OLS consistent.

If [A4][A4 Homoskedasticity] does not hold, OLS is still consistent, but not efficient, and we need cluster robust SE.

Sufficient for [A3a] to hold, we need

-   **Exogeneity** for $u_{it}$ [A3a] (contemporaneous exogeneity): $E(\mathbf{x_{it}'}u_{it})=0$ time varying error
-   **Random Effect Assumption** (time constant error): $E(\mathbf{x_{it}'}c_{i})=0$

Pooled OLS will give you consistent coefficient estimates under [A1][A1 Linearity], [A2][A2 Full rank], [A3a] (for both $u_{it}$ and RE assumption), and [A5][A5 Data Generation (random Sampling)] (randomly sampling across i).

### Individual-specific effects model

-   If we believe that there is unobserved heterogeneity across individual (e.g., unobserved ability of an individual affects $y$), If the individual-specific effects are correlated with the regressors, then we have the [Fixed Effects Estimator]. and if they are not correlated we have the [Random Effects Estimator].

#### Random Effects Estimator

Random Effects estimator is the Feasible GLS estimator that assumes $u_{it}$ is serially uncorrelated and homoskedastic

-   Under [A1][A1 Linearity], [A2][A2 Full rank], [A3a] (for both $u_{it}$ and RE assumption) and [A5][A5 Data Generation (random Sampling)] (randomly sampling across i), RE estimator is consistent.

    -   If [A4][A4 Homoskedasticity] holds for $u_{it}$, RE is the most efficient estimator
    -   If [A4][A4 Homoskedasticity] fails to hold (may be heteroskedasticity across i, and serial correlation over t), then RE is not the most efficient, but still more efficient than pooled OLS.

#### Fixed Effects Estimator

also known as **Within Estimator** uses within variation (over time)

If the **RE assumption** is not hold ($E(\mathbf{x_{it}'}c_i) \neq 0$), then A3a does not hold ($E(\mathbf{x_{it}'}\epsilon_i) \neq 0$). Hence, the OLS and RE are inconsistent/biased (because of omitted variable bias)

To deal with violation in $c_i$, we have 

$$
y_{it}= \mathbf{x_{it}\beta} + c_i + u_{it}
$$

$$
\bar{y_i}=\bar{\mathbf{x_i}} \beta + c_i + \bar{u_i}
$$ 

where the second equation is the time averaged equation

using **within transformation**, we have 

$$
y_{it} - \bar{y_i} = \mathbf{(x_{it} - \bar{x_i}\beta)} + u_{it} - \bar{u_i}
$$

because $c_i$ is time constant.

The Fixed Effects estimator uses POLS on the transformed equation

$$
y_{it} - \bar{y_i} = \mathbf{(x_{it} - \bar{x_i}\beta)} + d_1\delta_1 + ... + d_{T-2}\delta_{T-2} + u_{it} - \bar{u_i}
$$

-   we need [A3][A3 Exogeneity of Independent Variables] (strict exogeneity) ($E((\mathbf{x_{it}-\bar{x_i}})'(u_{it}-\bar{u_i})=0$) to have FE consistent.

-   Variables that are time constant will be absorbed into $c_i$. Hence we cannot make inference on time constant independent variables.

    -   If you are interested in the effects of time-invariant variables, you could consider the OLS or **between estimator**

-   It's recommended that you should still use cluster robust standard errors.

Equivalent to the within transformation, we can have the fixed effect estimator be the same with the dummy regression

$$
y_{it} = x_{it}\beta + d_1\delta_1 + ... + d_{T-2}\delta_{T-2} + c_1\gamma_1 + ... + c_{n-1}\gamma_{n-1} + u_{it}
$$ 

where

\begin{equation}
c_i
=
\begin{cases}
1 &\text{if observation is i} \\
0 &\text{otherwise} \\
\end{cases}
\end{equation}

-   The standard error is incorrectly calculated.
-   the FE within transformation is controlling for any difference across individual which is allowed to correlated with observables.

### Tests for Assumptions

We typically don't test heteroskedasticity because we will use robust covariance matrix estimation anyway.

Dataset


```r
library("plm")
data("EmplUK", package="plm")
data("Produc", package="plm")
data("Grunfeld", package="plm")
data("Wages", package="plm")
```

#### Poolability

also known as an F test of stability (or Chow test) for the coefficients

$H_0$: All individuals have the same coefficients (i.e., equal coefficients for all individuals).

$H_a$ Different individuals have different coefficients.

Notes:

-   Under a within (i.e., fixed) model, different intercepts for each individual are assumed
-   Under random model, same intercept is assumed


```r
library(plm)
plm::pooltest(inv~value+capital, data=Grunfeld, model="within")
```

```
## 
## 	F statistic
## 
## data:  inv ~ value + capital
## F = 5.7805, df1 = 18, df2 = 170, p-value = 1.219e-10
## alternative hypothesis: unstability
```

Hence, we reject the null hypothesis that coefficients are stable. Then, we should use the random model.

#### Individual and time effects

use the Lagrange multiplier test to test the presence of individual or time or both (i.e., individual and time).

Types:

-   `honda`: [@Honda_1985] Default
-   `bp`: [@Breusch_1980] for unbalanced panels
-   `kw`: [@King_1997] unbalanced panels, and two-way effects
-   `ghm`: [@Gourieroux_1982]: two-way effects


```r
pFtest(inv~value+capital, data=Grunfeld, effect="twoways")
```

```
## 
## 	F test for twoways effects
## 
## data:  inv ~ value + capital
## F = 17.403, df1 = 28, df2 = 169, p-value < 2.2e-16
## alternative hypothesis: significant effects
```

```r
pFtest(inv~value+capital, data=Grunfeld, effect="individual")
```

```
## 
## 	F test for individual effects
## 
## data:  inv ~ value + capital
## F = 49.177, df1 = 9, df2 = 188, p-value < 2.2e-16
## alternative hypothesis: significant effects
```

```r
pFtest(inv~value+capital, data=Grunfeld, effect="time")
```

```
## 
## 	F test for time effects
## 
## data:  inv ~ value + capital
## F = 0.23451, df1 = 19, df2 = 178, p-value = 0.9997
## alternative hypothesis: significant effects
```

#### Cross-sectional dependence/contemporaneous correlation

-   Null hypothesis: residuals across entities are not correlated.

##### Global cross-sectional dependence 


```r
pcdtest(inv~value+capital, data=Grunfeld, model="within")
```

```
## 
## 	Pesaran CD test for cross-sectional dependence in panels
## 
## data:  inv ~ value + capital
## z = 4.6612, p-value = 3.144e-06
## alternative hypothesis: cross-sectional dependence
```

##### Local cross-sectional dependence

use the same command, but supply matrix `w` to the argument.


```r
pcdtest(inv~value+capital, data=Grunfeld, model="within")
```

```
## 
## 	Pesaran CD test for cross-sectional dependence in panels
## 
## data:  inv ~ value + capital
## z = 4.6612, p-value = 3.144e-06
## alternative hypothesis: cross-sectional dependence
```

#### Serial Correlation

-   Null hypothesis: there is no serial correlation

-   usually seen in macro panels with long time series (large N and T), not seen in micro panels (small T and large N)

-   Serial correlation can arise from individual effects(i.e., time-invariant error component), or idiosyncratic error terms (e..g, in the case of AR(1) process). But typically, when we refer to serial correlation, we refer to the second one.

-   Can be

    -   **marginal** test: only 1 of the two above dependence (but can be biased towards rejection)

    -   **joint** test: both dependencies (but don't know which one is causing the problem)

    -   **conditional** test: assume you correctly specify one dependence structure, test whether the other departure is present.

<br>

##### Unobserved effect test

-   semi-parametric test (the test statistic $W \dot{\sim} N$ regardless of the distribution of the errors) with $H_0: \sigma^2_\mu = 0$ (i.e., no unobserved effects in the residuals), favors pooled OLS.

    -   Under the null, covariance matrix of the residuals = its diagonal (off-diagonal = 0)

-   It is robust against both **unobserved effects** that are constant within every group, and any kind of **serial correlation**.


```r
pwtest(log(gsp)~log(pcap)+log(pc)+log(emp)+unemp, data=Produc)
```

```
## 
## 	Wooldridge's test for unobserved individual effects
## 
## data:  formula
## z = 3.9383, p-value = 8.207e-05
## alternative hypothesis: unobserved effect
```

Here, we reject the null hypothesis that the no unobserved effects in the residuals. Hence, we will exclude using pooled OLS.

<br>

##### Locally robust tests for random effects and serial correlation

-   A joint LM test for **random effects** and **serial correlation** assuming normality and homoskedasticity of the idiosyncratic errors [@Baltagi_1991][@Baltagi_1995]


```r
pbsytest(log(gsp)~log(pcap)+log(pc)+log(emp)+unemp, data=Produc, test="j")
```

```
## 
## 	Baltagi and Li AR-RE joint test - balanced panel
## 
## data:  formula
## chisq = 4187.6, df = 2, p-value < 2.2e-16
## alternative hypothesis: AR(1) errors or random effects
```

Here, we reject the null hypothesis that there is no presence of **serial correlation,** and **random effects**. But we still do not know whether it is because of serial correlation, of random effects or of both

To know the departure from the null assumption, we can use [@Bera_2001]'s test for first-order serial correlation or random effects (both under normality and homoskedasticity assumption of the error).

BSY for serial correlation


```r
pbsytest(log(gsp)~log(pcap)+log(pc)+log(emp)+unemp, data=Produc)
```

```
## 
## 	Bera, Sosa-Escudero and Yoon locally robust test - balanced panel
## 
## data:  formula
## chisq = 52.636, df = 1, p-value = 4.015e-13
## alternative hypothesis: AR(1) errors sub random effects
```

BSY for random effects


```r
pbsytest(log(gsp)~log(pcap)+log(pc)+log(emp)+unemp, data=Produc, test="re")
```

```
## 
## 	Bera, Sosa-Escudero and Yoon locally robust test (one-sided) -
## 	balanced panel
## 
## data:  formula
## z = 57.914, p-value < 2.2e-16
## alternative hypothesis: random effects sub AR(1) errors
```

Since BSY is only locally robust, if you "know" there is no serial correlation, then this test is based on LM test is more superior:


```r
plmtest(inv ~ value + capital, data = Grunfeld, type = "honda")
```

```
## 
## 	Lagrange Multiplier Test - (Honda) for balanced panels
## 
## data:  inv ~ value + capital
## normal = 28.252, p-value < 2.2e-16
## alternative hypothesis: significant effects
```

On the other hand, if you know there is no random effects, to test for serial correlation, use [@BREUSCH_1978]-[@Godfrey_1978]'s test


```r
lmtest::bgtest()
```

If you "know" there are random effects, use [@Baltagi_1995]'s. to test for serial correlation in both AR(1) and MA(1) processes.

$H_0$: Uncorrelated errors.

Note:

-   one-sided only has power against positive serial correlation.
-   applicable to only balanced panels.


```r
pbltest(log(gsp)~log(pcap)+log(pc)+log(emp)+unemp, 
        data=Produc, alternative="onesided")
```

```
## 
## 	Baltagi and Li one-sided LM test
## 
## data:  log(gsp) ~ log(pcap) + log(pc) + log(emp) + unemp
## z = 21.69, p-value < 2.2e-16
## alternative hypothesis: AR(1)/MA(1) errors in RE panel model
```

General serial correlation tests

-   applicable to random effects model, OLS, and FE (with large T, also known as long panel).
-   can also test higher-order serial correlation


```r
plm::pbgtest(plm::plm(inv~value+capital, data = Grunfeld, model = "within"), order = 2)
```

```
## 
## 	Breusch-Godfrey/Wooldridge test for serial correlation in panel models
## 
## data:  inv ~ value + capital
## chisq = 42.587, df = 2, p-value = 5.655e-10
## alternative hypothesis: serial correlation in idiosyncratic errors
```

in the case of short panels (small T and large n), we can use


```r
pwartest(log(emp) ~ log(wage) + log(capital), data=EmplUK)
```

```
## 
## 	Wooldridge's test for serial correlation in FE panels
## 
## data:  plm.model
## F = 312.3, df1 = 1, df2 = 889, p-value < 2.2e-16
## alternative hypothesis: serial correlation
```

#### Unit roots/stationarity

-   Dickey-Fuller test for stochastic trends.
-   Null hypothesis: the series is non-stationary (unit root)
-   You would want your test to be less than the critical value (p\<.5) so that there is evidence there is not unit roots.

#### Heteroskedasticity

-   Breusch-Pagan test

-   Null hypothesis: the data is homoskedastic

-   If there is evidence for heteroskedasticity, robust covariance matrix is advised.

-   To control for heteroskedasticity: Robust covariance matrix estimation (Sandwich estimator)

    -   "white1" - for general heteroskedasticity but no serial correlation (check serial correlation first). Recommended for random effects.
    -   "white2" - is "white1" restricted to a common variance within groups. Recommended for random effects.
    -   "arellano" - both heteroskedasticity and serial correlation. Recommended for fixed effects

### Model Selection

#### POLS vs. RE

The continuum between RE (used FGLS which more assumption ) and POLS check back on the section of FGLS

**Breusch-Pagan LM** test

-   Test for the random effect model based on the OLS residual
-   Null hypothesis: variances across entities is zero. In another word, no panel effect.
-   If the test is significant, RE is preferable compared to POLS

#### FE vs. RE

-   RE does not require strict exogeneity for consistency (feedback effect between residual and covariates)

+----------------------------------------+----------------------------------------------------------------------------------------+
| Hypothesis                             | If true                                                                                |
+========================================+========================================================================================+
| $H_0: Cov(c_i,\mathbf{x_{it}})=0$      | $\hat{\beta}_{RE}$ is consistent and efficient, while $\hat{\beta}_{FE}$ is consistent |
+----------------------------------------+----------------------------------------------------------------------------------------+
| $H_0: Cov(c_i,\mathbf{x_{it}}) \neq 0$ | $\hat{\beta}_{RE}$ is inconsistent, while $\hat{\beta}_{FE}$ is consistent             |
+----------------------------------------+----------------------------------------------------------------------------------------+

**Hausman Test**

For the Hausman test to run, you need to assume that

-   strict exogeneity hold
-   A4 to hold for $u_{it}$

Then,

-   Hausman test statistic: $H=(\hat{\beta}_{RE}-\hat{\beta}_{FE})'(V(\hat{\beta}_{RE})- V(\hat{\beta}_{FE}))(\hat{\beta}_{RE}-\hat{\beta}_{FE}) \sim \chi_{n(X)}^2$ where $n(X)$ is the number of parameters for the time-varying regressors.
-   A low p-value means that we would reject the null hypothesis and prefer FE
-   A high p-value means that we would not reject the null hypothesis and consider RE estimator.


```r
gw <- plm(inv~value+capital, data=Grunfeld, model="within")
gr <- plm(inv~value+capital, data=Grunfeld, model="random")
phtest(gw, gr)
```

```
## 
## 	Hausman Test
## 
## data:  inv ~ value + capital
## chisq = 2.3304, df = 2, p-value = 0.3119
## alternative hypothesis: one model is inconsistent
```

+-----------------------+-----------------+---------------------------------+---------------------------------+-----------------------------------------+------------------------+------------------------+-------+-------------------------------------------+
| Violation   Estimator | Basic Estimator | Instrumental variable Estimator | Variable Coefficients estimator | Generalized Method of Moments estimator | General FGLS estimator | Means groups estimator | CCEMG | Estimator for limited dependent variables |
+-----------------------+-----------------+---------------------------------+---------------------------------+-----------------------------------------+------------------------+------------------------+-------+-------------------------------------------+

### Summary

-   All three estimators (POLS, RE, FE) require [A1][A1 Linearity], [A2][A2 Full rank], [A5][A5 Data Generation (random Sampling)] (for individuals) to be consistent. Additionally,

-   POLS is consistent under A3a(for $u_{it}$): $E(\mathbf{x}_{it}'u_{it})=0$, and RE Assumption $E(\mathbf{x}_{it}'c_{i})=0$

    -   If [A4][A4 Homoskedasticity] does not hold, use cluster robust SE but POLS is not efficient

-   RE is consistent under A3a(for $u_{it}$): $E(\mathbf{x}_{it}'u_{it})=0$, and RE Assumption $E(\mathbf{x}_{it}'c_{i})=0$

    -   If [A4][A4 Homoskedasticity] (for $u_{it}$) holds then usual SE are valid and RE is most efficient
    -   If [A4][A4 Homoskedasticity] (for $u_{it}$) does not hold, use cluster robust SE ,and RE is no longer most efficient (but still more efficient than POLS)

-   FE is consistent under [A3][A3 Exogeneity of Independent Variables] $E((\mathbf{x}_{it}-\bar{\mathbf{x}}_{it})'(u_{it} -\bar{u}_{it}))=0$

    -   Cannot estimate effects of time constant variables
    -   A4 generally does not hold for $u_{it} -\bar{u}_{it}$ so cluster robust SE are needed

**Note**: [A5][A5 Data Generation (random Sampling)] for individual (not for time dimension) implies that you have [A5a] for the entire data set.

| Estimator / True Model | POLS       | RE         | FE           |
|------------------------|------------|------------|--------------|
| POLS                   | Consistent | Consistent | Inconsistent |
| FE                     | Consistent | Consistent | Consistent   |
| RE                     | Consistent | Consistent | Inconsistent |

Based on table provided by [Ani Katchova](https://sites.google.com/site/econometricsacademy/econometrics-models/panel-data-models)

### Application

Recommended application of `plm` can be found [here](https://cran.r-project.org/web/packages/plm/vignettes/B_plmFunction.html) and [here](https://cran.r-project.org/web/packages/plm/vignettes/C_plmModelComponents.html) by Yves Croissant


```r
#install.packages("plm")
library("plm")

library(foreign)
Panel <- read.dta("http://dss.princeton.edu/training/Panel101.dta")

attach(Panel)
Y <- cbind(y)
X <- cbind(x1, x2, x3)

# Set data as panel data
pdata <- pdata.frame(Panel, index=c("country","year"))

# Pooled OLS estimator
pooling <- plm(Y ~ X, data=pdata, model= "pooling")
summary(pooling)

# Between estimator
between <- plm(Y ~ X, data=pdata, model= "between")
summary(between)

# First differences estimator
firstdiff <- plm(Y ~ X, data=pdata, model= "fd")
summary(firstdiff)

# Fixed effects or within estimator
fixed <- plm(Y ~ X, data=pdata, model= "within")
summary(fixed)

# Random effects estimator
random <- plm(Y ~ X, data=pdata, model= "random")
summary(random)

# LM test for random effects versus OLS
# Accept Null, then OLS, Reject Null then RE
plmtest(pooling,effect = "individual", type = c("bp")) # other type: "honda", "kw"," "ghm"; other effect : "time" "twoways"


# B-P/LM and Pesaran CD (cross-sectional dependence) test
pcdtest(fixed, test = c("lm")) # Breusch and Pagan's original LM statistic
pcdtest(fixed, test = c("cd")) # Pesaran's CD statistic

# Serial Correlation
pbgtest(fixed)

# stationary
library("tseries")
adf.test(pdata$y, k = 2)

# LM test for fixed effects versus OLS
pFtest(fixed, pooling)

# Hausman test for fixed versus random effects model
phtest(random, fixed)

# Breusch-Pagan heteroskedasticity
library(lmtest)
bptest(y ~ x1 + factor(country), data = pdata)

# If there is presence of heteroskedasticity
## For RE model
coeftest(random) #orginal coef
coeftest(random, vcovHC) # Heteroskedasticity consistent coefficients

t(sapply(c("HC0", "HC1", "HC2", "HC3", "HC4"), function(x) sqrt(diag(vcovHC(random, type = x))))) #show HC SE of the coef
# HC0 - heteroskedasticity consistent. The default.
# HC1,HC2, HC3 – Recommended for small samples. HC3 gives less weight to influential observations.
# HC4 - small samples with influential observations
# HAC - heteroskedasticity and autocorrelation consistent

## For FE model
coeftest(fixed) # Original coefficients
coeftest(fixed, vcovHC) # Heteroskedasticity consistent coefficients
coeftest(fixed, vcovHC(fixed, method = "arellano")) # Heteroskedasticity consistent coefficients (Arellano)
t(sapply(c("HC0", "HC1", "HC2", "HC3", "HC4"), function(x) sqrt(diag(vcovHC(fixed, type = x))))) #show HC SE of the coef
```

**Advanced**

Other methods to estimate the random model:

-   `"swar"`: *default* [@Swamy_1972]
-   `"walhus"`: [@Wallace_1969]
-   `"amemiya"`: [@Fuller_1974]
-   `"nerlove"`" [@Nerlove_1971]

Other effects:

-   Individual effects: *default*
-   Time effects: `"time"`
-   Individual and time effects: `"twoways"`

**Note**: no random two-ways effect model for `random.method = "nerlove"`


```r
amemiya <- plm(Y ~ X, data=pdata, model= "random",random.method = "amemiya",effect = "twoways")
```

To call the estimation of the variance of the error components


```r
ercomp(Y~X, data=pdata, method = "amemiya", effect = "twoways")
```

Check for the unbalancedness. Closer to 1 indicates balanced data [@Ahrens_1981]


```r
punbalancedness(random)
```

**Instrumental variable**

-   `"bvk"`: default [@Balestra_1987]
-   `"baltagi"`: [@Baltagi_1981]
-   `"am"` [@Amemiya_1986]
-   `"bms"`: [@Breusch_1989]


```r
instr <- plm(Y ~ X | X_ins, data = pdata, random.method = "ht", model = "random", inst.method = "baltagi")
```

### Other Estimators

#### Variable Coefficients Model


```r
fixed_pvcm <- pvcm(Y~X, data=pdata, model="within")
random_pvcm <- pvcm(Y~X, data=pdata, model="random")
```

More details can be found [here](https://cran.r-project.org/web/packages/plm/vignettes/plmPackage.html)

#### Generalized Method of Moments Estimator

Typically use in dynamic models. Example is from [plm package](https://cran.r-project.org/web/packages/plm/vignettes/plmPackage.html)


```r
z2 <- pgmm(log(emp) ~ lag(log(emp), 1)+ lag(log(wage), 0:1) +
           lag(log(capital), 0:1) | lag(log(emp), 2:99) +
           lag(log(wage), 2:99) + lag(log(capital), 2:99),        
           data = EmplUK, effect = "twoways", model = "onestep", 
           transformation = "ld")
summary(z2, robust = TRUE)
```

#### General Feasible Generalized Least Squares Models

Assume there is no cross-sectional correlation Robust against intragroup heteroskedasticity and serial correlation. Suited when n is much larger than T (long panel) However, inefficient under groupwise heteorskedasticity.


```r
# Random Effects
zz <- pggls(log(emp)~log(wage)+log(capital), data=EmplUK, model="pooling")

# Fixed
zz <- pggls(log(emp)~log(wage)+log(capital), data=EmplUK, model="within")
```
