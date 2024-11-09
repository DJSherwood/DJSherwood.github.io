---
author: "Daniel Sherwood"
layout: post
title: "Calculating the Brier Score of an Election Forecast"
date: 2024-10-16 08:15:00 -0500
categories: python
lead: "Have you ever wanted to evaluate your favorite election forecast?"
---
**Another election cycle** is upon us, and with it, prognositications from our favorite forecasters on who will win the Presidential Election.
My personal favorite is **Nate Silver**, whose 2015 book _The Signal and the Noise_ influenced me greatly as I was learning statistics. 
Nate now hosts his election forecasts on his substack _The Silver Bulletin_. 
On the other hand, I am interested in G Elliot Morris, who took over fivethirtyeight.com after Nate's contract failed to renew with Disney. 
Morris is newer to the game but has a decent track record; I'm not prepared to write him off just yet. 
Although both forecasters predict a Harris/Walz win, the magnitude of their predictions differ slightly. Which raises the question: could we evaluate my two favorite forecasters
before the election even takes place?

**It turns out** that we can! All we need to do is calculate the `Brier Score` for each candidate for both forecasts. I will not replicate the 
[wikipedia entry](https://en.wikipedia.org/wiki/Brier_score), but I will say briefly that the score ranges between 1 and 0. 
A score of 1 means that the forecast is 100% in the wrong direction, while a score of 0 means the forecast is 100% correct.
The `Brier Score` is calculated by the following formula:

$BS = \frac{1}{N} \sum_{t=1}^{N} \sum_{i=1}^{R} (f_{ti} - o_{ti})^2$

Here's how to do it in Python:

## 1. Download Forecast
Both fivethirtyeight.com and Nate Silver's _Bulletin_ provide a data download. It's relatively easy to work Silver's data, but fivethirtyeight's data
requires some extra selection/transformation.

## 2. Import Libraries & Load Data
Only a few libraries are needed: `pandas` to load data into a data frame and the `brier_score_loss` function from `scikit-learn`. 

```python
import numpy as np
import pandas as pd
from sklearn.metrics import brier_score_loss

# load nate silver's data from .csv
ns = pd.read_csv(filepath_or_buffer="./SilverFinal.csv")
ns = ns.dropna(axis=0)

# load morris' data from .csv
m = pd.read_csv(filepath_or_buffer="./MorrisFinal.csv")
m = m[ (m["state"] == "National") & (m["cycle"] == 2024) ][['candidate','pct_estimate']]
m_trump =  m[ m['candidate'] == "Trump" ][['pct_estimate']].values
m_harris = m[ m['candidate'] == "Harris" ][['pct_estimate']].values

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
silver_trump_bsl = calc_brier_score(ns["trump"].values)
silver_harris_bsl = calc_brier_score(ns["harris"].values)
# calculate brier_score for morris
morris_trump_bsl = calc_brier_score(m_trump)
morris_harris_bsl = calc_brier_score(m_harris)
```

## 4. Output!
The output is this: 

|        | Trump Wins | Harris Wins |
|:-------|:-----------|:------------|
| Silver | 0.31       | 0.31        |
| Morris | 0.32       | 0.28        |

**Essentially, this table** shows that Nate Silver's forecast is better than Morris' forecast if former President Trump is re-elected. 
It also suggests that Morris' forecast is slightly better if Vice-President Harris wins. 
A closer look shows that Silver's scores are identical for each candidate, while Morris' are slightly more 
unbalanced. This indicates to me that Silver's model may indeed be better than Morris' model, as it _seems_ to be better calibrated. 
Silver and Morris have tangled a bit in the past, so you have to wonder if their criticisms of each other's modeling approaches are 
valid or just enmity.  We'll find out soon, in a month!


