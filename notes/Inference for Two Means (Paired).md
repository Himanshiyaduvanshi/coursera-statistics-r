

# Inference for Two Means (Paired)

## Definition

**Population Parameter — Difference between Two Paired Population Means** $\mu_{diff}$

- Two sets of observations are _paired_ if each observation in one set has a special correspondence or connection with exactly one observation in the other data set.

**Point Estimate — Difference between Two Paired Sample Means** $\bar{x}_{diff}$
$$
\bar{x}_{diff} = \frac{\sum^n_{i=1} {x}_{1(i)} - {x}_{2(i)}}{n}
$$

**Number of Samples** $n_{diff}$

$$
n_{diff} = n_1 = n_2
$$

**Sample Standard Deviation** $s_{diff}$

## Theoretical-Based Inference

### T-Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.

**Normality**

- Observations come from a nearly normal distribution.
- The normal condition can be relaxed as the sample size increases.
- $n_{diff} < 30$: If the sample size $n$ is less than 30 and there are no clear outliers in the data, then we typically assume the data come from a nearly normal distribution to satisfy the condition.
- $n_{diff} ≥ 30$: If the sample size $n$ is at least 30 and there are no particularly extreme outliers, then we typically assume the sampling distribution of $\bar{x}_{diff}$ is nearly normal, even if the underlying distribution of individual observations is not.

#### Sampling Distribution

**Standard Error** $SE_{\bar{x}_{diff}}$

$$
SE_{\bar{x}_{diff}} = \frac{s_{diff}}{\sqrt{n_{diff}}}
$$


#### Parameters

**Degrees of Freedom** $df$

$$
df = n_{diff} - 1
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
\bar{x}_{diff} \pm t^\star_{df} \times SE_{\bar{x}_{diff}} 
$$

#### Hypothesis Testing

**Hypothesis Test**

$$
\begin{aligned}
H_0: \mu_{diff} &= 0 \\
H_A: \mu_{diff} &\ge or \le or \ne 0
\end{aligned}
$$

**T-Score** $T$

$$
T = \frac{\bar{x}_{diff} - 0}{SE_{\bar{x}_{diff}}}
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

