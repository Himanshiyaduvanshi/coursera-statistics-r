# Inference for Two Proportions (Two Levels)

## Definition

**Population Parameter — Difference between Two Population Proportions** $p1 - p2$ 

- To estimate the difference between two proportions, we label one of our categorical variables the explanatory variable and the other one our response variable.

**Point Estimate — Difference between Two Sample Proportions** $\hat{p}_1 - \hat{p}_2$

**Number of Samples** $n_1, n_2$

## Theoretical-Based Inference

### Normal Distribution

#### Conditions for Central Limit Theorem

**Independence**

- Random sampling / random assignment.
- The random samples are collected from less than 10% of the population.
- The data are independent within and between the two groups (non-paired).
Generally this is satisfied if the data come from two independent random samples or if the data come from a randomized experiment.

**Success-Failure Condition**

- The success-failure condition expect to see at least 10 success and 10 failure in the sample.
- The success-failure condition holds for both groups, where we check successes and failures in each group separately.

$$
\begin{aligned}
n p \ge 10 \\
n(1-p) \ge 10
\end{aligned}
$$

#### Sampling Distribution

**Standard Error** $SE_{\hat{p}_1 - \hat{p}_2}$

$$
SE_{\hat{p}_1 - \hat{p}_2} = \sqrt{
\frac{{p_1}(1-{p}_1)}{n_1} + \frac{{p_2}(1-{p}_2)}{n_2}
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

**Standard Error** $SE_{\hat{p}_1 - \hat{p}_2}$

- For computing the confidence interval, we use $\hat{p}$ in place of $p$ to compute the standard error.

$$
SE_{\hat{p}_1 - \hat{p}_2} = \sqrt{
\frac{\hat{p_1}(1-\hat{p}_1)}{n_1} + \frac{\hat{p_2}(1-\hat{p}_2)}{n_2}
}
$$

**Critical Z-Score** $z^\star$

```r
z = qnorm((1 - ci_level)/2, df)
```

**Confidence Interval**

$$
\hat{p}_1 - \hat{p}_2 \pm z^\star \times SE_{\hat{p}_1 - \hat{p}_2} 
$$

#### Hypothesis Testing

**Pooled Sample Proportion** $\hat{p}_{pool}$

- If our null value set up in the hypothesis is $0$, i.e. $H_0: p_1 - p_2 = 0$, we have to use the pooled sample proportion for checking the conditions and computing the standard error. 

$$
\begin{aligned}
\hat{p}_{pool} 
&= \frac{\text{number of succcess}}
{\text{total number of cases}} \\
&= \frac{\hat{p}_1 n_1 + \hat{p}_2 n_2}{n_1 + n_2}
\end{aligned}
$$

**Success-Failure Condition**

- If the null value is $0$, we need to use the pooled proportion $\hat{p}_{pool}$ to check the conditions.

$$
\begin{aligned}
n_1 \hat{p}_{pool} \ge 10 \\
n_1 (1 - \hat{p}_{pool}) \ge 10 \\
n_2 \hat{p}_{pool} \ge 10 \\ 
n_2 (1 - \hat{p}_{pool}) \ge 10 
\end{aligned}
$$

- If the null value is not $0$, use $\hat{p}$ for each levels.

$$
\begin{aligned}
n \hat{p} \ge 10 \\
n(1-\hat{p}) \ge 10
\end{aligned}
$$

**Standard Error (Pooled)** $SE_{pool}$

- If the null value is $0$, we use the pooled proportion $\hat{p}_{pool}$ to compute the standard error.

$$
SE_{pool} = \sqrt{
\frac{\hat{p}_{pool}(1-\hat{p}_{pool})}{n_1} + \frac{\hat{p}_{pool}(1-\hat{p}_{pool})}{n_2}
}
$$

- If the null value is not $0$, use $\hat{p}$ for each levels.

$$
SE_{\hat{p}_1 - \hat{p}_2} = \sqrt{
\frac{\hat{p_1}(1-\hat{p}_1)}{n_1} + \frac{\hat{p_2}(1-\hat{p}_2)}{n_2}
}
$$

**Hypothesis Test**

- When we conduct a 2-proportion hypothesis test, usually $H_0$ is $p_1 - p_2 = 0$. 
- However, there are rare situations where we want to check for some difference in p1and p2that is some value other than $0$.

$$
\begin{aligned}
H_0: p_1 - p_2 &= \text{null value} \\
H_A: p_1 - p_2 &\ge or \le or \ne \text{null value}
\end{aligned}
$$

**Z-Score** $Z$

$$
Z = \frac{(\hat{p}_1 - \hat{p}_2) - \text{null value}}{SE_{\hat{p}_1 - \hat{p}_2}}
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
