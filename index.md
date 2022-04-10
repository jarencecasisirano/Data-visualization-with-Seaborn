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

## [Histograms and Density Plots](https://www.kaggle.com/code/alexisbcook/distributions)
We import relevant libraries, then we load and examine the data.
``` python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

# Path of the file to read
iris_filepath = "../input/iris.csv"

# Read the file into a variable iris_data
iris_data = pd.read_csv(iris_filepath, index_col="Id")

# Print the first 5 rows of the data
iris_data.head()
```

### Histograms
To create histograms, we use the `sns.distplot` command. The parameter `a=` dictates the column to plot while `kde=False` is something we'll always provide when creating a histogram, as leaving it out will create a slightly different plot.
```python
# Histogram 
sns.distplot(a=iris_data['Petal Length (cm)'], kde=False)
```

### Density Plots
**Kernel density estimate (KDE)** plot is a smoothed histogram. To create a KDE plot, we use the `sns.kdeplot` command. Setting `shade=True` colors the area below the curve (and `data=` has identical functionality as when we made the histogram above).
```python
# KDE plot 
sns.kdeplot(data=iris_data['Petal Length (cm)'], shade=True)
```

We can create a **two-dimensional (2D) KDE** plot with the `sns.jointplot` command.
```python
# 2D KDE plot
sns.jointplot(x=iris_data['Petal Length (cm)'], y=iris_data['Sepal Width (cm)'], kind="kde")
```

### Color-Coded Histograms and Density Plots
To create color-coded plots, we break the dataset into separate files.

#### Color-Coded Histograms
We create a different histogram for each by using the `sns.distplot` command. We use `label=` parameter to set how each histogram will appear in the legend. To force the legend to show (for any plot type), we can always use `plt.legend()`.
```python
# Paths of the files to read
iris_set_filepath = "../input/iris_setosa.csv"
iris_ver_filepath = "../input/iris_versicolor.csv"
iris_vir_filepath = "../input/iris_virginica.csv"

# Read the files into variables 
iris_set_data = pd.read_csv(iris_set_filepath, index_col="Id")
iris_ver_data = pd.read_csv(iris_ver_filepath, index_col="Id")
iris_vir_data = pd.read_csv(iris_vir_filepath, index_col="Id")

# Print the first 5 rows of the Iris versicolor data
iris_ver_data.head()

# Histograms for each species
sns.distplot(a=iris_set_data['Petal Length (cm)'], label="Iris-setosa", kde=False)
sns.distplot(a=iris_ver_data['Petal Length (cm)'], label="Iris-versicolor", kde=False)
sns.distplot(a=iris_vir_data['Petal Length (cm)'], label="Iris-virginica", kde=False)

# Add title
plt.title("Histogram of Petal Lengths, by Species")

# Force legend to appear
plt.legend()
```

#### Color-Coded Density Plots
We can also create a KDE plot for each by using `sns.kdeplot`. Again, `label=` is used to set the values in the legend.
```python
# KDE plots for each species
sns.kdeplot(data=iris_set_data['Petal Length (cm)'], label="Iris-setosa", shade=True)
sns.kdeplot(data=iris_ver_data['Petal Length (cm)'], label="Iris-versicolor", shade=True)
sns.kdeplot(data=iris_vir_data['Petal Length (cm)'], label="Iris-virginica", shade=True)

# Add title
plt.title("Distribution of Petal Lengths, by Species")
```

## [Choosing Plot Types and Custom Styles](https://www.kaggle.com/code/alexisbcook/choosing-plot-types-and-custom-styles/tutorial)
It's tricky to decide how to best tell the story behind your data and charts can help with that. Charts can be categorized into three:
- **Trends** - A trend is defined as a pattern of change.
	- `sns.lineplot` - **Line charts** are best to show trends over a period of time, and multiple lines can be used to show trends in more than one group.
- **Relationship** - There are many different chart types that you can use to understand relationships between variables in your data.
  - `sns.barplot` - **Bar charts** are useful for comparing quantities corresponding to different groups.
	- `sns.heatmap` - **Heatmaps** can be used to find color-coded patterns in tables of numbers.
	- `sns.scatterplot` - **Scatter plots** show the relationship between two continuous variables; if color-coded, we can also show the relationship with a third categorical variable.
	- `sns.regplot` - Including a **regression line** in the scatter plot makes it easier to see any linear relationship between two variables.
	- `sns.lmplot` - This command is useful for drawing multiple regression lines, if the scatter plot contains multiple, color-coded groups.
	- `sns.swarmplot` - **Categorical scatter plots** show the relationship between a continuous variable and a categorical variable.
- **Distribution** - We visualize distributions to show the possible values that we can expect to see in a variable, along with how likely they are.
	- `sns.distplot` - **Histograms** show the distribution of a single numerical variable.
	- `sns.kdeplot` - **KDE plots (or 2D KDE plots)** show an estimated, smooth distribution of a single numerical variable (or two numerical variables).
	- `sns.jointplot` - This command is useful for simultaneously displaying a 2D KDE plot with the corresponding KDE plots for each individual variable.

The categories above are summarized in the figure below:

![image1](https://github.com/jarencecasisirano/Data-visualization-with-Seaborn/blob/main/assets/2VmgDnF.png)
