





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
library(MASS)

# Create index16, pointing to 16-week chicks
index16 <- which(ChickWeight$Time == 16)

# Get the 16-week chick weights
weights <- ChickWeight$weight[index16]

# Show the normal QQ-plot of the chick weights
qqPlot(weights)

# Show the normal QQ-plot of the Boston$tax data
qqPlot(Boston$tax)
```

![](https://github.com/alffajardo/datavisualzation/blob/master/densities.png)

## The sunflowerplot() function for repeated numerical data

A scatterplot represents each (x, y) pair in a dataset by a single point. If some of these pairs are *repeated* (i.e. if the same combination of x and y values appears more than once and thus lie on top of each other), we can't see this in a scatterplot. Several approaches have been developed to deal with this problem, including *jittering*, which adds small random values to each x and y value, so repeated points will appear as clusters of nearby points.

A useful alternative that is equally effective in representing repeated data points is the *sunflowerplot*, which represents each repeated point by a "sunflower," with one "petal" for each repetition of a data point.

This exercise asks you to construct both a scatterplot and a sunflowerplot from the same dataset, one that contains repeated data points. Comparing these plots allows you to see how much information can be lost in a standard scatterplot when some data points appear many times.

```R
library(MASS)
# Set up a side-by-side plot array
par(mfrow = c(1,2))

# Create the standard scatterplot
plot(Boston$zn,Boston$rad)

# Add the title
title("Standard scatterplot")

# Create the sunflowerplot
sunflowerplot(Boston$zn,Boston$rad)

# Add the title
title("Sunflower plot")
```

![](/home/alfonso/Documentos/github/datavisualzation/scatterplot_sunflowerplot.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/scatterplot_sunflowerplot.png)

## Useful options for the boxplot() function

The `boxplot()` function shows how the distribution of a numerical variable `y`differs across the unique levels of a second variable, `x`. To be effective, this second variable should not have too many unique levels (e.g., 10 or fewer is good; many more than this makes the plot difficult to interpret).

The `boxplot()` function also has a number of optional parameters and this exercise asks you to use three of them to obtain a more informative plot:

- `varwidth` allows for variable-width boxplots that show the different sizes of the data subsets.
- `log` allows for log-transformed y-values.
- `las` allows for more readable axis labels.

This exercise also illustrates the use of the formula interface: `y ~ x` indicates that we want a boxplot of the `y` variable across the different levels of the `x`variable. See [`boxplot()`](https://www.rdocumentation.org/packages/graphics/topics/boxplot) for more details.



```
library(MASS)
# Create a variable-width boxplot with log y-axis & horizontal labels
boxplot(crim ~ rad, data = Boston, 
        varwidth = TRUE, log = "y", las = 1)

# Add a title
title("Crime rate vs. radial highway index")
```

![](/home/alfonso/Documentos/github/datavisualzation/boxplot.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/boxplot.png)

## Using the mosaicplot() function

A mosaic plot may be viewed as a scatterplot between categorical variables and it is supported in R with the `mosaicplot()` function.

As this example shows, in addition to categorical variables, this plot can also be useful in understanding the relationship between numerical variables, either integer- or real-valued, that take only a few distinct values.

More specifically, this exercise asks you to construct a mosaic plot showing the relationship between the numerical `carb` and `cyl` variables from the [`mtcars`](https://www.rdocumentation.org/packages/datasets/topics/mtcars) data frame, variables that exhibit 6 and 3 unique values, respectively

```R
# Create a mosaic plot using the formula interface
mosaicplot(carb ~ cyl, data=mtcars)
```

![](/home/alfonso/Documentos/github/datavisualzation/mosaicplot.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/mosaicplot.png)



## Using the bagplot() function

A single box plot gives a graphical representation of the range of variation in a numerical variable, based on five numbers:

The minimum and maximum values
The median (or "middle") value
Two intermediate values called the lower and upper quartiles
In addition, the standard box plot computes a nominal data range from three of these numbers and flags points falling outside this range as outliers, representing them as distinct points.

The bag plot extends this representation to two numerical variables, showing their relationship, both within two-dimensional "bags" corresponding to the "box" in the standard boxplot, and indicating outlying points outside these limits.

This exercise asks you to construct, first, side-by-side box plots of the Min.Price and Max.Price variables from the mtcars dataset, and then to use the bagplot() function from the aplpack package to construct the corresponding bag plot.

```R
library(MASS)
library(aplpack)
# Load aplpack to make the bagplot() function available
library(aplpack)

# Create a side-by-side boxplot summary
boxplot(Cars93$Min.Price,Cars93$Max.Price)

# Create a bagplot for the same two variables
bagplot(Cars93$Min.Price,Cars93$Max,cex=1.2)

# Add an equality reference line
abline(0,1, lty=2)
```



![](/home/alfonso/Documentos/github/datavisualzation/boxplot_vs_bagplot.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/boxplot_vs_baplot.png)

## Plotting correlation matrices with the corrplot() function

Correlation matrices were introduced in the video as a useful tool for obtaining a preliminary view of the relationships between multiple numerical variables.

This exercise asks you to use the `corrplot()` function from the `corrplot`package to visualize this correlation matrix for the numerical variables from the [`UScereal`](https://www.rdocumentation.org/packages/MASS/topics/UScereal) data frame in the `MASS` package. Recall that in this version of these plots, ellipses that are thin and elongated indicate a large correlation value between the indicated variables, while ellipses that are nearly circular indicate correlations near zero.

```R
# Load the corrplot library for the corrplot() function
library(corrplot)
library(MASS)
## set the par paramether
par(mfrow=c(1,2))
# Extract the numerical variables from UScereal
numericalVars <- UScereal[,-c(1,11)]

# Compute the correlation matrix for these variables
corrMat <- cor(numericalVars)
 

# Generate the correlation ellipse plot
corrplot(corrMat,method="ellipse")
corrplot(corrMat,method="color")
```

![](/home/alfonso/Documentos/github/datavisualzation/corrplot.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/corrplot.png)

## Building and plotting rpart() models

It was noted in the video that decision trees represent a popular form of predictive model because they are easy to visualize and explain. It was also noted that the `rpart` package is probably the most popular of several R packages that can be used to build and visualize these models.

This exercise asks you to, first, build a decision tree model using the `rpart()`function from this package, and then display the resulting model structure using the generic functions `plot()` and `text()`.

```R
# Load the rpart library
library(rpart)

# Fit an rpart model to predict medv from all other Boston variables
rpart(medv~.,data=Boston) -> tree_model

# Plot the structure of this decision tree model
plot(tree_model)

# Add labels to this plot
text(tree_model,cex=0.7)
```

![](/home/alfonso/Documentos/github/datavisualzation/treemodel.png)

![](https://github.com/alffajardo/datavisualzation/blob/master/treemodel.png)

## Introduction to the par() function

You already saw how the `mfrow` parameter to the `par()` function could be used to plot multiple graphs in one pane. The `par()` function also allows you to set many other graphics parameters, whose values will remain in effect until they are reset by a subsequent `par()` call.

Specifically, a call to the `par()` function with no parameters specified will return a list whose element names each specify a graphics parameter and whose element values specify the corresponding default values of these parameters. These parameters may be set by a call in the form `par(name = value)` where `name`is the name of the parameter to be set and `value` is the value to be assigned to this parameter.

The purpose of this exercise is to give an idea of what these graphics parameters are. In the subsequent exercises we'll show how some of these parameters can be used to enhance plot results.



```R
# Assign the return value from the par() function to plot_pars
plot_pars <- (par())

# Display the names of the par() function's list elements
names(plot_pars)

# Display the number of par() function list elements
length(plot_pars)
```

## Exploring the type option

One of the important graphics parameters that can be set with the `par()`function is `mfrow`, which specifies the numbers of rows and columns in an array of plots. Valid values for this parameter are two-element numerical vectors, whose first element specifies the number of rows in the plot array and the second element specifies the number of columns.

A more detailed discussion of using the `mfrow` parameter is given in Chapter 4 of this course. For now, note that to specify a plot array with three rows and one column, the command would be `par(mfrow = c(3, 1))`.

The following exercise also introduces the `type` parameter for the `plot()`command, which specifies how the plot is drawn. The specific `type` values used here are:

- `"p"` for "points"
- `"l"` for "lines"
- `"o"` for "overlaid" (i.e., lines overlaid with points)
- `"s"` for "steps"

