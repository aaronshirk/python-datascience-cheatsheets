# Matplotlib

## Basic plots with Matplotlib

### Matplotlib line plot

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.show()
```

### Matplotlib scatter plot

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.scatter(year, pop)

plt.show()
```

## Histogram

- Explore dataset
- Get idea about distribution

### Matplotlib example of histogram

```
value = [0, 0.6, 1.4, 1.6, 2.2, 2.5, 2.6, 3.2, 3.5, 3.9, 4.2, 6]

plt.hist(value, bins=3)

plt.show()
```

## Customization

- Matplotlib plots has many options
  - Different plot types
  - Many customizations
- Choice depends on
  - Data
  - Story you want to tell

### Axis labels

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')

plt.show()
```

### Title

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')

plt.show()
```

### Ticks

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')
plt.yticks([0, 2, 4, 6, 8, 10])

plt.show()
```

- _plt.yticks()_ sets the range of the y-axis and can shift the graph

### Ticks(2)

```
import Matplotlib.pyplot as plt

year = [1950, 1970, 1990, 2010]

pop = [2.519, 3.692, 5.263, 6.972]

plt.plot(year, pop)

plt.xlabel('Year')
plt.ylabel('Populations')
plt.title('World Population Projections')
plt.yticks([0, 2, 4, 6, 8, 10],
            [0, 2B, 4B, 6B, 8B, 10B])

plt.show()
```

- The second _yticks()_ argument is the ticks labels for more clarify

# Dictionaries and Pandas

## Dictionaries, Part 1

- Object of key values pairs

```
world = {"afghanistan": 30.55, "albania": 2.77, "algeria": 39.21}

world["albania"]
```

- Easy to index into the dictionary using the key value

## Dictionaries, Part 2

### Recap

- Dictionary keys have to be immutable values
- For example, a "string" is immutable, but a list is not, so a list cannot be a key

### More dictionary examples

`world['sealand'] = 0.00607`

- Set the value of a key; this is an update
- Keys are unique in a dictionary

`'sealand' in world`

- This is a way to check if the key 'sealand' exists in the world dictionary

`del(world['sealand'])`

- _del()_ function will remove a key/value entry from a dictionary

## Pandas, Part 1

## Pandas, Part 2

# Logic, Control Flow and Filtering

# Loops
