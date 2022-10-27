# Seaborn Introduction


## Distribution plots
```
# kind = 'hist' by default
sns.displot(df['Award_Amount'])

# Create a displot
sns.displot(df['Award_Amount'],kde=False,bins=20)

sns.displot(df['Award_Amount'],kind='kde',rug=True,fill=True)
```

## Regression analysis - bivariate

- `regplot` is lowlevel
- `lmplot` is highlevel and more powerful, supports faceting with `hue` and `col`

```
sns.regplot(x="insurance_losses", y="premiums", data=df)

sns.lmplot(x="insurance_losses", y="premiums", data=df)

sns.lmplot(data=df,x="insurance_losses",y="premiums",hue="Region")

sns.lmplot(data=df,x="insurance_losses",y="premiums",row="Region")
```

# Customizing Seaborn 


## Setting styles
```
# Set the default seaborn style
sns.set()

# Set style to 'dark'
sns.set_style('dark')
sns.distplot(df['fmr_2'])

# Set style to 'whitegrid'
sns.set_style('whitegrid')
sns.distplot(df['fmr_2'])


# Remove the spines, removes top and right spines by default 
sns.despine()
```

## Colors

```
# Set style, enable color code, and create a magenta distplot
# color_codes maps matplotlib colors to sns palette
sns.set(color_codes=True)
sns.distplot(df['fmr_3'], color='m')


# Loop through differences between bright and colorblind palettes
for p in ['bright', 'colorblind']:
    sns.set_palette(p)
    sns.distplot(df['fmr_3'])
    plt.show()


# Creates a sequential purples palette containing 8 colors
sns.palplot(sns.color_palette('Purples', 8))
plt.show()

sns.palplot(sns.color_palette('husl', 10))
plt.show()

sns.palplot(sns.color_palette('coolwarm', 6))
plt.show()

```

## Customize sns via matplotlib

- Setting labels
```
# Create a figure and axes
fig, ax = plt.subplots()

# Plot the distribution of data
sns.distplot(df['fmr_3'], ax=ax)

# Create a more descriptive x axis label
ax.set(xlabel="3 Bedroom Fair Market Rent")

# Show the plot
plt.show()
```

- Adding annotations
```
# Create a figure and axes. Then plot the data
fig, ax = plt.subplots()
sns.distplot(df['fmr_1'], ax=ax)

# Customize the labels and limits
ax.set(xlabel="1 Bedroom Fair Market Rent", xlim=(100,1500), title="US Rent")

# Add vertical lines for the median and mean
ax.axvline(x=df['fmr_1'].median(), color='m', label='Median', linestyle='--', linewidth=2)
ax.axvline(x=df['fmr_1'].mean(), color='b', label='Mean', linestyle='-', linewidth=2)

# Show the legend and plot the data
ax.legend()
plt.show()
```

- Multiple plots
```
# Create a plot with 1 row and 2 columns that share the y axis label
fig, (ax0, ax1) = plt.subplots(nrows=1, ncols=2, sharey=True)

# Plot the distribution of 1 bedroom apartments on ax0
sns.distplot(df['fmr_1'], ax=ax0)
ax0.set(xlabel="1 Bedroom Fair Market Rent", xlim=(100,1500))

# Plot the distribution of 2 bedroom apartments on ax1
sns.distplot(df['fmr_2'], ax=ax1)
ax1.set(xlabel="2 Bedroom Fair Market Rent", xlim=(100,1500))

# Display the plot
plt.show()
```

# Additional Plot Types



- Categorical Plots
```
# Create the stripplot
sns.stripplot(data=df,x='Award_Amount',y='Model Selected',jitter=True)
plt.show()

# Create and display a swarmplot with hue set to the Region
sns.swarmplot(data=df,x='Award_Amount',y='Model Selected',hue='Region')
plt.show()

# Create a boxplot
sns.boxplot(data=df,x='Award_Amount',y='Model Selected')
plt.show()

# Create a violinplot
sns.boxplot(data=df,x='Award_Amount',y='Model Selected', palette='husl')
plt.show()

# Create a lvplot with the Paired palette and the Region column as the hue
sns.boxenplot(data=df,x='Award_Amount',y='Model Selected',palette='Paired',hue='Region')
plt.show()

# Show a countplot with the number of models used with each region a different color
sns.countplot(data=df,y="Model Selected",hue="Region")
plt.show()

# Create a pointplot and include the capsize in order to show caps on the error bars
sns.pointplot(data=df,y='Award_Amount',x='Model Selected',capsize=.1)
plt.show()

# Create a barplot with each Region shown as a different color
sns.barplot(data=df,y='Award_Amount',x='Model Selected',hue='Region')
plt.show()

```

- Regression Plots
```
# Display a regression plot for Tuition
sns.regplot(data=df,y='Tuition',x="SAT_AVG_ALL",marker='^',color='g')
plt.show()

# Display the residual plot
sns.residplot(data=df,y='Tuition',x="SAT_AVG_ALL",color='g')
plt.show()

# Plot a regression plot of Tuition and the Percentage of Pell Grants
sns.regplot(data=df,y='Tuition',x="PCTPELL")
plt.show()

# Create another plot that estimates the tuition by PCTPELL
sns.regplot(data=df,y='Tuition',x="PCTPELL",x_bins=5)
plt.show()

# The final plot should include a line using a 2nd order polynomial
sns.regplot(data=df,y='Tuition',x="PCTPELL",x_bins=5,order=2)
plt.show()

# Create a scatter plot by disabling the regression line
sns.regplot(data=df,y='Tuition',x="UG",fit_reg=False)
plt.show()

# Create a scatter plot and bin the data into 5 bins
sns.regplot(data=df,y='Tuition',x="UG",x_bins=5)
plt.show()

# Create a regplot and bin the data into 8 bins
sns.regplot(data=df,y='Tuition',x="UG",x_bins=8)
plt.show()
```

- Matrix Plots

```
# Create a crosstab table of the data
pd_crosstab = pd.crosstab(df["Group"], df["YEAR"])

# Plot a heatmap of the table
sns.heatmap(pd_crosstab)

# Rotate tick marks for visibility
plt.yticks(rotation=0)
plt.xticks(rotation=90)

plt.show()
```

```
# Create the crosstab DataFrame
pd_crosstab = pd.crosstab(df["Group"], df["YEAR"])

# Plot a heatmap of the table with no color bar and using the BuGn palette
sns.heatmap(pd_crosstab, cbar=False, cmap="BuGn", linewidths=0.3)

# Rotate tick marks for visibility
plt.yticks(rotation=0)
plt.xticks(rotation=90)

#Show the plot
plt.show()
```

# Plots on data aware grids

- FacetGrid vs catplot; easier way to create a FacetGrid
```
# Create FacetGrid with Degree_Type and specify the order of the rows using row_order
g2 = sns.FacetGrid(df, row="Degree_Type",row_order=['Graduate', 'Bachelors', 'Associates', 'Certificate'])
# Map a pointplot of SAT_AVG_ALL onto the grid
g2.map(sns.pointplot, 'SAT_AVG_ALL')
# Show the plot
plt.show()

# Create a cat plot that contains boxplots of Tuition values
sns.catplot(data=df,x='Tuition',kind='box',row='Degree_Type')
plt.show()

# Create a facetted pointplot of Average SAT_AVG_ALL scores facetted by Degree Type 
sns.catplot(data=df,x='SAT_AVG_ALL',kind='point',row='Degree_Type',row_order=['Graduate', 'Bachelors', 'Associates', 'Certificate'])
plt.show()
```

- FacetGrid vs lmplot
```
# Create a FacetGrid varying by column and columns ordered with the degree_order variable
g = sns.FacetGrid(df, col="Degree_Type", col_order=degree_ord)
# Map a scatter plot of Undergrad Population compared to PCTPELL
g.map(plt.scatter, 'UG', 'PCTPELL')
plt.show()

# Re-create the previous plot as an lmplot
sns.lmplot(data=df,x='UG',y='PCTPELL',col="Degree_Type",col_order=degree_ord)
plt.show()

# Create an lmplot that has a column for Ownership, a row for Degree_Type and hue based on the WOMENONLY column
sns.lmplot(data=df,x='SAT_AVG_ALL',y='Tuition',col="Ownership",row='Degree_Type',row_order=['Graduate', 'Bachelors'],hue='WOMENONLY',col_order=inst_ord)
plt.show()
```

 - PairGrid vs pairplot
```
# Create a PairGrid with a scatter plot for fatal_collisions and premiums
g = sns.PairGrid(df, vars=["fatal_collisions", "premiums"])
g2 = g.map(plt.scatter)
plt.show()

# Create the same PairGrid but map a histogram on the diag
g = sns.PairGrid(df, vars=["fatal_collisions", "premiums"])
g2 = g.map_diag(plt.hist)
g3 = g2.map_offdiag(plt.scatter)
plt.show()

# Create a pairwise plot of the variables using a scatter plot
sns.pairplot(df,vars=["fatal_collisions", "premiums"],kind='scatter')
plt.show()

# Plot the same data but use a different color palette and color code by Region
sns.pairplot(df,vars=["fatal_collisions", "premiums"],kind='scatter',hue='Region',palette='RdBu',diag_kws={'alpha':.5})
plt.show()

# Build a pairplot with different x and y variables
sns.pairplot(df,x_vars=["fatal_collisions_speeding", "fatal_collisions_alc"],y_vars=['premiums', 'insurance_losses'],kind='scatter',hue='Region',palette='husl')
plt.show()

# plot relationships between insurance_losses and premiums
sns.pairplot(df,vars=["insurance_losses", "premiums"],kind='reg',palette='BrBG',
diag_kind = 'kde',hue='Region')
plt.show()
```

- JointGrid vs jointplot
```
# Build a JointGrid comparing humidity and total_rentals
sns.set_style("whitegrid")
g = sns.JointGrid(x="hum",y="total_rentals",data=df,xlim=(0.1, 1.0)) 
g.plot(sns.regplot, sns.distplot)
plt.show()

# Create a jointplot similar to the JointGrid 
sns.jointplot(x="hum",y="total_rentals",kind='reg',data=df)
plt.show()

# Plot temp vs. total_rentals as a regression plot
sns.jointplot(x="temp",y="total_rentals",kind='reg',data=df,order=2,xlim=(0, 1))
plt.show()

# Plot a jointplot showing the residuals
sns.jointplot(x="temp",y="total_rentals",kind='resid',data=df,order=2)
plt.show()

# Create a jointplot of temp vs. casual riders
# Include a kdeplot over the scatter plot
g = (sns.jointplot(x="temp",y="casual",kind='scatter',data=df,marginal_kws=dict(bins=10, rug=True)).plot_joint(sns.kdeplot))
plt.show()

# Replicate the previous plot but only for registered riders
g = (sns.jointplot(x="temp",y="registered",kind='scatter',data=df,marginal_kws=dict(bins=10, rug=True)).plot_joint(sns.kdeplot))
plt.show()
```


