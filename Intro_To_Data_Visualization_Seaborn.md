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

# Visualizing Two Quantitative Variables

# Visualizing a Categorical and a Quantitative Varaible

# Customizing Seaborn Plots
