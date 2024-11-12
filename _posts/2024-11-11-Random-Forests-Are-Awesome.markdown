---
author: "Daniel Sherwood"
layout: post
title: "Random Forests are Awesome"
date: 2024-11-11 19:52:00 -0500
categories: python
lead: "Bootstrapping and Decision Trees"
---
**Random Forests are** a bread-and-butter machine learning algorithm that, despite being invented 23 years ago by Leo Breiman, 
are still incredibly powerful tools for analyzing data. And they are relatively simple. A decision tree is a binary split 
of a particular variable. A random forest is a collection of decision trees, but the set of data is subset to include a 
random number of columns/variables and a random number of rows/samples. The _number of trees_ corresponds to the number of 
subsets specified from the data, but the number of binary splits corresponds to the _depth_ of the tree. Interestingly, it
has been shown that it is impossible for a random forest to overfit by increasing the number of trees _<citation needed>_. 
The same is not true for the depth of the tree; increasing the tree depth can absolutely lead to overfitting. 

**So how do** we go about programming a random forest? We need a way to subset our data to consist of an arbitrary number of
variables and samples. We need a function to choose a variable, and then split on it, and then choose one of the remaining
variables, and split on it, etc. There needs to be an element of recursion. And, what criteria are we going to use to split
on anyway? 

**Well, it's pretty** easy to do all of this in Python. There's a very impressive man named Jeremy Howard, who created just
such a tutorial for us. It is located [here](https://www.kaggle.com/code/jhoward/how-random-forests-really-work/). Jeremy Howard
is a former president of Kaggle, a data science competition website. His tutorial clearly explains the basics of how a 
random forest works. I wanted to draw your attention to it because it is excellent, and you should read it. I will also 
represent his code below, slightly re-organized for my own use. 

# Define a Class

```python
class RandomForest:
    def __init__(self, filepath):
        self.filepath = filepath
        self.train_data = None
        self.test_data = None
        self.tree = None
        self.cols = None

    def load_data(self, train_file, test_file):
        self.train_data = pd.read_csv(filepath_or_buffer=self.filepath + '/' + train_file)
        self.test_data = pd.read_csv(filepath_or_buffer=self.filepath + '/' + test_file)
        self.cols = self.train_data.columns

    def process_data(self):
        self.train_data = proc_data(self.train_data)
        self.test_data = proc_data(self.test_data)

    def create_tree(self, cols):
        self.tree = {i : min_col(data_subset, i) for i in cols}
```

Here I've created a `RandomForest` class that 
1. Initializes some variables
2. Loads data into `pandas` dataframes attached to an object
3. Performs processing (JH's code)
4. Creates Trees (JH's code)

# The Static Methods 

**The first three** items are simple. But let's look at that last method, `create_tree`. 
It iterates through a dictionary, in turn calling `min_col` on a subset of data. Here's it's definition:

```python
# JH's
def min_col(df, nm):
    col, y = df[nm], df[dep]
    unq = col.dropna().unique()
    scores = np.array([score(col, y, o) for o in unq if not np.isnan(o)])
    idx = scores.argmin()
    return unq[idx], scores[idx]
```

`min_col` in turn takes in a dataframe and calls the `score` method for every column in the subset presented. 
The `[ for o in unq if not np.isnan(o)]` is a list comprehension. It is exactly what it looks like, a for loop inside of a 
list. This function loops through the columns, scoring each column compared to the target/dependent variable and 
delivers the index of that subset that corresponds to the minimal score (best split).

Here's the score method:

```python
# JH's 
def score(col, y, split):
    lhs = col <= split
    return (_side_score(lhs, y) + _side_score(~lhs, y))/len(y)
```

and here's the score helper method:

```python
def _side_score(side, y):
    tot = side.sum()
    if tot <= 1:
        return 0
    return y[side].std() * tot
```

finally, the static method to process the dataframe:

```python
def proc_data(df):
    df['Fare'] = df.Fare.fillna(0)
    df.fillna(df.mode().iloc[0], inplace=True)
    df['LogFare'] = np.log1p(df['Fare'])
    df['Embarked'] = pd.Categorical(df.Embarked)
    df['Sex'] = pd.Categorical(df.Sex)
    return df
```

In case you've never used `pandas`, what JH is doing here is transforming certain columns (denoted by the `[` symbol) 
within a dataframe. This function returns the dataframe thus transformed.
