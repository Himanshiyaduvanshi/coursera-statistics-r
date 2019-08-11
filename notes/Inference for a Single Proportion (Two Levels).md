# Inference for a Single Proportion (Two Levels)

## Definition

**Population Parameter — Population Proportion** $p$ 

**Point Estimate — Sample Proportion** $\hat{p}$

**Number of Samples** $n$

## Theoretical-Based Inference

### Normal Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.

**Success-Failure Condition**

- The success-failure condition expect to see at least 10 success and 10 failure in the sample.

$$
\begin{aligned}
n p \ge 10 \\
n(1-p) \ge 10
\end{aligned}
$$

#### Sampling Distribution

**Standard Error** $SE_{\hat{p}}$

$$
SE_{\hat{p}} = \sqrt{
\frac{{p}(1-{p})}{n}
}
$$

#### Confidence Interval

**Success-Failure Condition**

- For inference with confidence interval, use $\hat{p}$ in place of $p$ to check the success-failure condition.

$$
\begin{aligned}
n \hat{p} \ge 10 \\
n(1-\hat{p}) \ge 10
\end{aligned}
$$

**Standard Error** $SE_{\hat{p}}$

- For computing the confidence interval, we use the $\hat{p}$ to compute the standard error.

$$
SE_{\hat{p}} = \sqrt{
\frac{\hat{p}(1-\hat{p})}{n}
}
$$

**Critical Z-Score** $z^\star$

```r
z = qnorm((1 - ci_level)/2, df)
```

**Confidence Interval**

$$
\hat{p} \pm z^\star \times SE_{\hat{p}} 
$$

#### Hypothesis Testing

**Success-Failure Condition**

- For inference with hypothesis testing with two proportions, use the null value (expected proportion) to check the success-failure condition.

$$
\begin{aligned}
n p_0 \ge 10 \\
n(1-p_0) \ge 10
\end{aligned}
$$

**Standard Error** $SE_{\hat{p}}$

- In a hypothesis test, we use the null value $p_0$ to compute the standard error.

$$
SE_{\hat{p}} = \sqrt{
\frac{{p_0}(1-{p_0})}{n}
}
$$

**Hypothesis Test**

$$
\begin{aligned}
H_0: p &= \text{null value} = 0.5 \\
H_A: p &\ge or \le or \ne \text{null value} 
\end{aligned}
$$

**Z-Score** $Z$

$$
Z = \frac{\hat{p} - 0.5}{SE_{\hat{p}}}
$$

**P-Value**

One-sized test

```r
p_value = pnorm(Z, lower.tail)
```

Two-sized test

```r
p_value = pnorm(Z, lower.tail) * 2
```

## Simulation-Based Inference

- When sample size is too small, the success-failure condition is not met. Hence, we cannot rely on the central limit theorem to do our inference.
- Note that the t-distribution is only appropriate to use for means. When sample size isn't sufficiently large, and the parameter of interest is a proportion or a difference between two proportions, we need to use simulation.
- In hypothesis testing, for one categorical variable, generate simulated samples based on the null hypothesis, and then calculate the number of samples that are at least as extreme as the observed data.
- Roughly 10,000 seems sufficient.

```r
library(statsr)
inference(
    y, est = 'proportion', type = 'ht', 
    method = 'simulation', null = 0.5, 
    success, alternative
)
```

- We can compute the exact p-value using the binomial model.

$$
P(k \text{ successes}) = {n \choose k} p^k (1-p)^{n-k} 
= \frac{n!}{k!(n-k)!} p^k (1-p)^{n-k}
$$

```
dbinom(k, n, p)
```

