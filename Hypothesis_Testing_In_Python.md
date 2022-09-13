# Yum, That Dish Tests Good

## Calculating a sample mean

```
# Print the late_shipments dataset
print(late_shipments)

# Calculate the proportion of late shipments = sample statistic
late_prop_samp = late_shipments['late'].value_counts(normalize=True)

# Print the results
print(late_prop_samp)
```

## Calculating a z-score

```
# Hypothesize that the proportion is 6%
late_prop_hyp = 0.06

# Calculate the standard error
std_error = np.std(late_shipments_boot_distn, ddof=1)

# Find z-score of late_prop_samp
z_score = (late_prop_samp - late_prop_hyp) / std_error

# Print z_score
print(z_score)

```

## Calculating p-values

```
# Calculate the z-score of late_prop_samp
z_score = (late_prop_samp - late_prop_hyp) / std_error

# Calculate the p-value
p_value = 1 - norm.cdf(z_score, loc=0, scale=1)

# Print the p-value
print(p_value)
```

# Pass Me ANOVA Glass of Iced t

## Two sample mean test statistic

```
# Calculate the numerator of the test statistic
numerator = xbar_no  - xbar_yes

# Calculate the denominator of the test statistic
denominator = np.sqrt(s_no**2/n_no + s_yes**2/n_yes)

# Calculate the test statistic
t_stat = numerator / denominator

# Print the test statistic
print(t_stat)
```

## From t to p

```
# Calculate the degrees of freedom
degrees_of_freedom = n_no + n_yes - 2

# Calculate the p-value from the test stat
p_value = t.cdf(t_stat, df=degrees_of_freedom)

# Print the p_value
print(p_value)
```

When the standard error is estimated from the sample standard deviation and sample size, the test statistic is transformed into a p-value using the t-distribution.

## Visualizing the difference

```
# Calculate the differences from 2012 to 2016
sample_dem_data['diff'] = sample_dem_data['dem_percent_12'] - sample_dem_data['dem_percent_16']

# Find the mean of the diff column
xbar_diff = sample_dem_data['diff'].mean()

# Find the standard deviation of the diff column
s_diff = sample_dem_data['diff'].std()

# Plot a histogram of diff with 20 bins
sample_dem_data['diff'].hist(bins=20)
plt.show()
```

## Using ttest()

```
# Conduct a t-test on diff
test_results = pingouin.ttest(x=sample_dem_data['diff'],
                              y=0,
                              alternative="two-sided")

# Conduct a paired t-test on dem_percent_12 and dem_percent_16
paired_test_results = pingouin.ttest(x=sample_dem_data['dem_percent_12'],
                                     y=sample_dem_data['dem_percent_16'],
                                     paired=True,
                                    alternative="two-sided")




# Print the paired test results
print(paired_test_results)
```

## Visualizing many categories

```
# Calculate the mean pack_price for each shipment_mode
xbar_pack_by_mode = late_shipments.groupby("shipment_mode")['pack_price'].mean()

# Calculate the standard deviation of the pack_price for each shipment_mode
s_pack_by_mode = late_shipments.groupby("shipment_mode")['pack_price'].std()

# Boxplot of shipment_mode vs. pack_price
sns.boxplot(x='pack_price', y='shipment_mode', data=late_shipments)
plt.show()
```

## Pairwise t-tests

No p-value adjustment

```
# Perform a pairwise t-test on pack price, grouped by shipment mode
pairwise_results = pingouin.pairwise_ttests(data=late_shipments,
                                    dv='pack_price',
                                    between='shipment_mode',
                                    padjust='none')


# Print pairwise_results
print(pairwise_results)
```

With Bonferroni p-value adjustment

```
# Modify the pairwise t-tests to use Bonferroni p-value adjustment
pairwise_results = pingouin.pairwise_ttests(data=late_shipments,
                                            dv="pack_price",
                                            between="shipment_mode",
                                            padjust="bonf")

# Print pairwise_results
print(pairwise_results)
```

# Letting the Categoricals Out of the Bag

## Test for single proportions

```
# Hypothesize that the proportion of late shipments is 6%
p_0 = 0.06

# Calculate the sample proportion of late shipments
p_hat = (late_shipments['late'] == "Yes").mean()

# Calculate the sample size
n = len(late_shipments)

# Calculate the numerator and denominator of the test statistic
numerator = p_hat - p_0
denominator = np.sqrt(p_0 * (1 - p_0) / n)

# Calculate the test statistic
z_score = numerator / denominator

# Calculate the p-value from the z-score
p_value = 1 - norm.cdf(z_score)

# Print the p-value
print(p_value)
```

## Test for two proportions

```
# Calculate the pooled estimate of the population proportion
p_hat = (p_hats["reasonable"] * ns["reasonable"] + p_hats["expensive"] * ns["expensive"]) / (ns["reasonable"] + ns["expensive"])

# Calculate p_hat one minus p_hat
p_hat_times_not_p_hat = p_hat * (1 - p_hat)

# Divide this by each of the sample sizes and then sum
p_hat_times_not_p_hat_over_ns = p_hat_times_not_p_hat / ns["expensive"] + p_hat_times_not_p_hat / ns["reasonable"]

# Calculate the standard error
std_error = np.sqrt(p_hat_times_not_p_hat_over_ns)

# Calculate the z-score
z_score = (p_hats["expensive"] - p_hats["reasonable"]) / std_error

# Calculate the p-value from the z-score
p_value = 1 - norm.cdf(z_score)

# Print p_value
print(p_value)
```

## proportions_ztest() for two samples

```
# Count the late column values for each freight_cost_group
late_by_freight_cost_group = late_shipments.groupby("freight_cost_group")['late'].value_counts()
print(late_by_freight_cost_group)

# Create an array of the "Yes" counts for each freight_cost_group
y_exp = late_by_freight_cost_group[('expensive', 'Yes')]
y_reas = late_by_freight_cost_group[('reasonable', 'Yes')]
success_counts = np.array([y_exp, y_reas])
print(success_counts)

# Create an array of the total number of rows in each freight_cost_group
n_exp = late_by_freight_cost_group[('expensive', 'Yes')] + late_by_freight_cost_group[('expensive', 'No')]
n_reas = late_by_freight_cost_group[('reasonable', 'Yes')] + late_by_freight_cost_group[('reasonable', 'No')]
n = np.array([n_exp, n_reas])
print(n)

# Run a z-test on the two proportions
stat, p_value = proportions_ztest(count=success_counts, nobs=n, alternative='larger')

# Print the results
print(stat, p_value)
```

## Chi-square test of independence

```
# Proportion of freight_cost_group grouped by vendor_inco_term
props = late_shipments.groupby('vendor_inco_term')['freight_cost_group'].value_counts(normalize=True)

# Convert props to wide format
wide_props = props.unstack()

# Proportional stacked bar plot of freight_cost_group vs. vendor_inco_term
wide_props.plot(kind="bar", stacked=True)
plt.show()

# Determine if freight_cost_group and vendor_inco_term are independent
expected, observed, stats = pingouin.chi2_independence(data=late_shipments, x='vendor_inco_term', y='freight_cost_group')

# Print results
print(stats[stats['test'] == 'pearson'])
```

## Visualizing goodness of fit

```
# Find the number of rows in late_shipments
n_total = len(late_shipments)

# Create n column that is prop column * n_total
hypothesized["n"] = hypothesized["prop"] * n_total

# Plot a yellow bar graph of n vs. vendor_inco_term for incoterm_counts
plt.bar(incoterm_counts['vendor_inco_term'], incoterm_counts['n'], color="yellow", alpha=0.5)

# Add a blue scatter plot for the hypothesized counts
plt.scatter(hypothesized['vendor_inco_term'], hypothesized['n'], color='blue')
plt.show()
```

## Chi-square test of goodness of fit

The test to compare the proportions of a categorical variable to a hypothesized distribution is called a chi-square goodness of fit test.

```
# Perform a goodness of fit test on vendor_inco_term
gof_test = chisquare(f_obs=incoterm_counts['n'], f_exp=hypothesized['n'])


# Print gof_test results
print(gof_test)
```

# Time to Define the Relationship

## Testing sample size

```
# Count the freight_cost_group values
counts = late_shipments['freight_cost_group'].value_counts()

# Print the result
print(counts)

# Inspect whether the counts are big enough for two sample t-test
print((counts >= 30).all())
```

```
# Count the late values
counts = late_shipments['late'].value_counts()

# Print the result
print(counts)

# Inspect whether the counts are big enough for a one sample proportion test
print((counts >= 10).all())
```

```
# Count the values of freight_cost_group grouped by vendor_inco_term
counts = late_shipments.groupby('vendor_inco_term')['freight_cost_group'].value_counts()

# Print the result
print(counts)

# Inspect whether the counts are big enough for a chi-square independence test
print((counts >= 5).all())
```

```
# Count the shipment_mode values
counts = late_shipments['shipment_mode'].value_counts()

# Print the result
print(counts)

# Inspect whether the counts are big enough for an ANOVA test
print((counts >= 30).all())
```

## Wilcoxon signed-rank test

Here, because the sample size was 500, the p-value results between
the ttest and the wilcoxon test were both small, so they got similar values.
This gives us more confidence that our assumptions are correct.

```
# Conduct a paired t-test on dem_percent_12 and dem_percent_16
paired_test_results = pingouin.ttest(x=sample_dem_data['dem_percent_12'],
                                y=sample_dem_data['dem_percent_16'],
                                paired=True,
                                alternative='greater')

# Print paired t-test results
print(paired_test_results)
```

```
# Conduct a Wilcoxon test on dem_percent_12 and dem_percent_16
wilcoxon_test_results = pingouin.wilcoxon(x=sample_dem_data['dem_percent_12'],
                                        y=sample_dem_data['dem_percent_16'],
                                        alternative='greater')

# Print Wilcoxon test results
print(wilcoxon_test_results)

```

## Wilcoxon-Mann-Whitney

One common rank-based test is the Wilcoxon-Mann-Whitney test, which is like a non-parametric t-test.

```
# Select the weight_kilograms and late columns
weight_vs_late = late_shipments[['weight_kilograms', 'late']]

# Convert weight_vs_late into wide format
weight_vs_late_wide = weight_vs_late.pivot(columns='late',
                                           values='weight_kilograms')

# Run a two-sided Wilcoxon-Mann-Whitney test on weight_kilograms vs. late
wmw_test = pingouin.mwu(x=weight_vs_late_wide['Yes'], y=weight_vs_late_wide['No'],
                        alternative='two-sided')

# Print the test results
print(wmw_test)
```

## Kruskal-Wallis

The Kruskal-Wallis test is a non-parametric version of an ANOVA test, comparing the means across multiple groups.

```
# Run a Kruskal-Wallis test on weight_kilograms vs. shipment_mode
kw_test = pingouin.kruskal(data=late_shipments, dv='weight_kilograms',
                        between='vendor_inco_term')

# Print the results
print(kw_test)
```

# Chapter Summary

## Chap 1

1. Proportion test - one sample problem

a. z-Score: stat_mean - Hyp-mean / std_error

b. p-value: `scipy.stats.norm.cdf(z_score, loc, scale)`

## Chap 2

1. Two-sample problems - compare sample statistic across groups of a variable

ex: numerical variable compared to categorical variable

a. t test statistic follows a t-distribution and has a parameter called degrees of freedom

b. p-value: `scipy.stats.t.cdf(t_stat, df=degrees_of_freedom)`

2. Two-sample with paired data

paired data are data that are not independent
can use the diff between the two variables

- `t.cdf(t_stat, df=n_diff-1)`

pingouin package with diff

- `pingouin.ttest(x=sample_data['diff'], y=0, alternative='less)`

pingouin package with two variables instead of diff

- `pingouin.ttest(x=sample_data['col1'], y=sample_data['col2'], paired=True, alternative='less)`

3. More than 2 groups

test for differences between groups

- `pingouin.anova(data=stack_overflow, dv="converted_comp", between="job_sat")`

identify which category is different

- `pingouin.pairwise_tests(data=stack_overflow, dv="converted_comp", between="job_sat", padjust="none")`

## Chap 3

1. Use simplified Standard Error calculation instead of bootstrap_dstn

- `norm.cdf(z_score)`

2. Two sample case using standard error equation

- `statsmodels.stats.proportions.proportions_ztest(count=n_hobbyists, nobs=nrows, alternative='two-sided')`

3. Test for independence of variables

- `expected, observed, stats = pingouin.chi2_independence(data=stack_overflow, x='hobbyist', y='age_cat', correction=False)`

4. Check proportions of each level of categorical variable to hypothesized values

- `scipy.stats.chisquare(f_obs=purple_link_counts['n'], f_exp=hypothesized['n'])`

## Chap 4

1. Wilcoxon-signed rank test

Works on the ranked absolute differences between the pairs of data
Used for paired data

- `scipy.stats.rankdata(x) : where x = [1, 15, 3, 10, 6]`
- `pingouin.wilcoxon(x=repub_votes['percent_08'], y=repub_votes['percent_12'], alternative='less')`

2. Wilcoxon-Mann-Whitney

Basically a t-test on the ranks of numeric input, for non-paired data

- `pingouin.mwu(x=age_vs_comp_wide['child'], y=age_vs_comp_wide['adult'], alternative='greater')`

3. Kruskal-Wallis test

Non-parametric version of the Anova test
Extends the Wilcoxon-Mann-Whitney test to more than 2 groups

- `pingouin.kruskal(data=stack_overflow, dv='converted_comp', between='job_sat')`