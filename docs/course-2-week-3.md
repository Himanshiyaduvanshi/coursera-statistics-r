# Week 3 {-}

## T-Distribution {-}

### T-Distribution {-}

* When $\sigma$ is unknown (almost always), use the t-distribution to address
the uncertainty of the standard error estimate
* Bell shaped but thicker tails than the normal distribution
* Observations more likely to fall beyond 2 SDs from the mean (more conservative)
* Extra thick tails helpful for mitigating the effect of a less reliable estimate for the standard error of the sampling distribution

* Always centered at 0 (like the standard normal)
* One parameter: **degrees of freedom (df)** - determines the thickness of tails (normal distribution has two parameters: mean and SD)
* When degrees of freedom increases, the shape of the t-distribution approaches the normal distribution

#### T-Score {-}

$$
T = \frac{obs-null}{SE}
$$


```r
# P(|Z| > 2)
pnorm(2, lower.tail = FALSE) * 2
```

```
## [1] 0.04550026
```

```r
# P(|t_df=50| > 2)
pt(2, df = 50, lower.tail = FALSE) * 2
```

```
## [1] 0.05094707
```

```r
# P(|t_df=10| > 2)
pt(2, df = 10, lower.tail = FALSE) * 2
```

```
## [1] 0.07338803
```

### Inference for a Mean {-}

#### Confidence Interval {-}

$$
\bar{x} \pm t^{\star}_{df} SE_\bar{x} \\
\bar{x} \pm t^{\star}_{df} \frac{s}{\sqrt{n}} \\
\bar{x} \pm t^{\star}_{n-1} \frac{s}{\sqrt{n}}
$$

#### Degrees of Freedom for T-Statistic for Inference on One Sample Mean {-}

$$
df = n - 1
$$



```r
# Critical t-score for 0.95 confidence interval with df = 21
qt((1-0.95)/2, df = 21)
```

```
## [1] -2.079614
```

**Example**

Suppose the suggested serving of these biscuits is 30 grams. Do these data provide convincing evidence that the amount of snacks consumed by distracted eaters post lunch is different than the suggested serving size?

* x̄ = 52.1
* s = 45.1
* n = 22
* t_21 = 2.08



```r
# Confidence interval
list(ci = c(52.1 - 2.08 * 45.1/sqrt(22), 52.1 + 2.08 * 45.1/sqrt(22)))
```

```
## $ci
## [1] 32.10007 72.09993
```


```r
# H0: mu = 30
# HA: mu != 30

# T-score
t <- (52.1 - 30) / (45.1/sqrt(22))

# P-value
p_value <- pt(t, df = 21, lower.tail = FALSE) * 2

list(t = t, p_value = p_value)
```

```
## $t
## [1] 2.298408
## 
## $p_value
## [1] 0.03190849
```

### Inference for Comparing Two Independent Means {-}

#### Confidence Interval {-}

$$
(\bar{x}_1 - \bar{x}_2) \pm t^{\star}_{df} SE_{(\bar{x}_1 - \bar{x}_2)} \\
(\bar{x}_1 - \bar{x}_2) \pm t^{\star}_{df} \sqrt{\frac{s^2_1}{n_1} + \frac{s^2_2}{n_2}} \\
(\bar{x}_1 - \bar{x}_2) \pm t^{\star}_{min(n_1 - 1, n_2 - 1）} \sqrt{\frac{s^2_1}{n_1} + \frac{s^2_2}{n_2}}
$$

#### Standard Error of Difference between Two Independent Means {-}

$$
SE_{(\bar{x}_1 - \bar{x}_2)} = \sqrt{\frac{s^2_1}{n_1} + \frac{s^2_2}{n_2}}
$$

#### Degrees of Freedom for T-Statistic for Inference on Difference of Two Means {-}

$$
df = min(n_1 - 1, n_2 - 1）
$$

**Example**

* Solitaire
* x̄ = 52.1
* s = 45.1
* n = 22
* No Distraction
* x̄ = 27.1
* s = 26.4
* n = 22


```r
# Confidence interval

df = 22-1
t_21 = qt((1-0.95)/2, df = 21)

se = sqrt(45.1^2/22 + 26.4^2/22)

ci = c((52.1 - 27.1) - 2.08 * se, (52.1 - 27.1) + 2.08 * se)

# We are 95% confident that those who eat with distractions 
# consume 1.83 g and 48.17 g more snacks than those 
# who eat without distractions, on average.
list(
  df = df,
  t_21 = t_21,
  se = se,
  ci = ci
)
```

```
## $df
## [1] 21
## 
## $t_21
## [1] -2.079614
## 
## $se
## [1] 11.14159
## 
## $ci
## [1]  1.825495 48.174505
```


```r
# H0: mu1 - mu2 = 0
# HA: mu1 - mu2 != 0

# T-score
t <- ((52.1 - 27.1) - 0) / se

# P-value
p_value <- pt(t, df = 21, lower.tail = FALSE) * 2

list(t = t, p_value = p_value)
```

```
## $t
## [1] 2.243845
## 
## $p_value
## [1] 0.03575082
```

### Inference for Comparing Two Paired Means {-}

* Same as the inference for a single population mean, only the mean is a difference between the two paired means.

* $\mu_{\text{diff}}$ : Parameter of Interest
* $\bar{x}_{\text{diff}}$ : Point Estimate

**Example**

* x̄_diff = -0.545
* s_diff = 8.887
* n_diff = 200



```r
# H0: mu_diff = 0
# HA: mu_diff != 0

# Standard error
se <- 8.887 / sqrt(200)

# T-score
t <- (-0.545 - 0) / se

# P-value
p_value <- pt(t, df = 199) * 2

list(se = se, t = t, p_value = p_value)
```

```
## $se
## [1] 0.6284058
## 
## $t
## [1] -0.867274
## 
## $p_value
## [1] 0.3868365
```

### Power {-}

* $\alpha$ : Type I error - P(reject H0 | H0 true)
* $\beta$ : Type II error - P(fail to reject H0 | HA true)
* $1 - \beta$ : **Power** - P(reject H0 | HA true)

**Example**

* sd = 12
* n = 100 (per group)


```r
# H0: mu1 - mu2 = 0
# HA: mu1 - mu2 != 0

# Standard error
list(se = sqrt(12^2/100 + 12^2/100))
```

```
## $se
## [1] 1.697056
```

* For what values of difference would we reject the null hypothesis at the 5% significance level?


```r
1.96 * 1.7
```

```
## [1] 3.332
```

* Suppose the company cares about finding any effect that is 3mmHg or larger vs
the standard medication.
* What is the power of the test that can detect this effect.

* effect size = -3


```r
# Distribution with mu1 - mu2 = -3

z <- (-3.332 - (-3)) / 1.7

# Power of the test
power <- pnorm(z)

list(z = z, power = power)
```

```
## $z
## [1] -0.1952941
## 
## $power
## [1] 0.4225814
```

* What sample size will lead to a power of 80% for this test?


```r
# Z-score that marks the 80th percentile of the normal curve
qnorm(0.8)
```

```
## [1] 0.8416212
```

### Exercises {-}

OpenIntro Statistics, 3rd edition<br>
5.1, 5.3, 5.5, 5.13, 5.17, 5.19, 5.21, 5.23, 5.27, 5.31, 5.35, 5.37<br>
5.39<br>
5.41, 5.43, 5.45, 5.47, 5.49, 5.51

**5.1 Identify the critical t.**

* An independent random sample is selected from an approximately normal population with unknown standard deviation. Find the degrees of freedom and the critical t-value (t*) for the given sample size and confidence level.
* (a) n = 6, CL = 90% 
* (b) n = 21, CL = 98% 
* (c) n = 29, CL = 95% 
* (d) n = 12, CL = 99%


```r
# (a)
qt(.05, 5)
```

```
## [1] -2.015048
```


```r
# (b)
qt(.01, 20)
```

```
## [1] -2.527977
```


```r
# (c)
qt(.025, 28)
```

```
## [1] -2.048407
```


```r
# (d)
qt(.005, 11)
```

```
## [1] -3.105807
```

**5.3 Find the p-value, Part I.** 

* An independent random sample is selected from an approximately normal population with an unknown standard deviation. Find the p-value for the given set of hypotheses and T test statistic. Also determine if the null hypothesis would be rejected at α = 0.05.
* (a) HA : μ > μ0, n = 11, T = 1.91 
* (b) HA : μ < μ0, n = 17, T = −3.45 
* (c) HA : μ != μ0, n = 7, T = 0.83 
* (d) HA : μ > μ0, n = 28, T = 2.1


```r
# (a)
# H0 rejected
pt(1.91, 10, lower.tail = FALSE)
```

```
## [1] 0.04260244
```


```r
# (b)
# H0 rejected
pt(-3.45, 16)
```

```
## [1] 0.001646786
```


```r
# (c)
# H0 not rejected
pt(0.83, 6, lower.tail = FALSE) * 2
```

```
## [1] 0.4383084
```


```r
# (d)
# H0 rejected
pt(2.1, 27, lower.tail = FALSE)
```

```
## [1] 0.02260436
```

**5.5 Working backwards, Part I.** 

* A 95% confidence interval for a population mean, μ, is given as (18.985, 21.015). This confidence interval is based on a simple random sample of 36 observations. 
* Calculate the sample mean and standard deviation. Assume that all conditions necessary for inference are satisfied. Use the t-distribution in any calculations


```r
# t
t <- qt(.025, 35)

# me
me <- (21.015 - 18.985) / 2

# me = t * s / sqrt(n)
# 1.015 = 2.03 * s / sqrt(36)

# s
s <- 1.015 / 2.03 * sqrt(36)

# mean
x <- 18.985 + 1.015

list(t = t, me = me, s = s, mean = x)
```

```
## $t
## [1] -2.030108
## 
## $me
## [1] 1.015
## 
## $s
## [1] 3
## 
## $mean
## [1] 20
```

**5.13 Car insurance savings.** 

* A market researcher wants to evaluate car insurance savings at a competing company. Based on past studies he is assuming that the standard deviation of savings is 100. 
He wants to collect data such that he can get a margin of error of no more than 10 at a 95% confidence level. How large of a sample should he collect?


```r
# s = 100
# me = 10

# me = z * s / sqrt(n)

# 10 = 1.96 * 100 / sqrt(n)
(1.96 * 100 / 10)^2
```

```
## [1] 384.16
```

**5.17 Paired or not, Part I?** 

* In each of the following scenarios, determine if the data are paired.
* (a) Compare pre- (beginning of semester) and post-test (end of semester) scores of students.
* (b) Assess gender-related salary gap by comparing salaries of randomly sampled men and women.
* (c) Compare artery thicknesses at the beginning of a study and after 2 years of taking Vitamin E for the same group of patients.
* (d) Assess effectiveness of a diet regimen by comparing the before and after weights of subjects.



```r
# (a)
# Yes, pre- and post-test scores are paired because they're scores of the same student, and thus are not independent.

# (b)
# No, as data are randomly sampled, they're assuemed to be independent.

# (c)
# Yes, because they're of the same group of patients.

# (d)
# Yes, because they're of the same subjects.
```

**5.19 Global warming, Part I.** 

* Is there strong evidence of global warming? Let’s consider a small scale example, comparing how temperatures have changed in the US from 1968 to 2008. The daily high temperature reading on January 1 was collected in 1968 and 2008 for 51 randomly selected locations in the continental US. Then the difference between the two readings (temperature in 2008 - temperature in 1968) was calculated for each of the 51 different locations. The average of these 51 values was 1.1 degrees with a standard deviation of 4.9 degrees. We are interested in determining whether these data provide strong evidence of temperature warming in the continental US.
* (a) Is there a relationship between the observations collected in 1968 and 2008? Or are the observations in the two groups independent? Explain.
* (b) Write hypotheses for this research in symbols and in words.
* (c) Check the conditions required to complete this test.
* (d) Calculate the test statistic and find the p-value.
* (e) What do you conclude? Interpret your conclusion in context.
* (f) What type of error might we have made? Explain in context what the error means.
* (g) Based on the results of this hypothesis test, would you expect a confidence interval for the average difference between the temperature measurements from 1968 and 2008 to include 0? Explain your reasoning.


```r
# (a)
# The observations collected in 1968 and 2008 are not independent. 
# Although the 51 locations are randomly selected, the observations collected
# in two different years are from the same locations.
# The data are paired.
```


```r
# (b)
# H0: mu_diff = 0
# HA: mu_diff > 0

# The null hypothesis assume that the mean of the difference in temperature in 2008 and 1968 is 0.
# The alternative hypothesis assume that the mean of the difference greater than 0, i.e. warming.
```


```r
# (d)
n = 51
x = 1.1
s = 4.9

se <- 4.9 / sqrt(n)

t <- 1.1 / se

p_value <- pt(t, 50, lower.tail = FALSE) 

list(se = se, t = t, p_value = p_value)
```

```
## $se
## [1] 0.6861372
## 
## $t
## [1] 1.603178
## 
## $p_value
## [1] 0.05759731
```


```r
# (e)
# Assume the significance level to be 0.05, the p-value is not large enough to reject the null hypothesis.
# Hence, there is not enough evidence to say that the temperature is warming in the continental US.
```


```r
# (f)
# Type II error. There is actually temperature warming but we failed to reject the null hypothesis.
```


```r
# (g)
# Yes. Since the null hypothesis is not rejected, the null hypothesis assume that the mean is 0. 
# Hence the confidence interval should include 0.
```

**5.21 Global warming, Part II.** 

* We considered the differences between the temperature readings in January 1 of 1968 and 2008 at 51 locations in the continental US in Exercise 5.19. The mean and standard deviation of the reported differences are 1.1 degrees and 4.9 degrees.
* (a) Calculate a 90% confidence interval for the average difference between the temperature measurements between 1968 and 2008.
* (b) Interpret this interval in context.
* (c) Does the confidence interval provide convincing evidence that the temperature was higher in 2008 than in 1968 in the continental US? Explain



```r
# (a)
# t
t <- qt(0.05, 50)

# me
me <- 1.6759 * se

list(t = t, me = me, ci = c(1.1 - me, 1.1 + me))
```

```
## $t
## [1] -1.675905
## 
## $me
## [1] 1.149897
## 
## $ci
## [1] -0.0498974  2.2498974
```


```r
# (b)
# We are 90% confident that the average difference between the temperature measurements between 1968 and 2008 
# with a sample size of 51 is between -0.05 and 2.25.
```


```r
# (c)
# The confidence interval also has a negative range. Hence there is no convincing evidence that the temperature 
# was higher in 2008 than in 1968.
```

**5.23 Gifted children.** 

* Researchers collected a simple random sample of 36 children who had been identified as gifted in a large city. The following histograms show the distributions of the IQ scores of mothers and fathers of these children. Also provided are some sample statistics.

|/|Mother|Father|Diff|
|---|---|---|---|
|Mean|118.2|114.8|3.4|
|SD|6.5|3.5|7.5|
|n|36|36|36|

* (a) Are the IQs of mothers and the IQs of fathers in this data set related? Explain.
* (b) Conduct a hypothesis test to evaluate if the scores are equal on average. Make sure to clearly state your hypotheses, check the relevant conditions, and state your conclusion in the context of the data


```r
# (a)
# Yes. Since IQ could be a factor affecting marriage. The IQ of mothers and fathers are paired.
```


```r
# (b)

# H0: mu_diff = 0
# HA: mu_diff != 0 

# The null hypothesis assumes that there are no difference between the average IQ of mother and father.
# The alternative assumes that there are difference between the average IQ of mother and father.

# A random sample of 36 obviously will be less than 10% of the population of a large city.
# The distribution of IQ difference is slightly skewed.
# But the sample size of 36 is large enough to meet the condition.
```


```r
se <- 7.5/sqrt(36)
t <- (3.4-0)/se
p_value <- pt(t, 35, lower.tail = FALSE) * 2

list(se = se, t = t, p_value = p_value)
```

```
## $se
## [1] 1.25
## 
## $t
## [1] 2.72
## 
## $p_value
## [1] 0.01009512
```


```r
# With a significance level of 0.05, a p-value of 0.01 is small enough to reject the null hypothesis
# and conclude that our data provide strong evidence that there are difference between the average IQ of mother and father,
# and the data indicate that mothers’ scores are higher than fathers’ scores for the parents of gifted children.
```

**5.27 Friday the 13th, Part I.** 

* In the early 1990’s, researchers in the UK collected data on traffic flow, number of shoppers, and traffic accident related emergency room admissions on Friday the 13th and the previous Friday, Friday the 6th. The histograms below show the distribution of number of cars passing by a specific intersection on Friday the 6th and Friday the 13th for many such date pairs. Also given are some sample statistics, where the difference is the number of cars on the 6th minus the number of cars on the 13th.

|/|6th|13th|Diff|
|---|---|---|---|
|Mean|128385|126550|1835|
|SD|7259|7664|1176|
|n|10|10|10|

* (a) Are there any underlying structures in these data that should be considered in an analysis? Explain.
* (b) What are the hypotheses for evaluating whether the number of people out on Friday the 6th is different than the number out on Friday the 13th? 
* (c) Check conditions to carry out the hypothesis test from part (b).
* (d) Calculate the test statistic and the p-value.
* (e) What is the conclusion of the hypothesis test? 
* (f) Interpret the p-value in this context.
* (g) What type of error might have been made in the conclusion of your test? Explain.


```r
# (a)
# The number of cars on the 6th and the number of cars on the 13th should be paired.
```


```r
# (b)
# H0: mu_diff = 0
# HA: mu_diff != 0
```


```r
# (d)
df = 10 - 1
se = 1176/sqrt(10)
t = (1835 - 0) / se

p_value = pt(t, df, lower.tail = FALSE) * 2

list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 9
## 
## $se
## [1] 371.8839
## 
## $t
## [1] 4.934336
## 
## $p_value
## [1] 0.0008085065
```


```r
# (e)
# The p-value of 0.0008 is much smaller than the significance level of 0.05
# and hence reject the null hypothesis. 
```


```r
# (f)
# The p-value is the probability of observing a difference of the mean of the number of cars
# on 6th and 13th as large as the observation difference under
# the assumption that the null hypothesis is true, i.e. there are no difference.
```


```r
# (g)
# Type I error. There might actually be no difference but we wrongly 
# rejected the null hypothesis and state that there is a difference.
```

**5.31 Chicken diet and weight, Part I.** 

* Chicken farming is a multi-billion dollar industry, and any methods that increase the growth rate of young chicks can reduce consumer costs while increasing company profits, possibly by millions of dollars. An experiment was conducted to measure and compare the effectiveness of various feed supplements on the growth rate of chickens. Newly hatched chicks were randomly allocated into six groups, and each group was given a different feed supplement. Below are some summary statistics from this data set along with box plots showing the distribution of weights by feed type.

|/|Mean|SD|n|
|---|---|---|---|
|casein |323.58 |64.43 |12 |
|<mark>horsebean</mark> |160.20 |38.63 |10 |
|<mark>linseed</mark> |218.75 |52.24| 12 |
|meatmeal |276.91 |64.90 |11 |
|soybean |246.43 |54.13 |14 |
|sunflower |328.92 |48.84 |12 |

* (a) Describe the distributions of weights of chickens that were fed linseed and horsebean.
* (b) Do these data provide strong evidence that the average weights of chickens that were fed linseed and horsebean are different? Use a 5% significance level.
* (c) What type of error might we have committed? Explain.
* (d) Would your conclusion change if we used α = 0.01?


```r
# (a)
# The distribution of chickens fed linseed is normal, whereas that of linseed is slightly skewed.
```


```r
# (b)

# The newly hatched chicks were randomly allocated into groups, there are no evidence of dependent relationship between them.

# H0: mu_linseed - mu_horsebean = 0
# HA: mu_linseed - mu_horsebean != 0

df = min(12,10) - 1
se = sqrt(52.24^2/12 + 38.63^2/10)

t = (218.75-160.2)/se

p_value = pt(t, df, lower.tail = FALSE) * 2

# Reject H0. There is strong evidence that average weights of checking fed linseed is different from horsebean.

list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 9
## 
## $se
## [1] 19.40737
## 
## $t
## [1] 3.016896
## 
## $p_value
## [1] 0.01455232
```


```r
# (c)
# Type I error. Failed to reject H0.
```


```r
# (d)
# If alpha is 0.01, we failed to reject H0 with a p-value of 0.015.
```

**5.35 Gaming and distracted eating, Part I.**

* A group of researchers are interested in the possible effects of distracting stimuli during eating, such as an increase or decrease in the amount of food consumption. 
* To test this hypothesis, they monitored food intake for a group of 44 patients who were randomized into two equal groups. The treatment group ate lunch while playing solitaire, and the control group ate lunch without any added distractions. 
* Patients in the treatment group ate 52.1 grams of biscuits, with a standard deviation of 45.1 grams, and patients in the control group ate 27.1 grams of biscuits, with a standard deviation of 26.4 grams. 
* Do these data provide convincing evidence that the average food intake (measured in amount of biscuits consumed) is different for the patients in the treatment group? Assume that conditions for inference are satisfied.



```r
# Since the two groups of patients are randomly assigned, they're independent of each other.

# H0: mu_a - mu_b = 0
# HA: mu_a - mu_b != 0

df = 22 - 1
se = sqrt(45.1^2/22 + 26.4^2/22)
t = (52.1-27.1-0)/se

p_value = pt(t, df, lower.tail = FALSE) * 2

# Yes.
list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 21
## 
## $se
## [1] 11.14159
## 
## $t
## [1] 2.243845
## 
## $p_value
## [1] 0.03575082
```

**5.37 Prison isolation experiment, Part I.**
* Subjects from Central Prison in Raleigh, NC, volunteered for an experiment involving an “isolation” experience. The goal of the experiment was to find a treatment that reduces subjects’ psychopathic deviant T scores. This score measures a person’s need for control or their rebellion against control, and it is part of a commonly used mental health test called the Minnesota Multiphasic Personality Inventory (MMPI) test. The experiment had three treatment groups: 
* (1) Four hours of sensory restriction plus a 15 minute “therapeutic” tape advising that professional help is available.
* (2) Four hours of sensory restriction plus a 15 minute “emotionally neutral” tape on training hunting dogs.
* (3) Four hours of sensory restriction but no taped message.
* Forty-two subjects were randomly assigned to these treatment groups, and an MMPI test was administered before and after the treatment. Distributions of the differences between pre and post treatment scores (pre - post) are shown below, along with some sample statistics. Use this information to independently test the effectiveness of each treatment. Make sure to clearly state your hypotheses, check conditions, and interpret results in the context of the data.

|/|Tr 1|Tr 2|Tr 3|
|---|---|---|---|
|Mean|6.21|2.86|-3.21|
|SD|12.3|7.94|8.57|
|n|14|14|14|


```r
# The 42 subjects are randomly assigned, hence they're independent of each other.

# Treatment 1 

# H0: mu_diff_1 = 0
# HA: mu_diff_1 > 0

df = 13 - 1 
se = 12.3/sqrt(14)
t = (6.21-0)/se

p_value = pt(t, df, lower.tail = FALSE) 

list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 12
## 
## $se
## [1] 3.287313
## 
## $t
## [1] 1.889081
## 
## $p_value
## [1] 0.04164086
```


```r
# Treatment 2

# H0: mu_diff_2 = 0
# HA: mu_diff_2 > 0

df = 13 - 1 
se = 7.94/sqrt(14)
t = (2.86-0)/se

p_value = pt(t, df, lower.tail = FALSE) 

list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 12
## 
## $se
## [1] 2.122054
## 
## $t
## [1] 1.347751
## 
## $p_value
## [1] 0.1013163
```


```r
# Treatment 3

# H0: mu_diff_3 = 0
# HA: mu_diff_3 > 0

df = 13 - 1 
se = 8.57/sqrt(14)
t = (-3.21-0)/se

p_value = pt(t, df, lower.tail = FALSE) 

list(df = df, se = se, t = t, p_value = p_value)
```

```
## $df
## [1] 12
## 
## $se
## [1] 2.290429
## 
## $t
## [1] -1.401484
## 
## $p_value
## [1] 0.9068002
```

**5.39 Increasing corn yield.**

* A large farm wants to try out a new type of fertilizer to evaluate whether it will improve the farm’s corn production. The land is broken into plots that produce an average of 1,215 pounds of corn with a standard deviation of 94 pounds per plot. The owner is interested in detecting any average difference of at least 40 pounds per plot. How many plots of land would be needed for the experiment if the desired power level is 90%? Assume each plot of land gets treated with either the current fertilizer or the new fertilizer.



```r
# effect size = 40
# power = 0.9

s = 94

# x_old
# x_new

# se = sqrt( 94^2 / n + 94^2 / n )

# 40 = (qnorm(.9) + abs(qnorm(.05/2))) * se
# 40 = 3.24 * sqrt( 94^2 / n + 94^2 / n )
# 40^2 = 3.24^2 * 94^2*2 / n
# n = 3.24^2 * 94^2*2 / 40^2
3.24^2 * 94^2*2 / 40^2
```

```
## [1] 115.946
```

```r
# The sample size should be at least 116 plots
# of land per fertilizer.
```

## ANOVA and Bootstrapping {-}

### Comparing More Than Two Means {-}

* Compare means of 2 groups using a T statistic.
* Compare means of 3+ groups using a new test called **analysis of variance (ANOVA)** and a new statistic called **F**.


* ANOVA
* H0: The mean outcome is the same across all categories.
* HA: At least one pair of means are different from each other.


$$ F = \frac{\text{variability between groups}}{\text{variability within groups}}$$

* Obtaining a large F statistic requires that the variability between sample means is greater than the variability within the samples.

### ANOVA {-}

* Variability partitioning.


* **Group** : **Between group variablity**.
* **Error** : **Within group variablity**.


#### Degrees of Freedom {-}

* **Total degress of freedom** is calculated as sample size minus one.

$$
df_T = n - 1
$$

* **Group degrees of freedom** is calculated as number of groups minus one.

$$
df_G = k - 1
$$

* **Error degrees of freedom** is the difference between the above two DF.

$$
df_E = df_T - df_G
$$

#### Sum of Squares {-}

* **Sum of squares total (SST)** measures the **total variability** in the response variable. 
* Calculated very similarly to variance (except not scaled by the sample size).

$$
SST = \sum^n_{i=1} (y_i - \bar{y})^2
$$

$$
SST = SSG + SSE
$$

* **Sum of squares groups (SSG)** measures the variability **between groups**. 
* It is the **explained variability**.

$$
SSG = \sum^k_{j=1} n_j (\bar{y}_j - \bar{y})^2
$$

* **Sum of squares error (SSE)** measures the variability **within groups**.
* It is the **unexplained variability**, unexplained by the group variable, due to other reasons.

$$
SSE = SST - SSG
$$

#### Mean Squares {-}

* Mean sqares is the average variability between and withing groups, calculated as the total variability (sum of squares) scaled by the associated degress of freedom.

* **Mean squares group (MSG)**

$$
MSG = \frac{SSG}{df_G}
$$

* **Mean squares error (MSE)**

$$
MSE = \frac{SSE}{df_E}
$$

#### F-Statistic {-}

* **F-statistic** is the ratio of the average between group and within 
group variabilities.
* It is never negative. Hence it's right-skewed.

$$
F = \frac{MSG}{MSE}
$$

#### P-Value {-}

* **P-value** is the probability of at least as large a ratio between the "between" and "within" group variabilities if in fact the means of all groups are equal.

**Example**

* F-statistics = 21.735
* DF_G = 3
* DF_E = 791



```r
# If p-value is small (less than alpha), the data provide convincing
# evidence that at least one pair of population means are different
# from each other (but we can't tell which one).

# If p-value is large, the data do not provide convincing evidence that 
# at least one pair of population means are different from each other,
# the observed differences in sample means are attributable to 
# sampling variability (or chance).

pf(q = 21.735, df1 = 3, df2 = 791, lower.tail = FALSE)
```

```
## [1] 1.559855e-13
```

### Conditions for ANOVA {-}

* (1) **Independence**
  * Within groups: sampled observations must be independent.
  * Random sample / assignment
  * Each $n_j$ less than 10% of respective population
  * Between groups: the groups must be independent of each other (non-paired).
  * Carefully consider whether the groups may be dependent -> repeated measures anova
* (2) **Approximate normality**: distribution should be nearly normal within each group.
  * Especially important when sample sizes are small.
* (3) **Equal variance**: groups should have roughly equal variability.
  * Especially important when sample sizes differ between groups.

### Multiple Comparisons {-}

* Which means are different?


* Two sample T tests for differences in each possible pair of groups.
* Multiple tests will inflate the Type I error rate ($\alpha$ significance level).
* Solution: use **modified significance level**.


* Testing many pairs of groups is called **multiple comparisons**.
* The **Bonferroni correction** $\alpha^\star$ suggests that a more stringent significance level is more appropriate for these tests.
* Adjust $\alpha$ by the number of comparisons $K$ being considered.

$$
K = \frac{k(k-1)}{2}
$$

$$
\alpha^\star = \frac{\alpha}{K}
$$

* Constant variance: use consistent standard error and degrees of freedom for all tests.
* Compare the p-values from each test to the modified significance level.


* **Standard error for multiple pairwise comparisons**

$$
SE = \sqrt{
\frac{MSE}{n_1} + \frac{MSE}{n_2}
}
$$

* **Degrees of freedom for multiple pairwise comparisons**

$$
df = df_E
$$

**Example**

* If the explanatory variable in an ANOVA has 3 levels, and the F-test in ANOVA yields a significant result, how many pairwise comparisons are needed to compare each group to one another?



```r
3 * (3-1) / 2
```

```
## [1] 3
```

**Example**

* 4 class levels
* $\alpha$ = 0.05 for the original ANOVA



```r
# Number of comparisons
K <- 4 * (4-1) / 2
# Corrected significance level
alpha_corrected <- 0.05 / K

list(K = K, alpha_corrected = alpha_corrected)
```

```
## $K
## [1] 6
## 
## $alpha_corrected
## [1] 0.008333333
```

* Is there a difference between the average vocabulary scores between middle and lower class Americans> (A single pairwise comparison.)
* DF_E = 691
* MSE = 3.628
* Lower class
* N = 41
* Mean = 5.07
* Middle class
* N = 331
* Mean = 6.76



```r
# H0: mu_middle - mu_lower = 0
# HA: mu_middle - mu_lower != 0

se <- sqrt(3.628/41 + 3.628/331)

t <- ((6.76 - 5.07) - 0) / se

p_value <- pt(t, df = 791, lower.tail = FALSE) * 2

# P-value is smaller than the alpha 0.00833. Reject the null hypothesis.

list(se = se, t = t, p_value = p_value)
```

```
## $se
## [1] 0.3153546
## 
## $t
## [1] 5.359046
## 
## $p_value
## [1] 1.09828e-07
```

### Bootstrapping {-}

* Take a bootstrap sample - a random sample taken **with replacement** from **the original sample**, of **the same size** as the original sample.
* Calculate the bootstrap statistic - a statistic such as mean, median, proportion, etc. computed on the bootstrap samples.
* Repeat the above two steps many times to create a bootstrap distribution - a distribution of bootstrap statistics.


* **Percentile method**
* **Standard error method**


* Not as rigid conditions as CLT based methods.
* If the bootstrap distribution is extremely skewed or sparse, the bootstrap interval might be unreliable.
* A representative sample is still required - if the sample is biased, the estimates resulting from this sample will also be biased.

### Exercises {-}

OpenIntro Statistics, 3rd edition<br>
5.41, 5.43, 5.45, 5.47, 5.49, 5.51

**5.41 Fill in the blank.**

* When doing an ANOVA, you observe large differences in means between groups. Within the ANOVA framework, this would most likely be interpreted as evidence strongly favoring the ? hypothesis.



```r
# alternative
```

**5.43 Chicken diet and weight, Part III.**

* In Exercises 5.31 and 5.33 we compared the effects of two types of feed at a time. A better analysis would first consider all feed types at once: casein, horsebean, linseed, meat meal, soybean, and sunflower. The ANOVA output below can be used to test for differences between the average weights of chicks on different diets.

|/|Df |Sum Sq |Mean Sq |F value |Pr(>F) |
|---|---|---|---|---|---|
|feed |5 |231,129.16 |46,225.83 |15.36 |0.0000 |
|Residuals |65 |195,556.02 |3,008.55 | 

* Conduct a hypothesis test to determine if these data provide convincing evidence that the average weight of chicks varies across some (or all) groups. Make sure to check relevant conditions. Figures and summary statistics are shown below.



```r
# H0: The mean outcome is the same across all feed types.
# HA: At least one pair of means are different from each other.

# F = MSG / MSE
f = 46225.83/3008.55

p_value = pf(f, 5, 65, lower.tail = FALSE)

list(f = f, p_value = p_value)
```

```
## $f
## [1] 15.36482
## 
## $p_value
## [1] 5.936286e-10
```

**5.45 Coffee, depression, and physical activity.** 

* Caffeine is the world’s most widely used stimulant, with approximately 80% consumed in the form of coffee. Participants in a study investigating the relationship between coffee consumption and exercise were asked to report the number of hours they spent per week on moderate (e.g., brisk walking) and vigorous (e.g., strenuous sports and jogging) exercise. Based on these data the researchers estimated the total hours of metabolic equivalent tasks (MET) per week, a value always greater than 0. The table below gives summary statistics of MET for women in this study based on the amount of coffee consumed.

|/|≤ 1 cup/week |2-6 cups/week |1 cup/day |2-3 cups/day |≥ 4 cups/day |Total |
|---|---|---|---|---|---|---|
|Mean |18.7 |19.6 |19.3 |18.9 |17.5 ||
|SD |21.1 |25.5 |22.5 |22.0 |22.0 ||
|n |12,215 |6,617 |17,234 |12,290 |2,383 |50,739|

* (a) Write the hypotheses for evaluating if the average physical activity level varies among the different levels of coffee consumption.
* (b) Check conditions and describe any assumptions you must make to proceed with the test.
* (c) Below is part of the output associated with this test. Fill in the empty cells.
* (d) What is the conclusion of the test?


```r
# (a)
# H0: The mean MET is the same across all groups of coffee consumption.
# HA: At least one pair of means is different.
```



```r
# (c)
df_total = 50739 - 1
df_coffee = 5 - 1
df_residuals = df_total - df_coffee

list(
  df_total = df_total, 
  df_coffee = df_coffee, 
  df_residuals = df_residuals
)
```

```
## $df_total
## [1] 50738
## 
## $df_coffee
## [1] 4
## 
## $df_residuals
## [1] 50734
```


```r
sum_sq_coffee = 25575327 - 25564819

list(sum_sq_coffee = sum_sq_coffee)
```

```
## $sum_sq_coffee
## [1] 10508
```


```r
mean_sq_coffee = 10508 / df_coffee
mean_sq_residuals = 25564819 / df_residuals

list(mean_sq_coffee, mean_sq_residuals)
```

```
## [[1]]
## [1] 2627
## 
## [[2]]
## [1] 503.8991
```


```r
# F = MSG / MSE
f = mean_sq_coffee / mean_sq_residuals

list(f = f)
```

```
## $f
## [1] 5.213345
```


```r
pf(5.21, 4, 50734, lower.tail = FALSE)
```

```
## [1] 0.0003412589
```

|/|Df |Sum Sq |Mean Sq |F value |Pr(>F) |
|---|---|---|---|---|---|
|coffee| 4| 10508| 2627| 5.21|**0.0003** |
|Residuals| 50734|**25,564,819**| 503.9| /|/ |
|Total| 50738|**25,575,327**| /| /| /|


```r
# (d)
# Since p-value is very small,
# reject the null hypothesis and conclude that there is
# at least one mean of MET that is different.
```

**5.47 GPA and major.** 

* Undergraduate students taking an introductory statistics course at Duke University conducted a survey about GPA and major. The side-by-side box plots show the distribution of GPA among three groups of majors. Also provided is the ANOVA output.

|/|Df |Sum Sq |Mean Sq |F value |Pr(>F) |
|---|---|---|---|---|---|
|major| 2| 0.03| 0.015| 0.185| 0.8313|
|Residuals| 195| 15.77| 0.081| /|/ |

* (a) Write the hypotheses for testing for a difference between average GPA across majors.
* (b) What is the conclusion of the hypothesis test? 
* (c) How many students answered these questions on the survey, i.e. what is the sample size?



```r
# (a)
# H0: The average GPA across majors is the same.
# HA: At least one pair of average GPA is different.
```



```r
# (b)
# The p-value is too large and we cannot reject the null hypothesis.
# We cannot conclude that there're any difference between 
# average GPA across majors.
```



```r
# (c)
195+2+1
```

```
## [1] 198
```



**5.49 True / False: ANOVA, Part I.** 

* Determine if the following statements are true or false in ANOVA, and explain your reasoning for statements you identify as false.
* (a) As the number of groups increases, the modified significance level for pairwise tests increases as well.
* (b) As the total sample size increases, the degrees of freedom for the residuals increases as well.
* (c) The constant variance condition can be somewhat relaxed when the sample sizes are relatively consistent across groups.
* (d) The independence assumption can be relaxed when the total sample size is large.



```r
# (a)
# K = (k * (k-1)) / 2
# a_modified = a / K

# False
# The larger the number of groups, the larger the number of comparisons K,
# hence the smaller the modified significance level.
```



```r
# (b)
# df_residuals = df_total - df_group
# df_residuals = (n) - (k - 1)

# True
```



```r
# (c)
# True
```



```r
# (d)
# False
```

**5.51 Prison isolation experiment, Part II.**
* Exercise 5.37 introduced an experiment that was conducted with the goal of identifying a treatment that reduces subjects’ psychopathic deviant T scores, where this score measures a person’s need for control or his rebellion against control. In Exercise 5.37 you evaluated the success of each treatment individually. An alternative analysis involves comparing the success of treatments. The relevant ANOVA output is given below.

|/|Df |Sum Sq |Mean Sq |F value |Pr(>F) |
|---|---|---|---|---|---|
|treatment |2 |639.48 |319.74 |3.33 |0.0461 |
|Residuals |39 |3740.43 |95.91 |
*s_pooled = 9.793 on df = 39*

* (a) What are the hypotheses? 
* (b) What is the conclusion of the test? Use a 5% significance level.
* (c) If in part (b) you determined that the test is significant, conduct pairwise tests to determine which groups are different from each other. If you did not reject the null hypothesis in part (b), recheck your answer.

|/|Tr 1|Tr 2|Tr 3|
|---|---|---|---|
|Mean|6.21|2.86|-3.21|
|SD|12.3|7.94|8.57|
|n|14|14|14|



```r
# (b)
# Since p is smaller than 0.05, reject the null hypothesis
# and conclude that at least one pair of mean score is different.
```



```r
# (c)

# K = (k * (k-1)) / 2
K = 3 * 2 / 2

# a_modified = a / K
a_modified = 0.05 / K

list(K = K, a_modified = a_modified)
```

```
## $K
## [1] 3
## 
## $a_modified
## [1] 0.01666667
```


```r
s_pooled = 9.793
df = 39

se = sqrt(9.793^2/14 + 9.793^2/14)

# tr 1, tr 2
t1 = (6.21-2.86)/se
p1 = pt(t1, df, lower.tail = FALSE) * 2

# tr 1, tr 3
t2 = (6.21-(-3.21))/se
p2 = pt(t2, df, lower.tail = FALSE) * 2

# tr 1, tr 3
t3 = (2.86-(-3.21))/se
p3 = pt(t3, df, lower.tail = FALSE) * 2

list(
  se = se,
  tr1 = list(t = t1, p = p1),
  tr2 = list(t = t2, p = p2),
  tr3 = list(t = t3, p = p3)
)
```

```
## $se
## [1] 3.701406
## 
## $tr1
## $tr1$t
## [1] 0.9050615
## 
## $tr1$p
## [1] 0.3709903
## 
## 
## $tr2
## $tr2$t
## [1] 2.544979
## 
## $tr2$p
## [1] 0.01499763
## 
## 
## $tr3
## $tr3$t
## [1] 1.639917
## 
## $tr3$p
## [1] 0.1090649
```

