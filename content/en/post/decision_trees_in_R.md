+++
title = "Decision trees in R"

date = 2018-10-14T18:58:00
lastmod = 2018-10-15T01:52:00
draft = true

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
To create decision trees in R we will use the *rpart* library and the *rpart.plot* library for visualizing them. 
To import both of them into the workspace run: 
```r
library(rpart)
## Uncomment to install the rpart.plot package in case you don't have it.
# install.packages("rpart.plot") 
library(rpart.plot)
```

In order to make a "good" decision tree we have to choose the "seperation points" of each characteristics, in other words, the point
that seperates the branches of each node of the tree. In some cases, like the root of tree of the example, that is simple as it is
just a yes/no question, but in other cases we have to somehow specify a point (9.5 and 2.5 in the example) except if we use fuzzy rules
(but that is for another article).

Also, we have to choose the order of the nodes of the tree, the faster we reach a leaf the better, so we generally want to get the
nodes that divide the data well higher based on the results from the samples we already have.

### The GINI index
A way to choose the order is the gini index. We assign an index on each characteristic and choose the one with the lower.

The GINI type for each splitted into $k$ groups characteristic ($c$) is the following:
$$\mathit{GINI}\_c = \sum_{i=1}^{k}\frac{n_i}{n}\mathit{GINI}\_c(i)$$
where:
$$n_i = \text{the number of elements in group } i \\\n = \text{the number of elements in all } k \text{ groups}$$
and for each group $i$ $c$'s values can be into:

$$\mathit{GINI}\_c(i) = 1 - \sum_{j}p(\mathit{outcome}\in{j}|i)^2$$
where $j$ traverses the groups $\mathit{outcome}$'s values can take.