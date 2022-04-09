# [Seaborn](https://seaborn.pydata.org/index.html)
Seaborn is a powerful but easy-to-use data visualization tool. It short and simple code, making it much faster and easier to use than many other data visualization tools (such as Excel).

## [Line Charts](https://www.kaggle.com/code/alexisbcook/line-charts/tutorial)
First, we import the relevant libraries. Then we load and access the data. Finally, we make a simple plot using the Seaborn library.
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Path of the file to read
spotify_filepath = "../input/spotify.csv"

# Read the file into a variable spotify_data
spotify_data = pd.read_csv(spotify_filepath, index_col="Date", parse_dates=True)

# PLOT
# Line chart showing daily global streams of each song 
sns.lineplot(data=spotify_data)
```

For better visualization, we can specify some details such as figure size and title.

```python
# ADDITIONAL DETAILS
# Set the width and height of the figure
plt.figure(figsize=(14,6))

# Add title
plt.title("Daily Global Streams of Popular Songs in 2017-2018")

# Line chart showing daily global streams of each song 
sns.lineplot(data=spotify_data)
```

Sometimes, we only want to plot only a subset of the data we have.
```python
# PLOT ONLY A SUBSET
# Set the width and height of the figure
plt.figure(figsize=(14,6))

# Add title
plt.title("Daily Global Streams of Popular Songs in 2017-2018")

# Line chart showing daily global streams of 'Shape of You'
sns.lineplot(data=spotify_data['Shape of You'], label="Shape of You")

# Line chart showing daily global streams of 'Despacito'
sns.lineplot(data=spotify_data['Despacito'], label="Despacito")

# Add label for horizontal axis
plt.xlabel("Date")
```

## [Bar charts and Heatmaps](https://www.kaggle.com/code/alexisbcook/bar-charts-and-heatmaps/tutorial)
We import relevant libraries then access the data.
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Path of the file to read
flight_filepath = "../input/flight_delays.csv"

# Read the file into a variable flight_data
flight_data = pd.read_csv(flight_filepath, index_col="Month")
```

### Bar Charts
To create bar charts, we simply use the `sns.barplot()` function.
```python
# Set the width and height of the figure
plt.figure(figsize=(10,6))

# Add title
plt.title("Average Arrival Delay for Spirit Airlines Flights, by Month")

# Bar chart showing average arrival delay for Spirit Airlines flights by month
sns.barplot(x=flight_data.index, y=flight_data['NK'])

# Add label for vertical axis
plt.ylabel("Arrival delay (in minutes)")
```
Important Note: You must select the indexing column with flight_data.index, and it is not possible to use flight_data['Month'] (which will return an error). This is because when we loaded the dataset, the "Month" column was used to index the rows. We always have to use this special notation to select the indexing column.

### Heatmaps
To create heatmpas, we simply use the `sns.heatmap()` function.
```python 
# Set the width and height of the figure
plt.figure(figsize=(14,7))

# Add title
plt.title("Average Arrival Delay for Each Airline, by Month")

# Heatmap showing average arrival delay for each airline by month
sns.heatmap(data=flight_data, annot=True)

# Add label for horizontal axis
plt.xlabel("Airline")
```

## [Scatter Plots](https://www.kaggle.com/code/alexisbcook/scatter-plots)
To create scatter plots, we import relevant libraries and load the data. We inspect the data using the `.head()` command then create a simple scatter plot using `sns.scatterplot()`.
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Path of the file to read
insurance_filepath = "../input/insurance.csv"

# Read the file into a variable insurance_data
insurance_data = pd.read_csv(insurance_filepath)

# Inspect first few rows
insurance_data.head()

# Scatter plot
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'])
```

Sometimes, we would want to add a **regression line** or best-fit line. We do this by changing using `sns.regplot()`.
```python
# Scatter plot with regression line
sns.regplot(x=insurance_data['bmi'], y=insurance_data['charges'])
```

### Color-coded Scatter Plots
Taking this a step further, we can use color-coded scatter plots to show relationsips between three variables! We do this by adding a `hue=''` parameter. In addition, we can use `sns.lmplot()` to add regression lines corresponding to the third variable.
```python
# Color-coded scatter plots
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'], hue=insurance_data['smoker'])

# Color-coded scatter plots with regression line 
sns.lmplot(x="bmi", y="charges", hue="smoker", data=insurance_data)
```

### Categorical Scatter Plots
Categorical scatter plots feature a categorical variable on one of the main axes. For this, we use the `sns.swarmplot()`.
```python
sns.swarmplot(x=insurance_data['smoker'],
              y=insurance_data['charges'])
```
