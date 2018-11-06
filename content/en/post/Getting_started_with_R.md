+++
title = "Getting started with R"

date = 2018-10-14T18:58:00
lastmod = 2018-10-15T01:52:00
draft = false

authors = ["nicktheway"]

tags = ["programming", "data science", "R"]
summary = "How to install and use R with R-Studio to analyze data."

+++

## Why R?
**R** is an interpreted language that makes working with data as simple as it can get. It is similar to *MATLAB* but
open-source and while it might not have all the possibilities of *MATLAB*, thanks to its huge community, there will usually be
any package we might need for our work.

#### How to install R
You can download and install R from the [r-project website](https://www.r-project.org/).

##### GUI
{{< figure src="/img/posts/ds_R/RStudio_interface.png" alt="RStudio interface">}}

Installing a GUI IDE will make developing with R easier so let's use one. 

After a quick search I found that *RStudio* -which is the most commonly used one- looked like the familiar *MATLAB GUI* so 
I downloaded it and installed it from [here](https://www.rstudio.com/products/rstudio/download/#download).

It certainly makes thinks easier as it makes writing commands in the console, scripts, viewing the workspace, plots, packages
and documentation pretty straightforward.

#### Hello World?
Printing "Hello world" with *R* is really easy as you can see in the [hello world post]({{< ref "hello_world.md" >}}).
Just write the command in the console or in a script and execute it.

### Arrays and matrices
After upholding the "Hello World" tradition let's move into the foundation of the things we'll do with R, which are data related.
Our data will usually be part of vectors so let's see how to make one.

```r
A = c(1, 2, 3)   # Creates a vector of <double> values and stores it to A.
B = c("1", 2, 3) # Creates a vector of <character> values and stores it to B.
?c               # Pops up the documentation of function c() on the "Help" window.
```

{{< figure src="/img/posts/ds_R/RStudio_c.png" alt="RStudio C() function">}}

As you can see the elements stored to **B** are all casted to characters because of "1".
```MATLAB
    To learn about this and more information about c() just check the "Help" 
    window with the documentation of c() that opened using the "?command" notation.
```
A common way to combine vectors together is a matrix. The basic way to create a matrix in R is the following:
```r
values = 1:9    # Stores the <integer> values 1 to 9 into values.
N = 3           # The N of the NxM matrix we want to create.
M = 3           # The M of the NxM matrix we want to create.
A = matrix(values, N, M) # Creates the NxM matrix wrapping the values from col to col.
print(A)        # Prints the matrix A.
B = matrix(values, N, M, byrow = TRUE) # Wraps the values from row to row.
print(B)        
```

{{< figure src="/img/posts/ds_R/RStudio_matrix.png" alt="RStudio matrix() function">}}

### Data Frames
In R, Data Frames are very convenient structures for doing data analysis as they can store different types of data, label
them and provide fast and easy access to their attributes.

We can create a data frame holding some people's data like this:
```r
age = c(24, 18, 44, 26, 34)
height = c(1.95, 1.82, 1.75, 1.78, 1.77)
weight = c(85, 80, 82, 75, 78)
A = data.frame(age, height, weight)
str(A)      # displays the structure of A.
?summary(A) # shows the documentation of summary()
summary(A)
```
{{< figure src="/img/posts/ds_R/RStudio_summary.png" alt="RStudio data frames">}}

Now we have a summary of the characteristics of our 5 people!

To check the cross-correlation between the characteristics we can just run:
```r
cor(A)
```

And in order to check statistic values of the individual characteristics we can use the '$' notation, for example:
```r
var(A$height) # Returns the variance of the height values.
sd(A$height)  # Returns the standard deviation of the height values.
```

We can also plot two characteristics and maybe find a relation between them:
```r
plot(A$weight, A$height)
```

{{< figure src="/img/posts/ds_R/RStudio_plot.png" alt="RStudio individual char. and plot">}}

### A "real" case scenario
To finish this up, let's use the *AirPassengers* dataset that comes built in with R and try to get
information we might want from it.

As we can see in the documentation (```?AirPassengers```) this dataset contains
the numbers of the international airline passengers for each month of the years 1949-1960.

Knowing the above, we can make a matrix with these data putting every year on a different row
and every month on a different column. Also, to make the matrix more readable let's put labels
on each row/column:

```r
year_labels = paste("Y", 1949:1960, sep = "") # Create a vector with Y19xx character elements.
month_labels = month.abb                      # Get a vector with the first 3 letters of each month.
table_labels = list(year_labels, month_labels)# Create a list with the two vectors.
flights = matrix(AirPassengers, byrow=TRUE, ncol=12, dimnames=table_labels) # Create the matrix.
print(flights) # Print the matrix.
```

{{< figure src="/img/posts/ds_R/flights_table.png" alt="AirPassengers data to matrix.">}}

To create a data frame from the table is simple, so let's transpose the matrix first to learn
how to do that to.
```r
?t # View the documentation of the t() function that transposes a matrix.
airdata = as.data.frame(t(flights))
# the "airdata" data frame will contain the years as columns (characteristics)
# and the months as rows (observations).
```
Now, each observation will be a different month's data, and each attribute will be a year.

##### Now let's for example say that we want to learn the mean of passengers that traveled in 1955 per month.
1955's data are an attribute and we have access to each attribute of the data frame with the ```$``` symbol.
Therefore, we can just run:
```r
mean(airdata$Y1955) # answer: 284
```
or ```summary(airdata)``` and check the mean value for ```Y1955``` or any other year.

##### What if we want to know what was the max number of travelers on January and/or February?

Jan and Feb are the first two rows of our data frame and it turns out that we can access
the x'th row by ```airdata[x,]```. So to find the answer we can run:
```r
max(airdata[1:2,])  # answer: 417
```

##### Let's check the correlation of the data:
```r
cor(airdata)        # Prints the cross correlation matrix.
min(cor(airdata))   # Prints the min of the cor matrix.
```

{{< figure src="/img/posts/ds_R/flights_cor.png" alt="AirPassengers correlation.">}}

Interesting! Cross-correlation values close to 1 mean that the data are strongly correlated.
That usually gives us the possibility to assume that the observed pattern will continue in the future.

##### Number of flights per year?
```r
fpy = colSums(airdata)
barplot(fpy, ylim = c(0, 6000)) # Draw a bar plot with the results.
```
And here they are:

{{< figure src="/img/posts/ds_R/fpy_plot.png" alt="AirPassengers flight per year.">}}

Thats it! If we were working for that airline at that time we would probably be happy that
our passengers increase each year.
