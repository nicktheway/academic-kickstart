+++
title = "Decision trees in R"

date = 2018-10-14T18:58:00
lastmod = 2018-10-27T18:52:00
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
For example, if we stop on the root node, the prediction would be ```setosa``` but with probability 0.33, the same as the species.

By going down one node, based on the ```Petal.Lenght``` attribute, the prediction either remains ```setosa``` but now with probability 1
(at least on the training data) or it becomes ```versicolor``` with probability 0.5 and so on.

The percentage probability on each node indicates the percentage of the training data in that node.

As we can see, the produced tree doesn't use the ```Sepal.Lenght``` and ```Sepal.Width``` attributes. That's because *rpart()* chose
not to. However, if we want to force *rpart()* to use these attributes we can explicitly specify (only) them like this:

```r
iris.model <- rpart(Species ~ Sepal.Length + Sepal.Width, data = iris)
```
This, will make *rpart()* consider only the ```Sepal.Lenght``` and ```Sepal.Width``` attributes when building the tree.

The ```Species ~ .``` that was used in the formula argument before is equivalent to ```Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width```

For more information about formulas in R you can check this nice [tutorial](https://www.datacamp.com/community/tutorials/r-formula-tutorial) on datacamp.

## What lies underneath
That's nice but how do *rpart()* decides on the order of the nodes of the tree?

In order to make a "good" decision tree we have to choose the "seperation points" of each characteristic, in other words, the point
that seperates the branches of each node of the tree. In some cases, like the root of tree of the titanic example, that is simple as it is
just a yes/no question, but in other cases we have to somehow specify a point of seperation (9.5 and 2.5 in the example) except if we use fuzzy rules
(where we would have to specify these rules, but that is for another article).

Also, we have to choose the order of the nodes of the tree, the faster we reach a leaf the better, so we generally want to get the
nodes that divide the data well closer to the root, based on the results from the samples (training data) we have.

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

