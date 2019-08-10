# Inference for a Single Mean

## Definition

**Population Parameter — Population Mean** $\mu$ 

**Point Estimate — Sample Mean** $\bar{x}$

**Number of Samples** $n$

**Sample Standard Deviation** $s$

## Theoretical-Based Inference

- If we have a lot of data and can estimate the population standard deviaion $\sigma$ using $s$ accurately, we can make inference with the _normal distribution_. 
- However, the estimate is less precise with smaller samples, and this leads to problems when using the normal distribution to model $\bar{x}$.
- In this regard, the _t-disbrituion_ is more robust for inference calculations.
- The extra thick tails of the t-distribution are exactly the correction needed to resolve the problem of using $s$ in place of $\sigma$ in the $SE$ calculation.

### Normal Distribution

### T-Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.

**Normality**

- Observations come from a nearly normal distribution.
- The normal condition can be relaxed as the sample size increases.
- $n < 30$: If the sample size $n$ is less than 30 and there are no clear outliers in the data, then we typically assume the data come from a nearly normal distribution to satisfy the condition.
- $n ≥ 30$: If the sample size $n$ is at least 30 and there are no particularly extreme outliers, then we typically assume the sampling distribution of $\bar{x}$ is nearly normal, even if the underlying distribution of individual observations is not.

#### Sampling Distribution

**Standard Error** $SE_{\bar{x}}$

$$
SE_{\bar{x}} = \frac{s}{\sqrt{n}}
$$

#### Parameters

**Degrees of Freedom** $df$

$$
df = n - 1
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
\bar{x} \pm t^\star_{df} \times SE_{\bar{x}}
$$

#### Hypothesis Testing

**Hypothesis Test**

$$
\begin{aligned}
H_0: \mu &= \text{null value} \\
H_A: \mu &\ge or \le or \ne \text{null value} 
\end{aligned}
$$

**T-Score** $T$
$$
T = \frac{\bar{x} - \text{null value}}{SE_{\bar{x}}}
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

