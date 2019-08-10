# Inference for Two Proportions (Many Levels)

## Definition

**Population Distribution** 

* The goal is to evaluate whether the response variable is independent of the explanatory variable.

**Sample Distribution** 

**Number of Columns** $C$

- The column level represents the explanatory variable.

**Number of Rows** $R$

- The row level represents the response variable.

**Number of Cells** $k$

$$
k = R \times C
$$

**Two-Way Table**

|Levels 1 / 2 | $C_1$| $C_2$| ...| $C_j$|
|:-------------------|-----:|-----:|---:|-----:|
|$R_1$|$O_{11}$ $E_{11}$|$O_{12}$ $E_{12}$|...|$O_{1j}$ $E_{1j}$|
|$R_2$|$O_{21}$ $E_{21}$|$O_{22}$ $E_{22}$|...|$O_{2j}$ $E_{2j}$|
|...|...|...|...|...|
|$R_i$|$O_{i1}$ $E_{i1}$|$O_{i2}$ $E_{i2}$|...|$O_{ij}$ $E_{ij}$|

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

![image-20190810161512876](/Users/cecilia/Dropbox/dropbox/repos/github/coursera-statistics-r/notes/images/image-20190810161512876.png)

**Degrees of Freedom** $df$

$$
df = (R - 1) \times (C - 1)
$$

- The calculation of $df$ is different from that of the GOF test. 
- It is defined as the number of rows $R$ multiplied by the number of columns $C$.

#### Independence Test (Hypothesis Test)

**Hypothesis Test**

**Observed Count** $O$

**Expected Count** $E$

$$
E_{\text{row}_i, \text{col}_j} = 
\frac{(\text{row}_i \text{ total}) \times 
(\text{col}_j \text{ total})}
{\text{table total}}
$$

**Chi-Square Statistic** $\chi^2$

$$
\chi^2 = \sum^k_{i=1} \frac{(O_i-E_i)^2}{E_i}
$$

- $O$ : Observed counts.

- $E$ : Expected counts.
- $k$ : Number of cells.

```r
sum(mapply(function(o, e) (o-e)^2/e, observed, expected)) 
```

**P-Value**

- The p-value for a chi-square test is defined as the tail area above the calculated test statistic.
- Because the test statistic is always positive, a higher test statistic means a higher deviation from the null hypothesis.

```r
p_value = pchisq(chi2, df, lower.tail = FALSE) 
```

