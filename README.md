

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

![](https://github.com/alffajardo/datavisualization/blob/master/scatterplot1.png)

![](/home/alfonso/Documentos/github/datavisualization/scatterplot1.png)



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

![](https://github.com/alffajardo/datavisualization/blob/master/scatterplot2.png)



![](/home/alfonso/Documentos/github/datavisualization/scatterplot2.png)

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

![](/home/alfonso/Documentos/github/datavisualization/par.png)
![](https://github.com/alffajardo/datavisualization/blob/master/par.png)

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





![](/home/alfonso/Documentos/github/datavisualization/pie_vs_bar.png)

![](https://github.com/alffajardo/datavisualization/blob/master/pie_vs_bar.png)

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

  ![](/home/alfonso/Documentos/github/datavisualization/hist_truhist.png)

![](https://github.com/alffajardo/datavisualization/blob/master/hist_truhist.png)

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

![](/home/alfonso/Documentos/github/datavisualization/density_histogram.png)

![](https://github.com/alffajardo/datavisualization/blob/master/density_histogram.png)



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

![](https://github.com/alffajardo/datavisualization/blob/master/densities.png)

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

![](/home/alfonso/Documentos/github/datavisualization/scatterplot_sunflowerplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/scatterplot_sunflowerplot.png)

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

![](/home/alfonso/Documentos/github/datavisualization/boxplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/boxplot.png)

## Using the mosaicplot() function

A mosaic plot may be viewed as a scatterplot between categorical variables and it is supported in R with the `mosaicplot()` function.

As this example shows, in addition to categorical variables, this plot can also be useful in understanding the relationship between numerical variables, either integer- or real-valued, that take only a few distinct values.

More specifically, this exercise asks you to construct a mosaic plot showing the relationship between the numerical `carb` and `cyl` variables from the [`mtcars`](https://www.rdocumentation.org/packages/datasets/topics/mtcars) data frame, variables that exhibit 6 and 3 unique values, respectively

```R
# Create a mosaic plot using the formula interface
mosaicplot(carb ~ cyl, data=mtcars)
```

![](/home/alfonso/Documentos/github/datavisualization/mosaicplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/mosaicplot.png)



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



![](/home/alfonso/Documentos/github/datavisualization/boxplot_vs_bagplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/boxplot_vs_bagplot.png)

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

![](/home/alfonso/Documentos/github/datavisualization/corrplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/corrplot.png)

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

![](/home/alfonso/Documentos/github/datavisualization/treemodel.png)

![](https://github.com/alffajardo/datavisualization/blob/master/treemodel.png)

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

```R
# Set up a 2-by-2 plot array
par(mfrow  = c(2,2))

# Plot the Animals2 brain weight data as points
plot(Animals2$brain,type="p")

# Add the title
title("points")

# Plot the brain weights with lines
plot(Animals2$brain,type="l")

# Add the title
title("lines")

# Plot the brain weights as lines overlaid with points
plot(Animals2$brain,type = "o")

# Add the title
title("overlaid")

# Plot the brain weights as steps
plot(Animals2$brain,type="s")

# Add the title
title("steps")
```

![](https://github.com/alffajardo/datavisualization/blob/master/types.png)

## The surprising utility of the type "n" option

The `type = "n"` option was discussed in the video and this exercise provides a simple example. This option is especially useful when we are plotting data from multiple sources on a common set of axes. In such cases, we can compute ranges for the x- and y-axes so that all data points will appear on the plot, and then add the data with subsequent calls to `points()` or `lines()` as appropriate.

This exercise asks you to generate a plot that compares mileage vs. horsepower data from two different sources: the `mtcars`data frame in the `datasets` package and the `Cars93` data frame in the `MASS` package. To distinguish the different results from these data sources, the graphics parameter `pch` is used to specify point shapes. See `?points` for a comprehensive list of some `pch` values and their corresponding point shapes.

```R
# Compute max_hp
max_hp <- max(Cars93$Horsepower, mtcars$hp)

# Compute max_mpg
max_mpg <- max(Cars93$MPG.city, Cars93$MPG.highway,
               mtcars$mpg)

# Create plot with type = "n"               
plot(Cars93$Horsepower, Cars93$MPG.city,
     type = "n", xlim = c(0, max_hp),
     ylim = c(0, max_mpg), xlab = "Horsepower",
     ylab = "Miles per gallon")

# Add open circles to plot
points(mtcars$hp, mtcars$mpg, pch = 1)

# Add solid squares to plot
points(Cars93$Horsepower, Cars93$MPG.city,
       pch = 15)

# Add open triangles to plot
points(Cars93$Horsepower, Cars93$MPG.highway,
       pch = 2)
```

![](https://github.com/alffajardo/datavisualization/blob/master/ntype.png)

## The lines() function and line types

As noted in Chapter 2, numerical data is often assumed to conform approximately to a Gaussian probability distribution, characterized by the bell curve. One point of this exercise is to show what this bell curve looks like for exactly Gaussian data and the other is to show how the `lines()` function can be used to add lines to an existing plot.

The curves you are asked to draw here have the same basic shape but differ in their details (specifically, the means and standard deviations of these Gaussian distributions are different). For this reason, it is useful to draw these curves with different line types to help us distinguish them.

Note that line types are set by the `lty` argument, with the default value `lty = 1` specifying solid lines, `lty = 2`specifying dashed lines, and `lty = 3` specifying dotted lines. Also note that the `lwd` argument specifies the relative width.

```
# Create the numerical vector x
x <- seq(0, 10, length = 200)

# Compute the Gaussian density for x with mean 2 and standard deviation 0.2
gauss1 <- dnorm(x, mean = 2, sd = 0.2)

# Compute the Gaussian density with mean 4 and standard deviation 0.5
gauss2 <- dnorm(x, mean = 4, sd = 0.5)

# Plot the first Gaussian density
plot(x, gauss1, type = "l", ylab = "Gaussian probability density")

# Add lines for the second Gaussian density
lines(x, gauss2, lty = 2, lwd = 3)
```

![](https://github.com/alffajardo/datavisualization/blob/master/lines.png)

## The points() function and point types

One advantage of specifying the `pch` argument locally is that, in a call to functions like `plot()` or `points()`, local specification allows us to make `pch` depend on a variable in our dataset. This provides a simple way of indicating different data subsets with different point shapes or symbols.

This exercise asks you to generate two plots of `mpg` vs. `hp`from the `mtcars` data frame in the `datasets` package. The first plot specifies the point shapes using numerical values of the `pch` argument defined by the `cyl` variable in the `mtcars`data frame. The second plot illustrates the fact that `pch` can also be specified as a vector of *single characters*, causing each point to be drawn as the corresponding character.

```R
plot(mtcars$hp, mtcars$mpg, type = "n",
     xlab = "Horsepower", ylab = "Gas mileage")

# Add points with shapes determined by cylinder number
points(mtcars$hp, mtcars$mpg, pch = mtcars$cyl)

# Create a second empty plot
plot(mtcars$hp, mtcars$mpg, type = "n",
     xlab = "Horsepower", ylab = "Gas mileage")

# Add points with shapes as cylinder characters
points(mtcars$hp, mtcars$mpg, 
       pch = as.character(mtcars$cyl))
```

![](/home/alfonso/Documentos/github/datavisualization/points.png)

![](https://github.com/alffajardo/datavisualization/blob/master/points.png)



## Boxplot with jittering points 

code taken from [here](http://www.r-graph-gallery.com/96-boxplot-with-jitter/)

```R
# Create data
names=c(rep("A", 80) , rep("B", 50) , rep("C", 70))
value=c( rnorm(80 , mean=10 , sd=9) , rnorm(50 , mean=2 , sd=15) , rnorm(70 , mean=30 , sd=10) )
data=data.frame(names,value)
 
# Basic boxplot
boxplot(data$value ~ data$names , col=terrain.colors(4) )
 
# Add data points
mylevels<-levels(data$names)
levelProportions<-summary(data$names)/nrow(data)
 
for(i in 1:length(mylevels)){
 
  thislevel<-mylevels[i]
  thisvalues<-data[data$names==thislevel, "value"]
   
  # take the x-axis indices and add a jitter, proportional to the N in each level
  myjitter<-jitter(rep(i, length(thisvalues)), amount=levelProportions[i]/2)
  points(myjitter, thisvalues, pch=20, col=rgb(0,0,0,.2)) 
   
}
```

![](https://github.com/alffajardo/datavisualization/blob/master/boxplot_jitter.png)

## Adding trend lines from linear regression models

The low-level plot function `abline()` adds a straight line to an existing plot. This line is specified by an intercept parameter `a` and a slope parameter `b`, and the simplest way to set these parameters is directly. For example, the command `abline(a = 0, b = 1)` adds an *equality reference line* with zero intercept and unit (i.e. 1) slope: points for which y = x fall on this reference line, while points with y > x fall above it, and points with y < x fall below it.

An alternative way of specifying these parameters is through a linear regression model that determines them from data. One common application is to generate a scatterplot of y versus x, then fit a linear model that predicts y from x, and finally call `abline()` to add this *best fit* line to the plot.

This exercise asks you to do this for the `Gas` versus `Temp` data from the `whiteside` data frame in the `MASS` package. The standard R function that fits linear regression models is `lm()`, which supports the formula interface. Thus, to fit a linear model that predicts `y` from `x` in the data frame `df`, the call would be `lm(y ~ x, data = df)`. This call returns a linear model object, which can then be passed as an argument to the `abline()` function to draw the desired line on our plot.

```R
# Build a linear regression model for the whiteside data
linear_model <- lm(Gas ~ Temp, data = whiteside)

# Create a Gas vs. Temp scatterplot from the whiteside data
plot(whiteside$Temp, whiteside$Gas)

# Use abline() to add the linear regression line
abline(linear_model,lty=2)
```

![](/home/alfonso/Documentos/github/datavisualization/abline.png)

![](https://github.com/alffajardo/datavisualization/blob/master/abline.png)



## Using the text() function to label plot features

One of the main uses of the `text()` function is to add informative labels to a data plot. The `text()` function takes three arguments:

- `x`, which specifies the value for the x variable,
- `y`, which specifies the value for the y variable, and
- `label`, which specifies the label for the x-y value pair.

This exercise asks you to first create a scatterplot of city gas mileage versus horsepower from the `Cars93` data, then identify an interesting subset of the data (i.e. the 3-cylinder cars) and label these points. You will find that assigning a vector to the `x`, `y`, and `label` arguments to `text()` will result in labeling multiple points at once.

```R
# Create MPG.city vs. Horsepower plot with solid squares
plot(Cars93$Horsepower, Cars93$MPG.city, pch = 15)

# Create index3, pointing to 3-cylinder cars
index3 <- which(Cars93$Cylinders == 3)

# Add text giving names of cars next to data points
text(x = Cars93$Horsepower[index3], 
     y = Cars93$MPG.city[index3],
     labels = Cars93$Make[index3], adj = 0)
```

![](/home/alfonso/Documentos/github/datavisualization/text.png)

![](https://github.com/alffajardo/datavisualization/blob/master/text.png)

## Adjusting text position, size, and font

The previous exercise added explanatory text to a scatterplot. The purpose of this exercise is to improve this plot by modifying the text placement, increasing the text size, and displaying the text in boldface.

It was noted that the `adj` argument to the `text()` function determines the horizontal placement of the text and it can take any value between 0 and 1. In fact, this argument can take values outside this range. That is, making this value negative causes the text to start to the right of the specified `x` position. Similarly, making `adj` greater than 1 causes the text to end to the left of the `x` position.

Another useful optional argument for the `text()` function is `cex`, which scales the default text size. As a specific example, setting `cex = 1.5` increases the text size by 50 percent, relative to the default value. Similarly, specifying `cex = 0.8` reduces the text size by 20 percent.

Finally, the third optional parameter used here is `font`, which can be used to specify one of four text fonts: `font = 1` is the default text font (neither italic nor bold), `font = 2` specifies bold face text, `font = 3` specifies italic text, and `font = 4` specifies both bold and italic text.



```R
# Plot MPG.city vs. Horsepower as open circles
plot(Cars93$Horsepower,Cars93$MPG.city)
# Create index3, pointing to 3-cylinder cars
index3 <- which(Cars93$Cylinders==3)

# Highlight 3-cylinder cars as solid circles
points(Cars93$Horsepower[index3],Cars93$MPG.city[index3],pch=16)

# Add car names, offset from points, with larger bold text
text(x = Cars93$Horsepower[index3],
      y = Cars93$MPG.city[index3],
      labels= Cars93$Make[index3],
      adj= -0.2,
      cex= 1.2,
      font= 4)
```

![](/home/alfonso/Documentos/github/datavisualization/text_parameter.png)

![](https://github.com/alffajardo/datavisualization/blob/master/text_parameter.png)

## Rotating text with the srt argument

In addition to the optional arguments used in the previous exercises, the `text()` function also supports a number of other optional arguments that can be used to enhance the text. This exercise uses the `cex` argument to reduce the text size and introduces two new arguments. The first is the `col` argument that specifies the color used to display the text, and the second is the `srt` argument that allows us to rotate the text.

Color has been used in several of the previous exercises to specify point colors, and the effective use of color is discussed further in Chapter 5. One of the points of this exercise is to show that the specification of text color with the `text()` function is essentially the same as the specification of point color with the `plot()` function. As a specific example, setting `col = "green"` in the `text()` function causes the text to appear in green. If `col` is not specified, the text appears in the default color set by the `par()` function, which is typically black.

The `srt` parameter allows us to rotate the text through an angle specified in degrees. The typical default value (set by the `par()` function) is 0, causing the text to appear horizontally, reading from left to right. Specifying `srt = 90` causes the text to be rotated 90 degrees counter-clockwise so that it reads from bottom to top instead of left to right.

```R
# Plot Gas vs. Temp as solid triangles
plot(whiteside$Temp, whiteside$Gas, pch = 17)

# Create indexB, pointing to "Before" data
indexB <- which(whiteside$Insul == "Before")

# Create indexA, pointing to "After" data
indexA <- which(whiteside$Insul == "After")

# Add "Before" text in blue, rotated 30 degrees, 80% size
text(x = whiteside$Temp[indexB], y = whiteside$Gas[indexB],
     labels = "Before", col = "blue", srt = 30, cex = 0.8)

# Add "After" text in red, rotated -20 degrees, 80% size
text(x = whiteside$Temp[indexA], y = whiteside$Gas[indexA],
     labels = "After", col = "red", srt = -20, cex = 0.8)
```

![](/home/alfonso/Documentos/github/datavisualization/srt.png)

![](https://github.com/alffajardo/datavisualization/blob/master/srt.png)

## Using the legend() function

The video described and illustrated the use of the `legend()` function to add explanatory text to a plot.

This exercise asks you to first create a scatterplot and then use this function to add explanatory text for the point shapes that identify two different data subsets.

```R
# Set up and label empty plot of Gas vs. Temp
plot(whiteside$Temp,whiteside$Gas,
     type = "n", xlab = "Outside temperature",
     ylab = "Heating gas consumption")

# Create indexB, pointing to "Before" data
indexB <- which(whiteside$Insul=="Before")

# Create indexA, pointing to "After" data
indexA <-  which(whiteside$Insul=="After")

# Add "Before" data as solid triangles
points(whiteside$Temp[indexB],whiteside$Gas[indexB],pch=17)

# Add "After" data as open circles
points(whiteside$Temp[indexA],whiteside$Gas[indexA],pch=1)

# Add legend that identifies points as "Before" and "After"
legend("topright", pch = c(17,1), 
legend = c("Before","After"))
```

![](/home/alfonso/Documentos/github/datavisualization/legends.png)

![](https://github.com/alffajardo/datavisualization/blob/master/legends.png)

																																																																																																																																																							

## Adding custom axes with the axis() function

Typical base graphics functions like `boxplot()` provide x- and y-axes by default, with a label for the x-axis below the plot and one for the y-axis label to the left of the plot. These labels are generated automatically from the variable names used to generate the plot. Sometimes, we want to provide our own axes labels, and R makes this possible in two steps: first, we suppress the default axes when we create the plot by specifying `axes = FALSE`; then, we call the low-level graphics function `axis()` to create the axes we want.

In this exercise, you're asked to create your own labels using the `axis()` function with the `side`, `at`, and `labels` arguments. The `side` argument tells the function which axis to create: a value of 1 adds an axis below the plot; 2 adds an axis on the left; 3 puts it across the top; and 4 puts it on the right side. The second argument, `at`, is a vector that defines points where tick-marks will be drawn on the axis. The third argument, `labels`, is a vector that defines labels at each of these tick-marks.

One example of a boxplot with custom axes was presented in the video. This exercise asks you to create another example showing the relationship between the `sugars` variable and the `shelf` variable from the `UScereal` data frame in the `MASS` package.

```R
# Create a boxplot of sugars by shelf value, without axes
boxplot(sugars ~ shelf, data = UScereal,
        axes = FALSE)

# Add a default y-axis to the left of the boxplot
axis(side = 2)

# Add an x-axis below the plot, labelled 1, 2, and 3
axis(side = 1, at = c(1, 2, 3))

# Add a second x-axis above the plot
axis(side = 3, at = c(1, 2, 3),
     labels = c("floor", "middle", "top"))
```

![](/home/alfonso/Documentos/github/datavisualization/axis.png)

![](https://github.com/alffajardo/datavisualization/blob/master/axis.png)

##  Using the supsmu() function to add smooth trend curves

As we saw in the video, some scatterplots exhibit fairly obvious trends that are not linear. In such cases, we may want to add a curved trend line that highlights this behavior of the data and the `supsmu()` function represents one way of doing this.

To use this function, we need to specify values for the required arguments `x` and `y`, but it also has a number of optional arguments. Here, we consider the optional `bass` argument, which controls the degree of smoothness in the resulting trend curve. The default value is 0, but specifying larger values (up to a maximum of 10) results in a smoother curve. This exercise asks you to use the `supsmu()` function to add two trend lines to a scatterplot, one using the default parameters and the other with increased smoothness.

```R
# Create a scatterplot of MPG.city vs. Horsepower
plot(Cars93$Horsepower,Cars93$MPG.city)

# Call supsmu() to generate a smooth trend curve, with default bass
trend1 <- supsmu(Cars93$Horsepower,Cars93$MPG.city,bass =0)

# Add this trend curve to the plot
lines(trend1)

# Call supsmu() for a second trend curve, with bass = 10
trend2 <- supsmu(Cars93$Horsepower,Cars93$MPG.city,bass =10)

# Add this trend curve as a heavy, dotted line
lines(trend2,lty=3,lwd=2)
```

![](/home/alfonso/Documentos/github/datavisualization/supsmu.png)

![](https://github.com/alffajardo/datavisualization/blob/master/supsmu.png)

## Too much is too much

The first example presented in Chapter 1 applied the `plot()` function to a data frame, yielding an array of scatterplots with one for each pair of columns in the data frame. Thus, the number of plots in this array is equal to the square of the number of columns in the data frame.

This means that if we apply the `plot()` function to a data frame with many columns, we will generate an enormous array of scatterplots, each of which will be too small to be useful. The purpose of this exercise is to provide a memorable example. 	



```R
# Compute the number of plots to be displayed
ncol(Cars93)^2

# Plot the array of scatterplots
plot(Cars93)
```



![](/home/alfonso/Documentos/github/datavisualization/array.png)

![](https://github.com/alffajardo/datavisualization/blob/master/array.png)

## Deciding how many scatterplots is too many

As noted in the video, the `matplot()` function can be used to easily generate a plot with several scatterplots on the same set of axes. By default, the points in these scatterplots are represented by the numbers 1 through n, where n is the total number of scatterplots included, but most of the options available with the `plot()` function are also possible by specifying the appropriate arguments.

This exercise asks you to set up a plot array with four of these multiple scatterplot displays, each including one more scatterplot than the previous one. The point of this exercise is to encourage you to judge for yourself *how many scatterplots is too many?*.

```R
# Construct the vector keep_vars
keep_vars <- c("calories", "protein", "fat",
               "fibre", "carbo", "sugars")

# Use keep_vars to extract the desired subset of UScereal
df <- UScereal[, keep_vars]

# Set up a two-by-two plot array
par(mfrow = c(2, 2))

# Use matplot() to generate an array of two scatterplots
matplot(df$calories, df[, c("protein", "fat")], 
        xlab = "calories", ylab = "")

# Add a title
title("Two scatterplots")

# Use matplot() to generate an array of three scatterplots
matplot(df$calories, df[, c("protein", "fat", "fibre")], 
        xlab = "calories", ylab = "")

# Add a title
title("Three scatterplots")

# Use matplot() to generate an array of four scatterplots
matplot(df$calories, 
        df[, c("protein", "fat", "fibre", "carbo")], 
        xlab = "calories", ylab = "")

# Add a title
title("Four scatterplots")

# Use matplot() to generate an array of five scatterplots
matplot(df$calories, 
        df[, c("protein", "fat", "fibre", "carbo", "sugars")], 
        xlab = "calories", ylab = "")

# Add a title
title("Five scatterplots")
```

![](/home/alfonso/Documentos/github/datavisualization/matplot.png)

![](https://github.com/alffajardo/datavisualization/blob/master/matplot.png)

## How many words is too many?

The main point of the previous two exercises has been to show that scatterplot arrays lose their utility if they are allowed to become too complex, either including too many plots, or attempting to include too many scatterplots on one set of axes. More generally, *any* data visualization loses its utility if it becomes too complex.

This exercise asks you to consider this problem with *wordclouds*, displays that present words in varying sizes depending on their frequency. That is, more frequent words appear larger in the display, while rarer words appear in a smaller font.

In R, wordclouds are easy to generate with the `wordcloud()`function in the `wordcloud` package. This function is called with a character vector of words, and a second numerical vector giving the number of times each word appears in the collection used to generate the wordcloud.

Two other useful arguments for this function are `scale` and `min.freq`. The `scale` argument is a two-component numerical vector giving the relative size of the largest word in the display and that of the smallest word. The wordcloud only includes those words that occur at least `min.freq` times in the collection and the default value for this argument is 3.

```
if(!require(wordcloud)){
    install.packages("wordcloud")
    library(wordcloud)
    
```

![](/home/alfonso/Documentos/github/datavisualization/wordclouds.png)

![](https://github.com/alffajardo/datavisualization/blob/master/wordclouds.png)

# The Anscombe quartet

This exercise and the next one are based on the *Anscombe quartet*, a collection of four datasets that appear to be essentially identical on the basis of simple summary statistics like means and standard deviations. For example, the mean x-values for these datasets are identical to three digits, while the mean y-values differ only in the third digit.

In spite of these apparent similarities, the behavior of the four datasets is quite different and this becomes immediately apparent when we plot them.

```R
library(carData)
# Set up a two-by-two plot array
par(mfrow=c(2,2))

# Plot y1 vs. x1 
plot(anscombe$x1,anscombe$y1)

# Plot y2 vs. x2
plot(anscombe$x2,anscombe$y2)

# Plot y3 vs. x3
plot(anscombe$x3,anscombe$y3)

# Plot y4 vs. x4
plot(anscombe$x4,anscombe$y4)
```



![](/home/alfonso/Documentos/github/datavisualization/anscombe_quartet.png)

![](https://github.com/alffajardo/datavisualization/blob/master/anscombe_quartet.png)

## The utility of common scaling and individual titles

The plots you generated in the previous exercise showed that the four Anscombe quartet datasets have very different appearances, but a careful examination of these plots reveals that they exhibit different ranges of x and y values.

The point of this exercise is to illustrate how much more clearly we can see the differences in these datasets if we plot all of them with the same x and y ranges. This exercise also illustrates the utility of improving the x- and y-axis labels and of adding informative plot titles.

```R
# Define common x and y limits for the four plots
xmin <- 4
xmax <- 19
ymin <- 3
ymax <- 13

# Set up a two-by-two plot array
par(mfrow = c(2, 2))

# Plot y1 vs. x1 with common x and y limits, labels & title
plot(anscombe$x1, anscombe$y1,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "First dataset")

# Do the same for the y2 vs. x2 plot
plot(anscombe$x2, anscombe$y2,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Second dataset")

# Do the same for the y3 vs. x3 plot
plot(anscombe$x3, anscombe$y3,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Third dataset")

# Do the same for the y4 vs. x4 plot
plot(anscombe$x4, anscombe$y4,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Fourth dataset")
```

![](/home/alfonso/Documentos/github/datavisualization/limits.png)

![](https://github.com/alffajardo/datavisualization/blob/master/limits.png)

## Using multiple plots to give multiple views of a dataset

As noted in the video, another useful application of multiple plot arrays besides comparison is presenting multiple related views of the same dataset.

This exercise illustrates this idea, giving four views of the same dataset: a plot of the raw data values themselves, a histogram of these data values, a density plot, and a normal QQ-plot.

```R
 # Set up a two-by-two plot array
par(mfrow = c(2, 2))

# Plot the raw duration data
plot(geyser$duration, main = "Raw data")

# Plot the normalized histogram of the duration data
truehist(geyser$duration, main = "Histogram")

# Plot the density of the duration data
plot(density(geyser$duration), main = "Density")

# Construct the normal QQ-plot of the duration data
qqPlot(geyser$duration, main = "QQ-plot")
```

![](https://github.com/alffajardo/datavisualization/blob/master/multiple_plots.png)

## Constructing and displaying layout matrices

The video illustrated how to set up a layout matrix to be used with the `layout()` function in creating a plot array. You can think of the layout matrix as the plot pane, where a 0 represents empty space and other numbers represent the plot number, which is determined by the sequence of visualization function calls. For example, a 1 in the layout matrix refers to the visualization that was first called, a 2 refers to the plot of the second visualization call, etc. This exercise asks you to create your own 3 x 2 layout matrix, using the `c()` function to concatenate numbers into vectors that will form the rows of the matrix.

You will then use the `matrix()` function to convert these rows into a matrix and apply the `layout()` function to set up the desired plot array. The convenience function `layout.show()` can then be used to verify that the plot array has the shape you want.

As reference, feel free to use the slides from the video, which you can access by clicking on the "Slides" tab next to the "R Console" tab. Pay close attention to the example layout matrix and the resulting plot array.

```R
# Use the matrix function to create a matrix with three rows and two columns
layoutMatrix <- matrix(
  c(
    0, 1,
    2, 0,
    0, 3
  ), 
  byrow = T, 
  nrow = 3
)

# Call the layout() function to set up the plot array
layout(layoutMatrix)

# Show where the three plots will go 
layout.show(n=3)
```

![](/home/alfonso/Documentos/github/datavisualization/layout.png)

![](https://github.com/alffajardo/datavisualization/blob/master/layout.png)

# Creating a triangular array of plots

The previous exercise asked you to create a plot array using the `layout()` function. Recall the layout matrix from the previous exercise:

```
> layoutMatrix
     [,1] [,2]
[1,]    0    1
[2,]    2    0
[3,]    0    3
```

This exercise asks you to use this array to give three different views of the `whiteside` data frame.

 ```R
# Set up the plot array
layout(layoutMatrix)

# Construct vectors indexB and indexA
indexB <- which(whiteside$Insul == "Before")
indexA <- which(whiteside$Insul == "After")

# Create plot 1 and add title
plot(whiteside$Temp[indexB], whiteside$Gas[indexB],
     ylim = c(0,8))
title("Before data only")

# Create plot 2 and add title
plot(whiteside$Temp,whiteside$Gas,ylim=c(0,8))
title("Complete dataset")



# Create plot 3 and add title
plot(whiteside$Temp[indexA],whiteside$Gas[indexA],ylim=c(0,8))
title("After data only")

 ```

![](/home/alfonso/Documentos/github/datavisualization/triangle_layout.png)

![](https://github.com/alffajardo/datavisualization/blob/master/triangle_layout.png)

## Creating arrays with different sized plots

Besides creating non-rectangular arrays, the `layout()`function can be used to create plot arrays with different sized component plots -- something else that is not possible by setting the `par()` function's `mfrow` parameter.

This exercise illustrates the point, asking you to create a standard scatterplot of the `zn` versus `rad` variables from the `Boston` data frame as a smaller plot in the upper left, with a larger sunflower plot of the same data in the lower right.

```R
# Create row1, row2, and layoutVector
row1 <- c(1,0,0)
row2 <- c(0,2,2)
layoutVector <- c(row1,row2,row2)

# Convert layoutVector into layoutMatrix
layoutMatrix <- matrix(layoutVector,byrow=T, nrow=3)

# Set up the plot array
layout(layoutMatrix)

# Plot scatterplot
plot(Boston$rad,Boston$zn)

# Plot sunflower plot
sunflowerplot(Boston$rad,Boston$zn)
```

![](https://github.com/alffajardo/datavisualization/blob/master/diferent_size.png)

## Some plot functions also return useful information

As shown in the video, calling the `barplot()` function has the *side effect* of creating the plot we want, but it also returns a numerical vector with the center positions of each bar in the plot. This value is returned invisibly so we don't normally see it, but we can capture it with an assignment statement.

These return values can be especially useful when we want to overlay text on the bars of a horizontal barplot. Then, we capture the return values and use them as the `y` parameter in a subsequent call to the `text()` function, allowing us to place the text at whatever `x` position we want but overlaid in the middle of each horizontal bar. This exercise asks you to construct a horizontal barplot that exploits these possibilities.

Feel free to reference a similar example in the slides by clicking on the "Slides" tab next to the "R Console" tab.

```R
# Create a table of Cylinders frequencies
tbl <- table(Cars93$Cylinders)

# Generate a horizontal barplot of these frequencies
mids <- barplot(tbl, horiz = TRUE, 
                col = "transparent",
                names.arg = "")

# Add names labels with text()
text(20, mids, names(tbl))

# Add count labels with text()
text(35, mids, as.numeric(tbl))
```

![](https://github.com/alffajardo/datavisualization/blob/master/text_barplot.png)

## Using the symbols() function to display relations between more than two variables

The scatterplot allows us to see how one numerical variable changes with the values of a second numerical variable. The `symbols()` function allows us to extend scatterplots to show the influence of other variables.

This function is called with the variables `x` and `y` that define a scatterplot, along with another argument that specifies one of several possible shapes. Here, you are asked to use the `circles` argument to create a *bubbleplot* where each data point is represented by a circle whose radius depends on the third variable specified by the value of this argument.

```R
# Call symbols() to create the default bubbleplot
symbols(Cars93$Horsepower, Cars93$MPG.city,
        circles = sqrt(Cars93$Price))

# Repeat, with the inches argument specified
symbols(Cars93$Horsepower, Cars93$MPG.city,
        circles = sqrt(Cars93$Price),
        inches = 0.1)
```

![](https://github.com/alffajardo/datavisualization/blob/master/bubbleplot.png)

## Useful code to generate barplots 

###### simple bar plot

```R
# generate frequency table
counts <- table(mtcars$gear)
# plot the frequency tabel
barplot(counts, main="Car Distribution", 
  	xlab="Number of Gears")
```

if we want to plot in horizontal position simply modify `horiz=T` within the barplot function.

![](htpps://github.com/alffajardo/datavisualization/blob/master/simple_barplots.png)

#####  Stacked barplot

```R
# Stacked Bar Plot with Colors and Legend
counts <- table(mtcars$vs, mtcars$gear)
barplot(counts, main="Car Distribution by Gears and VS",
  xlab="Number of Gears", col=c("darkblue","red"),
 	legend = rownames(counts))
```



![](https://github.com/alffajardo/datavisualization/blob/masgter/stacked_barplot.png)

###### Notes:

Bar plots need not be based on counts or frequencies. You can create bar plots that represent means, medians, standard deviations, etc. Use the [aggregate( ) ](https://www.statmethods.net/management/aggregate.html)function and pass the results to the barplot( ) function.

By default, the categorical axis line is suppressed. Include the option **axis.lty=1** to draw it.

With many bars, bar labels may start to overlap. You can decrease the font size using the **cex.names =** option. Values smaller than one will shrink the size of the label. Additionally, you can use [graphical parameters](https://www.statmethods.net/advgraphs/parameters.html) such as the following to help text spacing:

##### Grouped barplot

To indicate you want a grouped bar set t `beside=TRUE`

```
# Grouped Bar Plot
counts <- table(mtcars$vs, mtcars$gear)
barplot(counts, main="Car Distribution by Gears and VS",
  xlab="Number of Gears", col=c("darkblue","red"),
 	legend = rownames(counts), beside=TRUE)
```

![](https://github.com/alffajardo/datavisualization/blob/master/grouped_barplot.png)

##### Building r plot with error bars

```R
# Generate an R object with de descriptive statistics to compute
myData <- aggregate(mtcars$mpg,
    by = list(cyl = mtcars$cyl, gears = mtcars$gear),
    FUN = function(x) c(mean = mean(x), sd = sd(x),
                        n = length(x)))
# After this, we’ll need to do a little manipulation since the previous function 
# returned matrices instead of vectors
# Here `do.call` is an usefull way to call a function.

                    myData <- do.call(data.frame, myData)

# The following code is intended to calculate standard error for each group
 myData$se <- myData$x.sd / sqrt(myData$x.n)

colnames(myData) <- c("cyl", "gears", "mean", "sd", "n", "se")

myData$names <- c(paste(myData$cyl, "cyl /",
                        myData$gears, " gear"))                 
# Now that colummns have been renamed the following code will construct the plot
                    par(mar = c(5, 6, 4, 5) + 0.1)

plotTop <- max(myData$mean) +
           myData[myData$mean == max(myData$mean), 6] * 3 # usful to set y limits

barCenters <- barplot(height = myData$mean,
                  names.arg = myData$names,
                  beside = T, las = 2,
                  ylim = c(0, plotTop),
                  cex.names = 0.75, xaxt = "n",
                  main = "Mileage by No. Cylinders and No. Gears",
                  ylab = "Miles per Gallon",
                  border = "black", axes = TRUE)

# Specify the groupings. We use srt = 45 for a
# 45 degree string rotation
text(x = barCenters, y = par("usr")[3] - 1, srt = 45,
     adj = 1, labels = myData$names, xpd = TRUE)

segments(barCenters, myData$mean - myData$se * 2, barCenters,
         myData$mean + myData$se * 2, lwd = 1.5)

arrows(barCenters, myData$mean - myData$se * 2, barCenters,
       myData$mean + myData$se * 2, lwd = 1.5, angle = 90,
       code = 3, length = 0.05)
```

![](https://github.com/alffajardo/datavisualization/blob/master/barplot_errorbars.png)

## grouped bar with barplots

```R
#Let's build a dataset : height of 10 sorgho and poacee sample in 3 environmental conditions (A, B, C)
A=c(rep("sorgho" , 10) , rep("poacee" , 10) )
B=rnorm(20,10,4)
C=rnorm(20,8,3)
D=rnorm(20,5,4)
data=data.frame(A,B,C,D)
colnames(data)=c("specie","cond_A","cond_B","cond_C")
 
#Let's calculate the average value for each condition and each specie with the *aggregate* function
bilan=aggregate(cbind(cond_A,cond_B,cond_C)~specie , data=data , mean)
rownames(bilan)=bilan[,1]
bilan=as.matrix(bilan[,-1])
 
#Then it is easy to make a classical barplot :
lim=1.2*max(bilan)
ze_barplot = barplot(bilan , beside=T , legend.text=T , col=c("blue" , "skyblue") , ylim=c(0,lim))
 
#I becomes a bit more tricky when we want to add the error bar representing the confidence interval.
 
#First I create a smell function that takes...in entry
error.bar <- function(x, y, upper, lower=upper, length=0.1,...){
  arrows(x,y+upper, x, y-lower, angle=90, code=3, length=length, ...)
}
 
#Then I calculate the standard deviation for each specie and condition :
stdev=aggregate(cbind(cond_A,cond_B,cond_C)~specie , data=data , sd)
rownames(stdev)=stdev[,1]
stdev=as.matrix(stdev[,-1]) * 1.96 / 10
 
# I am ready to add the error bar on the plot using my "error bar" function !
ze_barplot = barplot(bilan , beside=T , legend.text=T,col=c("blue" , "skyblue") , ylim=c(0,lim) , ylab="height")
error.bar(ze_barplot,bilan, stdev)
```

![](https://github.com/alffajardo/datavisualization/blob/master/barplot_ex2.png)

## Another example of grouped bar plot

```R
# Create a table summarizing variables (mean,sample size, and standard deviation)
myData <- aggregate(mtcars$mpg,
    by = list(cyl = mtcars$cyl, gears = mtcars$gear),
    FUN = function(x) c(mean = mean(x), sd = sd(x),
                        n = length(x)))
# Transform into a data frame
myData <- do.call(data.frame, myData)

# compute standard error
                    myData$se <- myData$x.sd / sqrt(myData$x.n)

colnames(myData) <- c("cyl", "gears", "mean", "sd", "n", "se")

myData$names <- c(paste(myData$cyl, "cyl /",
                        myData$gears, " gear"))
                    
# En el caso de que se quiera hacer una gráfica con barras agrupadas es mejor realizar una matrix con las medias. esto es facil de hacer con R de la siguiente forma:
                    
                    tabbedMeans <- tapply(myData$mean, list(myData$cyl,
                                      myData$gears),
                         function(x) c(x = x))
tabbedSE <- tapply(myData$se, list(myData$cyl,
                                      myData$gears),
                         function(x) c(x = x))
# Graficamos las tablas generadas:
                   par(mar = c(5, 6, 4, 5) + 0.1)

                   plotTop <- max(myData$mean) +
           myData[myData$mean == max(myData$mean), 6] * 3 # usful to set y limits

                   barCenters <- barplot(height = tabbedMeans,
                      beside = TRUE, las = 1,
                      ylim = c(0, plotTop),
                      cex.names = 0.75,
                      main = "Mileage by No. Cylinders and No. Gears",
                      ylab = "Miles per Gallon",
                      xlab = "No. Gears",
                      border = "black", axes = TRUE,
                      legend.text = TRUE,
                      args.legend = list(title = "No. Cylinders", 
                                         x = "topright",
                                         cex = .7))
                   segments(barCenters, tabbedMeans - tabbedSE * 2, barCenters,
         tabbedMeans + tabbedSE * 2, lwd = 1.5)
                   arrows(barCenters, tabbedMeans - tabbedSE * 2, barCenters,
       tabbedMeans + tabbedSE * 2, lwd = 1.5, angle = 90,
       code = 3, length = 0.05)
```

![](https://github.com/alffajardo/datavisualization/blob/master/grouped_barplot3.png)







## Saving plot results as files

In an interactive R session, we typically generate a collection of different plots, often using the results to help us decide how to proceed with our analysis. This is particularly true in the early phases of an exploratory data analysis, but once we have generated a plot we want to share with others, it is important to save it in an external file.

R provides support for saving graphical results in several different external file formats, including jpeg, png, tiff, or pdf files. In addition, we can incorporate graphical results into external documents, using tools like the `Sweave()` function or the `knitr` package. One particularly convenient way of doing this is to create an R Markdown document, an approach that forms the basis for another DataCamp course.

Because png files can be easily shared and viewed as e-mail attachments and incorporated into many slide preparation packages (e.g. Microsoft Powerpoint), this exercise asks you to create a plot and save it as a png file. The basis for this process is the `png()` function, which specifies the name of the png file to be generated and sets up a special environment that captures all graphical output until we exit this environment with the `dev.off()` command.

##  Another two awsome barplots

```R
# Create two awsome barplots
library(MASS)
par(mar=c(7, 4, 2, 2) + 0.2,mfrow=c(2,1))

animals <- data.frame( "specie" = row.names(Animals),Animals,brain_index = Animals$brain/ (0.12*(Animals$body^0.67)))
animals <- animals[with(animals,order(-brain)),]
end_point <- 0.5 +nrow(animals) + nrow(animals) 
barplot(animals$brain,main= "Brain weight by specie",xlab="",
        ylab= "brain weigth (grams)",col="turquoise3",space = 2,
        ylim=c(0,max(animals$brain)+300)) 
text(x = seq(2.5,86,by=3),adj=1,srt=65,xpd=T,par("usr")[3],cex=0.8,
          labels = as.character(animals$specie),font= 2)

#### ploting brain index
animals <- animals[with(animals,order(-brain_index)),]

barplot(animals$brain_index,main= "Brain weight relative to body mass",xlab="",
        ylab= "brain index (grams/kg)",col="skyblue",space = 2,
         ylim=c(0,max(animals$brain_index)+200)) 
text(x = seq(2.5,86,by=3),adj=1,srt=65,xpd=T,par("usr")[3],cex=0.8,
     labels = as.character(animals$specie),font= 2)
```

![](https://github.com/alffajardo/datavisualization/blob/master/brain_barplots.png)

## linechart categorical varibles vs continues`` 

```R
#create new spaguetti plot with the data

## First create data s
conditions <- paste("RM",1:4,sep="_")
subjects <- paste("rat",1:20,sep="_")
n <- 10
s <- 4
values <- c()

for ( i in 1:n){
 values <- rbind(values,rnorm(s,20,sd=runif(1)*10))
}
for (i in 1:n){
  values <- rbind(values,rnorm(s,5,sd=runif(1)*10))
}
group <- as.factor(c(rep("sham",10),rep("pilo",10)))

data <- data.frame(group,values,row.names = subjects)
names(data) <- c("Group",conditions)
###
Nr <- length(data[,1])
Nc <- length(data[1,]) - 1
# A vector of colours for each condition
colv <- c("darkred","darkgreen")[data$Group]
ltyv <- c(1,2)[data$Group]
x <- 1:Nc
ylimit <- c(floor(min(data[,2:5])/5)*5,ceiling(max(data[,2:5])/5)*5)

## create empty plot
plot(1:Nr,data$RM_1,type = "n",main = "Rs- BOLD",xlim=c(0,Nc),
     ylim = ylimit,axes = F, xlab = '',ylab='')
## add x axis
axis(1, at = seq(0,3,by = 1
                 ) ,lwd=2,labels = conditions)
axis(2, at = seq(ylimit[1],ylimit[2],by = 5),lwd=2)

## draw the lines

for (i in 1:Nr){
  lines(0:3,data[i,2:5],col=colv[i],lwd=2,lty=ltyv[i])
}
## draw the pints
for (i in 1:Nr){
 points(0:3,data[i,2:5],col=colv[i],cex=1.5,pch=16)
}  
## add a legend

legend(x=3,y=40,lty=c(2,1),legend = c("sham","pilo"),col=c("darkgreen","darkred"),
       cex=0.5,border = F,xpd = T)
```

![](https://github.com/alffajardo/datavisualization/blob/master/linechart_points.png)

