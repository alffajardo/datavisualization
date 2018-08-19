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

![](/home/alfonso/Documentos/github/Visualization_R/scatterplot1.png)

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

![](/home/alfonso/Documentos/github/Visualization_R/scatterplot2.png)