+++
title = "Decision trees in R"

date = 2018-10-14T18:58:00
lastmod = 2018-10-15T01:52:00
draft = false

authors = ["nicktheway"]

tags = ["programming", "data science", "R"]
summary = "."

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