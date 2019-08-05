

# Inference for a Single Proportion (Many Levels)

## Definition

**Population Distribution** 

- A sample of observations can be classified into several categories / cases / cells / levels.
- Inference calculations determine if the sample is representative of the general population.

**Sample Distribution**

**Number of Columns** $C$

**Number of Cells** $k$

- Since there's only one proportion, the number of cells is equal to the number of columns $k = C$.

**One-Way Table**

|Levels 1           | $C_1$| $C_2$| ...| $C_j$|
|:------------------|-----:|-----:|---:|-----:|
|/ | $O_1$ $E_1$| $O_2$ $E_2$| ...| $O_j$ $E_j$|

## Theoretical-Based Inference

### Chi-Square Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.
- Each case that contributes a count to the table must be independent of all the other cases in the table.

**Sample Size / Distribution**

- Each particular scenario (i.e. cell $k$) must have at least 5 expected cases.

#### Parameters

**Degrees of Freedom** $df$
$$
df = k - 1
$$

- To determine if the calculated $\chi^2$ statistic is considered unusally high or not we need to describe its distribution.

#### Goodness-of-Fit Test (Hypothesis Test)

**Hypothesis Test**

$$
\begin{aligned}
H_0: &\text{ The observations are a random sample.}\\ 
&\text{ The observed counts of observations from various categories follow the same distribution in the population.}\\
H_A: &\text{ The observations are not a random sample.}\\ 
&\text{ The observed counts of observations from various categories does not follow the same distribution in the population.}\\
\end{aligned}
$$

**Observed Count** $O$

**Expected Count** $E$

- Expected count is the null value.

**Chi-Square Statistic** $\chi^2$

$$
\chi^2 = \sum^k_{i=1} \frac{(O_i-E_i)^2}{E_i}
$$

**P-Value**

- The p-value for a chi-square test is defined as the tail area above the calculated test statistic.
- Because the test statistic is always positive, a higher test statistic means a higher deviation from the null hypothesis.

```r
p_value = pchisq(chi2, df, lower.tail = FALSE)
```