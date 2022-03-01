# Introduction to Matplotlib

## Introduction to data visualization with Matplotlib

### Introducing the pyplot interface

```
import matplotlib.pyplot as plt
fix, ax = plt.subplots()
plot.show()
```

- This will show an empty figure

### Adding data to axes

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL])
plt.show()
```

- This shows a simple line chart

### Adding more data

```
ax.plot(austin_weather["MONTH"], austin_weather["MLY-TAVG-NORMAL])
plt.show()
```

- This will add a line to the bar graph

### Putting it all together

```
fig, ax = plt.subplots()
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL])
ax.plot(austin_weather["MONTH"], austin_weather["MLY-TAVG-NORMAL])
plt.show()
```

- This chart will now have to lines, one for each city

## Customizing your plots

### Adding markers

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL],
        marker='o')
```

- Puts a 'dot' at each point

### Choosing markers

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL],
        marker='v')
```

- The marker is now a 'downward' arrow

### Setting the linestyle

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL],
        marker='v', linestyle='--')
```

- This makes the line a 'dashed' line

### Eliminating lines with linestyle

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL],
        marker='v', linestyle='None')
```

- This removes the line and keeps the points on the chart

### Choosing color

```
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL],
        marker='v', linestyle='None', color='r')
```

### Setting (x,y) labels and title

```
ax.set_xlabel("Time (months)")
ax.set_ylabel("Average temperature (Fahrenheight degrees)")
ax.set_title("Weather in Seattle")
```

## Small Multiples

- Adding too many lines or charts to a single subplot can get busy
- Small multiples is a way to create multiple axes in a single frame

### Small multiples with plt.subplots

```
fig, ax = plt.subplots()
```

- This creates one subplot

```
fig, ax = plt.subplots(3, 2)
plt.show()
```

- This creates 6 subplots
- Grid is 3 rows x 2 columns of subplots

### Adding data to subplots

```
ax[0, 0].plot(seattle_weather['MONTH'], seattle_weather['MLY-PRCP-NORMAL'], color='b')
plt.show()
```

### Subplots with data

```
fig, ax = plt.subplots(2, 1)
ax[0].plot(seattle_weather['MONTH'], seattle_weather['MLY-PRCP-NORMAL'], color='b')

ax[0].plot(seattle_weather['MONTH'], seattle_weather['MLY-PRCP-25PCTL'],
            linestyle='--', color='b')

ax[0].plot(seattle_weather['MONTH'], seattle_weather['MLY-PRCP-75PCTL'],
            linestyle='--', color='b')

ax[0].plot(austin_weather['MONTH'], austin_weather['MLY-PRCP-NORMAL'], color='r')

ax[0].plot(austin_weather['MONTH'], austin_weather['MLY-PRCP-25PCTL'],
            linestyle='--', color='r')

ax[0].plot(austin_weather['MONTH'], austin_weather['MLY-PRCP-75PCTL'],
            linestyle='--', color='r')

ax[0].set_ylabel('Precipitation (inches)')
ax[0].set_ylabel('Precipitation (inches)')
ax[1].set_xlabel('Time (months)')
```

### Sharing the y-axis range

```
fig, ax = plt.subplots(2, 1, sharey=True)
```

## Plotting time-series data

### Plotting time-series data

```
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.plot(climate_change.index, climate_change['co2'])
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm)')
plot.show()
```

- In this example matplotlib automatically selected to show year on the x-axis in intervals of 10 years

### Zooming in on a decade

```
sixties = climate_change['1960-01-01': '1969-12-31']
fig, ax = plt.subplots()
ax.plot(sixties.index, sixties['co2'])
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm)')
plt.show()
```

## Plotting time-series data with different variables

### Plotting two time-series together

```
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.plot(climate_change.index, climate_change['co2'])
ax.plot(climate_change.index, climate_change['relative_temp'])
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm) / Relative temperature')
plt.show()
```

- Plot doesn't look right
- Problem: scales for the two measurements are different

### Using twin axes

```
fig, ax = plt.subplots()
ax.plot(climate_change.index, climate_change['co2'])
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm)')

ax2 = ax.twinx()  # <-- This creates a 'twin axis' that shares the x-axis
ax2.plot(climate_change.index, climate_change['relative_temp'])
ax2.set_ylabel('Relative temperature (Celsius))
plt.show()
```

### Separating variables by color

```
fig, ax = plt.subplots()
ax.plot(climate_change.index, climate_change['co2'], color='blue')
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm)', color='blue')

ax2 = ax.twinx()  # <-- This creates a 'twin axis' that shares the x-axis
ax2.plot(climate_change.index, climate_change['relative_temp'], color='red')
ax2.set_ylabel('Relative temperature (Celsius)', color='red')
plt.show()
```

### Coloring the ticks

```
fig, ax = plt.subplots()
ax.plot(climate_change.index, climate_change['co2'], color='blue')
ax.set_xlabel('Time')
ax.set_ylabel('CO2 (ppm)', color='blue')
ax.tick_params('y', colors='blue')

ax2 = ax.twinx()  # <-- This creates a 'twin axis' that shares the x-axis
ax2.plot(climate_change.index, climate_change['relative_temp'], color='red')
ax2.set_ylabel('Relative temperature (Celsius)', color='red')
ax.tick_params('y', colors='red')
plt.show()
```

### A function that plots time-series

```
def plot_timeseries(axes, x, y, color, xlabel, ylabel):
    axes.plot(x, y, color=color)
    axes.set_xlabel(xlabel)
    axes.set_xlabel(ylabel, color=color)
    axes.tick_params('y', colors=color)

fig, ax = plt.subplots()
plot_timeseries(ax, climate_change.index, climate_change['co2'], 'blue', 'Time', 'CO2 (ppm)')

ax2 = ax.twinx()
plot_timeseries(ax, climate_change.index, climate_change['relative_temp'], 'red', 'Time', 'Relative temperature (Celsius)')

plt.show()
```

## Annotating time-series data

### Annotation

```
fig, ax = plt.subplots()
plot_timeseries(ax, climate_change.index, climate_change['co2'], 'blue', 'Time', 'CO2 (ppm)')

ax2 = ax.twinx()
plot_timeseries(ax, climate_change.index, climate_change['relative_temp'], 'red', 'Time', 'Relative temperature (Celsius)')

ax2.annotate(">1 degree", xy=[pd.TimeStamp("2015-10-06"), 1])  # <-- annotation
plt.show()
```

- Sometimes the annotation doesn't look good or overlaps other text

### Positioning the text

```
fig, ax = plt.subplots()
plot_timeseries(ax, climate_change.index, climate_change['co2'], 'blue', 'Time', 'CO2 (ppm)')

ax2 = ax.twinx()
plot_timeseries(ax, climate_change.index, climate_change['relative_temp'], 'red', 'Time', 'Relative temperature (Celsius)')

ax2.annotate(">1 degree", xy=(pd.TimeStamp("2015-10-06"), 1),
                          xytext=(pd.TimeStamp("2008-10-06"), -0.2))  # <-- annotation
plt.show()
```

- Here the **xytext** kwarg separates the text from the point of the annotation

### Adding arrows to annotations

```
ax2.annotate(">1 degree", xy=(pd.TimeStamp("2015-10-06"), 1),
                          xytext=(pd.TimeStamp("2008-10-06"), -0.2),
                          arrowprops={})
```

- **arrowprops** arguement takes a dictionary
- An empty dictionary, then the arrow gets the default props

### Customizing arrow properties

```
ax2.annotate(">1 degree", xy=(pd.TimeStamp("2015-10-06"), 1),
                          xytext=(pd.TimeStamp("2008-10-06"), -0.2),
                          arrowprops={"arrowstyle": "->", "color": "gray"})
```

- [Customizing annotations](https://matplotlib.org/users/annotations.html)

# Quantitative comparisons and statistical visualizations

## Quantitative comparisons: bar-charts

### Olympic medals: visualizing the data

- Here the olympic medals dataframe has country name as the index

```
medals = pd.csv_read('medals_by_country_2016.csv', index_col=0)
fig, ax = pd.subplots()
ax.bar(medals.index, medals["Gold"])
plt.show()
```

- Bar chart looks ok, but the country names overlap

### Interlude: rotate the tick labels

```
fig, ax = pd.subplots()
ax.bar(medals.index, medals["Gold"])
ax.set_xticklabels(medals.index, rotation=90)
ax.set_ylabel("Number of medals")
plt.show()
```

### Olympic medals: visualizing the other medals

- Can use a stacked bar chart

```
fig, ax = pd.subplots()
ax.bar(medals.index, medals["Gold"])
ax.bar(medals.index, medals["Silver"], bottom=medals["Gold"])
ax.set_xticklabels(medals.index, rotation=90)
ax.set_ylabel("Number of medals")
plt.show()
```

### Olympic medals: visualizing all three

- Can use a stacked bar chart

```
fig, ax = pd.subplots()
ax.bar(medals.index, medals["Gold"])
ax.bar(medals.index, medals["Silver"], bottom=medals["Gold"])
ax.bar(medals.index, medals["Bronze"], bottom=medals["Silver"] + medals["Gold"])
ax.set_xticklabels(medals.index, rotation=90)
ax.set_ylabel("Number of medals")
plt.show()
```

### Adding a legend

- Can use a stacked bar chart

```
fig, ax = pd.subplots()
ax.bar(medals.index, medals["Gold"], label="Gold")
ax.bar(medals.index, medals["Silver"], bottom=medals["Gold"], label="Silver")
ax.bar(medals.index, medals["Bronze"], bottom=medals["Silver"] + medals["Gold"], label="Bronze")
ax.set_xticklabels(medals.index, rotation=90)
ax.set_ylabel("Number of medals")
ax.legend()
plt.show()
```

## Quantitative comparisons: histograms

## Quantitative comparisons: statistical plotting

## Quantitative comparisons: scatter plots

# Sharing visualizations with others
