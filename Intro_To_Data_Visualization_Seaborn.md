# Introduction to Seaborn

## Introduction to Seaborn

### What is Seaborn

- Python data visualization library
- Easily create the most common types of plots
- Useful for the "explore" and "communicate results" phases of typical data flow

### Advantages of Seaborn

- Easy to use
- Works well with **pandas** data structures
- Built on top of **matplotlib**

### Getting started

```
import seaborn as sns
import matplotlib.pyplot as plt
```

- **S**amuel **N**orman **S**eaborn (sns)
  - "The West Wing" television show

### Example 1: Scatter plot

```
import seaborn as sns
import matplotlib.pyplot as plt
height = [62, 64, 78, ...]
weight = [120, 136, 148, ...]
sns.scatterplot(x=height, y=weight)
plt.show()
```

### Example 2: Create a count plot

```
import seaborn as sns
import matplotlib.pyplot as plt
gender = ["Female", "Female", "Male", ...]
sns.countplot(x=gender)
plt.show()
```

- This shows a bar plot with the counts of list entries for each category

## Using pandas with Seaborn

### Using DataFrames with countplot()

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("masculinity.csv")
sns.countplot(x="how_masculine", data=df)
plt.show()
```

- The kwarg = x is the name of the column to display or count
- The kwarg = data points to the dataframe

### Tidy data

- DataFrames used with seaborn must be tidy
- This means the data is complete, valid and consistent
- Reach row is a complete observation
- Each column has data and of same data types

## Adding a third variable with hue

### Tips dataset

- Comes built-in with seaborn

```
import pandas as pd
import seaborn as sns
tips = sns.load_dataset("tips")
print(tips.head())
```

### A scatter plot with hue

```
import matplotlib.pyplot as plt
import seaborn as sns

sns.scatterplot(x="total_bill",
                y="tip",
                data=tips,
                hue="smoker")

plt.show()
```

- The kwarg = hue is the column name to add to the chart using color
- This encodes the third variable using color
- Seaborn automatically adds a legend for the third variable

### Setting hue order

```
import matplotlib.pyplot as plt
import seaborn as sns

sns.scatterplot(x="total_bill",
                y="tip",
                data=tips,
                hue="smoker",
                hue_order=["Yes", "No"])

plt.show()
```

- The kwarg = hue_order adjusts the color order and legend order

### Specifying hue colors

```
import matplotlib.pyplot as plt
import seaborn as sns

hue_colors - {"Yes": "black", "No": "red"}

sns.scatterplot(x="total_bill",
                y="tip",
                data=tips,
                hue="smoker",
                palette=hue_colors)

plt.show()
```

- Here color names are based on the color names available in matplotlib
- You can also use matplotlib colors specified by single character, i.e. "blue" = "b"
- Can also use html hex codes

### Using HTML hex color codes with hue

```
import matplotlib.pyplot as plt
import seaborn as sns

hue_colors - {"Yes": "#808080", "No": "#00FF00"}

sns.scatterplot(x="total_bill",
                y="tip",
                data=tips,
                hue="smoker",
                palette=hue_colors)

plt.show()
```

### Using hue with countplots

```
import matplotlib.pyplot as plt
import seaborn as sns

hue_colors - {"Yes": "#808080", "No": "#00FF00"}

sns.countplot(x="smoker",
              data=tips,
              hue="sex")

plt.show()
```

# Visualizing Two Quantitative Variables

## Introduction to relational plots and subplots

### Questions about quantitative variables

- We often want to understand the relationship between two quantitative variables
- Seaborn calls this kind of chart a "relational" chart
  - Height vs Weight
  - Number of school absences vs final grade
  - GDP vs percent literate

### Introducing relplot()

- Create "relational plots": scatter or line plots

Why use **relplot()## instead of **scatterplot()\*\* ?

- **relplot()** lets you create subplots in a single figure

### scatterplot() vs relplot()

Using **scatterplot()**

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.scatterplot(x="total_bill", y="tip",
                data=tips)
plt.show()
```

Using **relplot()**

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter")
plt.show()
```

### Subplots in columns

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                col="smoker")
plt.show()
```

- Here the subplots will be based on the "smoker" column values, organized as columns

### Subplots in rows

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                row="smoker")
plt.show()
```

### Subplots in rows and columns

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                col="smoker", row="time")
plt.show()
```

### Wrapping columns

- Can use this if there are too many subplots on a row

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                col="smoker", col_wrap=2)
plt.show()
```

- Here there are two subplots per row

### Ordering columns

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                col="smoker", col_wrap=2,
                col_order=["Yes", "No"])
plt.show()
```

- You can also use **row_order** to organize the row value order

## Customizing scatter plots

- Customizations looked at here
  - Subgroups with point size and stle
  - Changing point transparency

### Subgroups with point size

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                size="size")
plt.show()
```

- Here **size** points to the size variable, representing the size of the group or party

### Point size and hue

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                size="size", hue="size")
plt.show()
```

### Subgroups with point style

```
import seaborn as sns
import matplotlib.pyplot as plt

sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                hue="smoker", style="smoker")
plt.show()
```

- Point style will vary by the value of the smoker variable

### Changing point transparency

```
import seaborn as sns
import matplotlib.pyplot as plt

# set alpha between 0 and 1
sns.relplot(x="total_bill", y="tip",
                data=tips, kind="scatter",
                alpha=0.4)
plt.show()
```

- Here, darker areas of the chart will indicate more observations

# Visualizing a Categorical and a Quantitative Varaible

# Customizing Seaborn Plots

```

```
