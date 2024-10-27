---
layout: default
title: "Calculating the Brier Score of an Election Forecast"
date: 2024-10-16 08:15:00 -0500
categories: python
lead: "Have you ever wanted to evaluate your favorite election forecast?"
---
Another election cycle is upon us. On the one hand, I feel compelled to check _fivethirtyeight.com_ (headed by G Elliot Morris)
or _The Silver Bulletin_ (headed by Nate Silver) every day to see how the percentage of winning has changed for each candidate. And then I see that usually, the estimation 
for both candidates has moved by a few tenths of a point. _How disappointing_. But I _also_ noticed that Morris and Silver
_differ_ in their predictions. Wouldn't it be neat to evaluate their forecasts before the election even occurs?

Well, it turns out we can! By calculating a `Brier Score` for both forecasts, for both candidates, we can see which has more
of a bias for each candidate. Here's how to do it in Python. 

## 1. Download Forecast
Nate Silver's _Bulletin_ is nice in that it provides a button to facilitate download. Unfortunately, _fivethirtyeight.com_ does not do that
so you will have to get their data manually. 

## 2. Import Libraries & Load Data
Only a few libraries are needed: `pandas` to load data into a data frame and the `brier_score_loss` function from `scikit-learn`. 

```python
import numpy as np
import pandas as pd
from sklearn.metrics import brier_score_loss

# load nate silver's data from .csv
ns = pd.read_csv(filepath_or_buffer="./NateSilver3Sep2024.csv")
ns = ns.dropna(axis=0)

# load morris' data from .csv
m = pd.read_csv(filepath_or_buffer="./Morris21Aug2024.csv")
m = m.dropna(axis=0)
```

## 3. Calculate Brier Score
Since we have two forecasters and two presidential candidates, we need to generate four Brier Scores. 
Let's make a function and run it. 

```python
# create function for brier score
def calc_brier_score(y):
    y_true = np.ones(len(y))
    y_proba = y / 100.
    bsl = brier_score_loss(y_true=y_true, y_proba=y_proba)
    return bsl

# calculate brier_score for silver
silver_trump_bsl = calc_brier_score(ns["trump"].to_numpy())
silver_harris_bsl = calc_brier_score(ns["harris"].to_numpy())
# calculate brier_score for morris
morris_trump_bsl = calc_brier_score(m["Trump"].to_numpy())
morris_harris_bsl = calc_brier_score(m["Harris"].to_numpy())
```

## 4. Output!

The output is this: 

|        | Trump Wins | Harris Wins |
|:-------|:-----------|:------------|
| Silver | 0.310      | 0.309       |
| Morris | 0.312      | 0.292       |

The smaller the score, the better. Essentially, this table shows that Nate Silver's forecast is better that Morris' 
forecast if former President Trump is re-elected. It also suggests that Morris' forecast is slightly better if Vice-President Harris 
wins. A closer look shows that Silver's scores are almost identical for each candidate, while Morris' are slightly more 
unbalanced. This is interesting to me because Morris essentially took over Nate Silver's job at fivethirtyeight.com. 
They've tangled a bit in the past, so you have to wonder if their criticisms of each other's modeling approaches are 
valid or just enmity.  We'll find out soon, in a month!

## 5. Testing

$$ y = x^2 $$

