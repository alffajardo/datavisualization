





# Data visualization in R (datacamp)

## Lerning to plot with R graphics

In this exercise, you'll construct a simple exploratory plot from a data frame that gives values for three variables, recorded over two winter heating seasons. The variables are:

Temp: a measure of the outside temperature during one week
Gas: the amount of heating gas consumed during that week
Insul: a categorical variable with two values, indicating whether the measurements were made before or after an insulation upgrade was made to the house

```R
# Load MASS package
library(MASS)

# Plot whiteside data
plot(whiteside)
```

### Creating an explanatory scatterplot

In constrast to the *exploratory* analysis plot you created in the previous exercise, this exercise asks you to create a simple *explanatory* scatterplot, suitable for sharing with others.

Here, it is important to make editorial choices in constructing this plot, rather than depending entirely on default options. In particular, the important editorial aspects of this plot are: first, the variables to be plotted, and second, the axis labels, which are specified as strings to the `xlab` and `ylab` arguments to the `plot()`function.

```R
# Plot Gas vs. Temp
plot(whiteside$Temp,whiteside$Gas,xlab="Outside Temperture",ylab="Heating gas consumption")
```

![](https://github.com/alffajardo/datavisualzation/blob/master/scatterplot1.png)

![](/home/alfonso/Documentos/github/datavisualzation/scatterplot1.png)



### Adding details to a plot using point shapes, color, and reference lines

Adding additional details to your explanatory plots can help emphasize certain aspects of your data. For example, by specifying the pch and col arguments to the plot() function, you can add different point shapes and colors to show how different variables or subsets of your data relate to each other. In addition, you can add a new set of points to your existing scatterplot with the points() function, and add reference lines with the abline() function.

This exercise asks you to use these methods to create an enhanced scatterplot that effectively shows how three variables in the Cars93 data frame relate to each other. These variables are:

Price: the average sale price for a car
Max.Price: the highest recorded price for that car
Min.Price: the lowest recorded price for that car

```R
# Plot Max.Price vs. Price as red triangles
plot(Cars93$Price,Cars93$Max.Price,pch=17,col="red")

# Add Min.Price vs. Price as blue circles
points(Cars93$Price,Cars93$Min.Price,pch=16,col="blue")
# Add an equality reference line with abline()
abline(a=0,b=1,lty=2,lwd=2)
```

![](https://github.com/alffajardo/datavisualzation/blob/master/scatterplot2.png)



![](/home/alfonso/Documentos/github/datavisualzation/scatterplot2.png)

### Creating multiple plot arrays

You can plot multiple graphs on a single pane using the `par()` function with its `mfrow` parameter. For example, `par(mfrow = c(1, 2))` creates a plot array with 1 row and 2 columns, allowing you to view two graphs side-by-side. This way, you can compare and contrast different datasets or different views of the same dataset. This exercise asks you to compare two views of the [`Animals2`](https://www.rdocumentation.org/packages/robustbase/topics/Animals2) dataset from the `robustbase` package, differing in how its variables are represented.

The objective of this exercise is to emphasize that the original representation of the variables that we have in a dataset is not always the best one for visualization or analysis. By representing the original variables in log scale, for example, we can better see and understand the data.

```R
# Load the robustbase package
library(robustbase)

# Set up the side-by-side plot array
par(mfrow= c(1,2))

# First plot: brain vs. body in its original form
plot(Animals2$body,Animals2$brain)
# Add the first title
title("Original representation")


# Second plot: log-log plot of brain vs. body
plot(Animals2$body,Animals2$brain,log="xy")


# Add the second title
title("Log-log plot")
```

![](/home/alfonso/Documentos/github/datavisualzation/par.png)
![](https://github.com/alffajardo/datavisualzation/blob/master/par.png)

### Avoid Pie charts

The same dataset can be displayed or summarized in many different ways, but some are much more suitable than others.

Despite their general popularity, pie charts are often a poor choice. Though R allows pie charts with the `pie()` function, even the help file for this function argues against their use. Specifically, the help file includes a "Note" that begins with the words: "Pie charts are a very bad way of displaying information."

Bar charts are a recommended alternative and, in this exercise, you'll see why.

```R
library(insuranceData)
# dataCar is in your workspace
str(dataCar)

# Set up a side-by-side plot array
par(mfrow = c(1, 2))

# Create a table of veh_body record counts and sort
tbl <- sort(table(dataCar$veh_body),
            decreasing = TRUE)

# Create the pie chart
pie(tbl)

# Give it a title
title("Pie chart")

# Create the barplot with perpendicular, half-sized labels
barplot(tbl, las = 2, cex.names = 0.5)

# Add a title
title("Bar chart")
```





![](/home/alfonso/Documentos/github/datavisualzation/pie_vs_bar.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/pie_vs_bar.png)

### The hist() and truehist() functions

Histograms are probably the best-known way of looking at how the values of a numerical variable are distributed over their range, and R provides several different histogram implementations.

The purpose of this exercise is to introduce two of these:

- `hist()` is part of base R and its default option yields a histogram based on the number of times a record falls into each of the bins on which the histogram is based.

- `truehist()` is from the `MASS` package and scales these counts to give an estimate of the probability density.

  ```R
  library(MASS)
  # Set up a side-by-side plot array
  par(mfrow=c(1,2))
  
  # Create a histogram of counts with hist()
  hist(Cars93$Horsepower,main='hist() plot')
  
  # Create a normalized histogram with truehist()
  truehist(Cars93$Horsepower,main='truehist() plot')
  
  ```

  ![](/home/alfonso/Documentos/github/datavisualzation/hist_truhist.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/hist_truhist.png)

## Density plots as smoothed histograms

While they are probably not as well known as the histogram, density estimates may be regarded as *smoothed histograms*, designed to give a better estimate of the density function for a random variable.

In this exercise, you'll use the [`ChickWeight`](https://www.rdocumentation.org/packages/datasets/topics/ChickWeight) dataset, which contains a collection of chicks' weights. You will first select for the chicks that are 16 weeks old. Then, you'll create a histogram using the `truehist()` function, and add its density plot on top, using the `lines()` and `density()` functions with their default options. The density plot of this type of variable is often expected to conform approximately to the bell-shaped curve, otherwise known as the Gaussian distribution. Let's find out whether that's the case for this dataset.

```R
# Create index16, pointing to 16-week chicks
index16 <- which(ChickWeight$Time==16)

# Get the 16-week chick weights
weights <- ChickWeight$weight[index16]
# Plot the normalized histogram
truehist(weights)
# Add the density curve to the histogram
lines(density(weights))R
```

![](/home/alfonso/Documentos/github/datavisualzation/density_histogram.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/density_histogram.png)



## Using the qqPlot() function to see many details in data

A practical limitation of both histograms and density estimates is that, if we want to know whether the Gaussian distribution assumption is reasonable for our data, it is difficult to tell.

The quantile-quantile plot, or QQ-plot, is a useful alternative: we sort our data, plot it against a specially-designed x-axis based on our reference distribution (e.g., the Gaussian "bell curve"), and look to see whether the points lie approximately on a straight line. In R, several QQ-plot implementations are available, but the most convenient one is the `qqPlot()` function in the `car` package.

The first part of this exercise applies this function to the 16-week chick weight data considered in the last exercise, to show that the Gaussian distribution appears to be reasonable here. The second part of the exercise applies this function to another variable where the Gaussian distribution is obviously a poor fit, but the results also show the presence of repeated values (flat stretches in the plot) and portions of the data range where there are no observations (vertical "jumps" in the plot).

```R
# Load the car package to make qqPlot() available
library(car)

# Create index16, pointing to 16-week chicks
index16 <- which(ChickWeight$Time == 16)

# Get the 16-week chick weights
weights <- ChickWeight$weight[index16]

# Show the normal QQ-plot of the chick weights
qqPlot(weights)

# Show the normal QQ-plot of the Boston$tax data
qqPlot(Boston$tax)
```



## The sunflowerplot() function for repeated numerical data

A scatterplot represents each (x, y) pair in a dataset by a single point. If some of these pairs are *repeated* (i.e. if the same combination of x and y values appears more than once and thus lie on top of each other), we can't see this in a scatterplot. Several approaches have been developed to deal with this problem, including *jittering*, which adds small random values to each x and y value, so repeated points will appear as clusters of nearby points.

A useful alternative that is equally effective in representing repeated data points is the *sunflowerplot*, which represents each repeated point by a "sunflower," with one "petal" for each repetition of a data point.

This exercise asks you to construct both a scatterplot and a sunflowerplot from the same dataset, one that contains repeated data points. Comparing these plots allows you to see how much information can be lost in a standard scatterplot when some data points appear many times.

