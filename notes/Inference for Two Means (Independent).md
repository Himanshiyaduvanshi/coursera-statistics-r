

# Inference for Two Means (Independent)

## Definition

**Population Parameter — Difference between Two Independent Population Means** $\mu_1 - \mu_2$

- Two sets of observations are _paired_ if each observation in one set has a special correspondence or connection with exactly one observation in the other data set.

**Point Estimate — Difference between Two Independent Sample Means** $\bar{x}_1 - \bar{x}_2$

**Number of Samples** $n_1, n_2$

**Sample Standard Deviation** $s_1, s_2$

## Theoretical-Based Inference

### T-Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.
- The data are independent within and between the two groups, e.g. the data come from independent random samples or from a randomized experiment.

**Normality**

- Observations come from a nearly normal distribution.
- The normal condition can be relaxed as the sample size increases.
- We check the outliers rules of thumb for each group separately.
- $n_i < 30$: If both the sample sizes $n_1$ and $n_2$ are less than 30 and there are no clear outliers in the data, then we typically assume the data come from a nearly normal distribution to satisfy the condition.
- $n_i ≥ 30$: If the both sample sizes $n_1$ and $n_2$ are at least 30 and there are no particularly extreme outliers, then we typically assume the sampling distribution of $\bar{x}_1 - \bar{x}_2$ is nearly normal, even if the underlying distribution of individual observations is not.

#### Sampling Distribution

**Standard Error** $SE_{\bar{x}_1 - \bar{x}_2}$

$$
SE_{\bar{x}_1 - \bar{x}_2} = \sqrt{
\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}
}
$$

#### Parameters

**Degrees of Freedom** $df$

- The official formula for the degrees of freedom for $\bar{x}_1 - \bar{x}_2$ is quite complex and is generally computed using software, so instead you may use the smaller of $n_1 - 1$ and $n_2 - 1$ for the degrees of freedom if software isn’t readily available.

$$
df = \min \{ n_1 - 1 , n_2 - 1 \}
$$

- Generally the value of $t^\star_{df}$ is slightly larger than what we would get under the normal model with $z^\star$.- The larger the $df$, the closer the t-distribution is to the normal distribution.
- When the $df$ is about 30 or more, the t-distribution is nearly indistinguishable from the normal distribution.

#### Confidence Interval

**Critical T-Score** $t^\star_{df}$

```r
t = qt((1 - ci_level)/2, df)
```

**Confidence Interval**

$$
\bar{x}_1 - \bar{x}_2 \pm t^\star_{df} \times SE_{\bar{x}_1 - \bar{x}_2} 
$$

#### Hypothesis Testing

**Hypothesis Test**

$$
\begin{aligned}
H_0: \mu_1 - \mu_2 &= 0 \\
H_A: \mu_1 - \mu_2 &\ge or \le or \ne 0
\end{aligned}
$$

**T-Score** $T$

$$
T = \frac{(\bar{x}_1 - \bar{x}_2) - 0}{SE_{\bar{x}_1 - \bar{x}_2}}
$$

**P-Value**

One-sized test

```r
p_value = pt(T, df, lower.tail)
```

Two-sized test

```r
p_value = pt(T, df, lower.tail) * 2
```





