+++
title = "Decision trees in R"

date = 2018-11-09T01:25:00
lastmod = 2018-11-09T01:25:00
draft = false

authors = ["nicktheway"]

tags = ["programming", "data science", "R"]
summary = "."

math = true
+++

## What is a decision tree?
{{< figure src="/img/posts/ds_R/dec_trees/dec_tree_wikipedia.png" alt="A decision tree." caption="Source: Wikipedia">}}

A decision tree is a human-friendly data structure that divides a dataset based on its characteristics and makes it
possible to predict the outcome of a new entry by following the tree's path.

For example, using the tree of the image above, we can predict that a till now undocumented woman probably survived on the Titanic.

## Decision trees in R
To create decision trees in R we will use the *rpart* library and the *rpart.plot* library to visualize them. 
To import both of them into the workspace run: 
```r
library(rpart)
## Uncomment to install the rpart.plot package in case you don't have it.
# install.packages("rpart.plot") 
library(rpart.plot)
```

### Using the rpart, rpart.plot libraries
Now that the libraries are imported into our workspace we can create and visualize a decision tree easily.

So, we will use the ```iris``` dataset that is provided with R, in order to make a decision tree to decide on the species of a flower.

To start, we should inspect the data:
```r
str(iris) # Inspect the struct.
```
The above command returns:
```
'data.frame':	150 obs. of  5 variables:
 $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```
Then, create a decision tree for predicting the ```Species``` attribute based on the others run:
```r
iris.model <- rpart(Species ~ ., data = iris)
```
Finally, we can visualize the produced model using the *rpart.plot()* function.
```r
rpart.plot(iris.model)
```
{{< figure src="/img/posts/ds_R/dec_trees/irisDTplot.png" alt="Iris species decision tree.">}}

The nodes of the produced figure indicate the predicted species if we were to stop on this node.
For example, if we stop on the root node, the prediction would be ```setosa``` but with probability 0.33, the same as the other species.

By going down one node, based on the ```Petal.Length``` attribute, the prediction either remains ```setosa``` but now with probability 1
(at least on the training data) or it becomes ```versicolor``` with probability 0.5 and so on.

The percentage probability on each node indicates the percentage of the training data in that node.

As we can see, the produced tree doesn't use the ```Sepal.Length``` and ```Sepal.Width``` attributes. That's because *rpart()* chose
not to. However, if we want to force *rpart()* to use these attributes we can explicitly specify (only) them like this:

```r
iris.model <- rpart(Species ~ Sepal.Length + Sepal.Width, data = iris)
```
This, will make *rpart()* consider only the ```Sepal.Length``` and ```Sepal.Width``` attributes when building the tree.

The ```Species ~ .``` that was used in the formula argument before is equivalent to ```Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width```

For more information about formulas in R you can check this nice [tutorial](https://www.datacamp.com/community/tutorials/r-formula-tutorial) on datacamp.

## Testing the tree.
In order to test the made decision tree we should use different data from the ones used to make it.
So let's divide the ```iris``` dataset to a training sub-dataset and a testing-subdataset. Moreover, to simplify things we will use only the first 100 samples that contain only two species of the flower.

```r
# Choose 40 out of 50 observations of each of the first two species for training.
training.data <- iris[c(1:40, 51:90),] 
# Use the other 10 for testing.
testing.data <- iris[c(41:50, 91:100),]
# Refactor the Species attribute so that the non-existend "virginica" species will be discarded
testing.data$Species <- factor(testing.data$Species)
training.data$Species <- factor(training.data$Species)
# Create two models
iris.modelA <- rpart(Species ~ ., data = training.data)
iris.modelB <- rpart(Species ~ Sepal.Length + Sepal.Width, data = training.data)
```

Now that the models are created we can test them using ```predict()``` and gain some useful metrics of them with the help of the ```MLmetrics``` library.

```r
# Get the attributes of the testing data except from the species one.
xtest <- testing.data[, 1:4] 
# The species attribute will be the correct result that should be produced by the other 4.
ytest <- testing.data[, 5]
# Get the predicted values: The more model(xtest) is near ytest the better the model (usually).
iris.modelA.pred <- predict(iris.modelA, xtest, type = "class")
iris.modelB.pred <- predict(iris.modelB, xtest, type = "class")
# Get the confusion matrices
iris.modelA.cm <- as.matrix(table(Actual = ytest, Predicted = iris.modelA.pred))
iris.modelB.cm <- as.matrix(table(Actual = ytest, Predicted = iris.modelB.pred))
```
By printing the two matrices we can easily see that for our test data, modelA is better than modelB. Of course, that doesn't mean in any
way that it perfect, it ONLY means that it was perfect for the data we tested and trained it with.

```r
> print(iris.modelA.cm)
            Predicted
Actual       setosa versicolor
  setosa         10          0
  versicolor      0         10
> print(iris.modelB.cm)
            Predicted
Actual       setosa versicolor
  setosa         10          0
  versicolor      2          8
```

From these matrices, we can calculate the accuracy, precision, recall and other useful metrics for our models. But ```MLmatrics``` already contains
functions for that, so let's test them on modelB.
```r
library(MLmetrics)  # Load the library.
# The number of right predictions divided by the number of all the predictions.
Accuracy(y_pred = iris.modelB.pred, y_true = ytest)  # 0.9
# How many of the actual "positive" species did the model predict right? (recalled)
MLmetrics::Recall(y_true = ytest, y_pred = iris.modelB.pred, positive = 'setosa')  # 1.0
MLmetrics::Recall(y_true = ytest, y_pred = iris.modelB.pred, positive = 'versicolor')  # 0.8
# How many of the predicted as "positive" species were actually that species.
Precision(y_true = ytest, y_pred = iris.modelB.pred, 'setosa')  # 0.8333333
Precision(y_true = ytest, y_pred = iris.modelB.pred, 'versicolor')  # 1.0
# The harmonic mean of recall and precision.
F1_Score(y_true = ytest, y_pred = iris.modelB.pred, 'setosa')  # 0.9090909
F1_Score(y_true = ytest, y_pred = iris.modelB.pred, 'versicolor')  # 0.8888889
```

Using the above metrics we can compare different models and decide on the one that fits better to our purposes.

## What lies underneath
That's nice but how are these models made? How does *rpart()* decide on the order of the nodes of the tree and on the point 
of seperation of each attribute?

When creating a decision tree, we have to choose these "seperation points" of each characteristic, in other words, the point
that seperates the branches of each node of the tree. 

In some cases, like the root of tree of the titanic example, that is simple as it is just a yes/no question, but
in other cases we have to somehow specify a point of seperation (9.5 and 2.5 in the example) except if we use fuzzy rules
(where we would have to specify these rules, but that is for another article).

Also, we have to choose the order of the nodes of the tree and what attributes will be used to make them.

### The GINI index
A way to do the above is by using the **gini** index. We assign an index to each characteristic and choose the one with the lower index.

The GINI index type for each, splitted into $k$ groups, characteristic ($c$) is the following:
$$\mathit{GINI}\_c = \sum_{i=1}^{k}\frac{n_i}{n}\mathit{GINI}\_c(i)$$
where:
$$n_i = \text{the number of elements in group } i \\\n = \text{the number of elements in all } k \text{ groups}$$
In fact, that is just the weighted mean of the groups' gini values.

For each group $i$ where $c$'s values can be into:

$$\mathit{GINI}\_c(i) = 1 - \sum_{j}p(\mathit{outcome}\in{j}|i)^2$$
where 
$$j \text{: traverses the groups that }\mathit{outcome}\text{'s values can take.}$$ 
$$\mathit{outcome}\text{: is the variable we want to predict using the tree.}$$ eg. the $\mathit{outcome}$ of the titanic example divides into the
{```survived```, ```died```} groups.

Let's see an example using the ```iris``` dataset again:

To find the GINI of the ```Petal.Length``` attribute for predicting the ```Species``` attribute we:

1. Divide the attribute into 2 groups.
2. Find the GINI of each group for all the ```Species``` values.
3. Calculate the total GINI index of the attribute.

```r
# Divide the Petal.Length attribute into Group A (values < 2.5) and Group B (the others).
inGroupA <- iris$Petal.Length < 2.5 # True where Petal.Length is smaller than 2.5.
# Create a probability table for groupA and Species.
probs <- prop.table(table(iris$Species, inGroupA), 2)
# Find the GINI index of groupA.
giniA <- 1 - sum(probs[, 2]^2) # Second column is for inGroupA == True.
giniB <- 1 - sum(probs[, 1]^2)
# Find the frequencies (n_i / n) of the groups' elements.
group.freq <- table(inGroupA) / length(inGroupA)
# Multiply the frequencies with the GINI(i) and sum them.
petalLength.GINI <- sum(group.freq * c(giniB, giniA)) # Returns 0.333...
```
We can summarize the above into a function for calculating GINI indices faster:

```r
gini.calc <- function(outcome, isInGroupArray = NULL){
  if (is.null(isInGroupArray)  # There is no group.
      || sum(isInGroupArray) == length(isInGroupArray)  # All values are in the group.
      || sum(isInGroupArray) == 0  # No value is in the group.
  ){
    # In any of these cases, the group chosen doesn't divide
    # the data in any way, and the gini index is actually the
    # index of the outcome's values group in the data.
    freq <- table(outcome) / length(outcome)
    return(1 - sum(freq ^ 2))
  }
  probs <- prop.table(table(outcome, isInGroupArray), 2)
  gini.inGroup <- 1 - sum(probs[, 2] ^ 2)
  gini.outOfGroup <- 1 - sum(probs[, 1] ^ 2)
  freq <- table(isInGroupArray) / length(isInGroupArray)
  return(sum(freq * c(gini.outOfGroup, gini.inGroup)))
}
```

And test it:
```r
gini.calc(iris$Species, iris$Petal.Length < 4.5) # 0.4419088
gini.calc(iris$Species, iris$Petal.Width < 0.8)  # 0.3333333
gini.calc(iris$Species, iris$Sepal.Width < 3)    # 0.5689493
```

As we can see, all the values calculated are at least 0.33. That's why *rpart()* chose ```Petal.Length < 2.5``` to divide the data initially. ```iris$Petal.Width < 0.8``` would
work the same as well.

### Information Gain
An alternative to the GINI index is the information gain of each attribute. In order to calculate the
information gain of any node, we need to substract the weighted *entropies* of the children nodes from the *outcome*'s *entropy*.

The entropy of each node is:
$$entropy(i) = -\sum_{j=1}^{k}p(\mathit{outcome}\in{j}|i)\cdot log_2(p(\mathit{outcome}\in{j}|i)) $$
where:
$$j \text{: traverses the groups that }\mathit{outcome}\text{'s values can take.}$$ 

Let's make a function that does that:

```r
entropy.calc <- function(outcome, isInGroupArray = NULL){
  if (is.null(isInGroupArray)
      || sum(isInGroupArray) == length(isInGroupArray)
      || sum(isInGroupArray) == 0
  ){
    # In any of these cases, the group chosen doesn't divide
    # the data in any way, and the entropy is actually the
    # entropy of the outcome's values group in the data.
    freq <- table(outcome) / length(outcome)
    return(-sum(freq * log2(freq)))
  }
  probs <- prop.table(table(outcome, isInGroupArray), 2)  # Propability table
  prob.inGroup <- probs[probs[, 2] > 0, 2]  # Probabilities of the values in Group that result in positive outcome.
  prob.outOfGroup <- probs[probs[, 1] > 0, 1]  # Probabilities of the values out of the Group that result in negative outcome.
  entropy.inGroup <- -sum(prob.inGroup * log2(prob.inGroup))  # The entropy of the "in-group" child node.
  entropy.outOfGroup <- -sum(prob.outOfGroup * log2(prob.outOfGroup))  # The entropy of the other child node. 
  freq <- table(isInGroupArray) / length(isInGroupArray)  # The frequency of the elements "going" into each child node.
  return(sum(freq * c(entropy.outOfGroup, entropy.inGroup)))  # The resulting entropy of the node.
}
```

Same to the GINI index, the lower the entropy, the better as the information gain increases with lower entropy.
```r
enAll = entropy.calc(iris$Species) # The outcome's entropy = 1.584963
enA = entropy.calc(iris$Species, iris$Petal.Length < 2.5)  # 0.6666667
enB = entropy.calc(iris$Species, iris$Petal.Length < 4.5)  # 0.9141666
enC = entropy.calc(iris$Species, iris$Sepal.Length < 5)  # 1.401471
infoGainA = enAll - enA  # 0.9182958
infoGainB = enAll - enB  # 0.6707959
infoGainC = enAll - enC  # 0.1834915
```

As with GINI index, the best node to serve as the root of the tree is: ```iris$Petal.Length < 2.5```

### Information gain using rpart
To create a tree based on the information gain use:

```r
rpart(Species ~ .,data=iris,parms = list(split="information"))
```

**Beware**: *rpart()* uses $ln$ to calculate the entropies and not $log_2$.

