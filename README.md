# League of Legends Strategic Resources Analysis

League of Legends Strategic Resources Analysis is ...

Author: Chengyuan Mao

## Introduction
### General Introduction
League of Legends (LoL) is a 2009 multiplayer online battle arena video game developed and published by Riot Games.

### Introduction of Colmuns
...

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
...

### Univariate Analysis
Distribution of Total Gold (graph)
The graph shows the distribution of total gold owned across a population or dataset. It is a histogram, where the x-axis represents the total amount of gold, and the y-axis shows the frequency or number of cases at each gold amount level.

The distribution appears to be roughly bell-shaped or normal, with the peak frequency occurring around 50,000-60,000 units of total gold. This suggests that most cases or individuals in the dataset have a moderate amount of gold around that central value.

However, there are also some cases with much lower or much higher gold amounts, as evidenced by the tails of the distribution extending to both ends of the x-axis.

Some key observations from the distribution:

1. It is unimodal, with a single prominent peak around 50,000-60,000 total gold.
2. The distribution is slightly skewed to the right, with a longer tail on the higher gold amount side.
3. The spread or variance is quite large, with gold amounts ranging from around 20,000 to over 100,000 units.
4. There are a few outliers or rare cases with extremely high gold amounts beyond 90,000 units.

This type of distribution pattern could arise in various contexts, such as wealth or resource ownership, where a majority of cases cluster around a moderate level, but there are also some cases with significantly more or less of the measured quantity.

### Bivariate Analysis

<iframe
  src="assets/percent_win_w_ft.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Securing the first turret in a match grants a significant strategic edge. Statistics show that teams accomplishing this objective claim victory around 64% of the time. This notably higher win rate underscores the pivotal role of obtaining an early turret advantage. Capturing this key strategic resource evidently sets the stage for subsequent dominance, ultimately translating to an increased likelihood of overall triumph.

### Interesting Aggregates
I first groupby the cleaned data set with firsttower status and then calculate the sum of all statistics.


## Assessment of Missingness
### NMAR Analysis
In my dataset, I believe the colums `a`

### Missingness Dependency
In this section, I will test whether the missingness of the `dragons (type unknown)` column depends on `firstbaron` and `result`. For both of my permutation tests, I will use a significance level of 0.5 and Total Variance Distance (TVD) as the test statistic.

- Null Hypothesis: The missingness of the `dragons (type unknown)` column is independent of the `firstbaron`.
- Alternative Hypothesis: The missingness of the `dragons (type unknown)` column depends on the `firstbaron`.

Below is the observed distribution of `firstbaron` when `dragons (type unknown)` is missing and not missing.



Since the p-value `0.0` is less than `0.5` significance level, we reject the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is depends on the `firstbaron` column.

#### The Second Permutation Test

- Null Hypothesis: The missingness of the `dragons (type unknown)` column is independent of `result`.
- Alternative Hypothesis: The missingness of the `dragons (type unknown)` column depends on `result`.

Below is the observed distribution of `result` when `dragons (type unknown)` is missing and not missing.


Since the p-value `1.0` is greater than `0.5` significance level, we fail to reject the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is not depends on the `result` column.

## Hypothesis Testing
* Null: The distribution of total gold for the team that gets the first turret is the same as the team that does not get the first turret. 
* Alternative: The distribution of total gold for the team that gets the first turret is NOT the same as the team that does not get the first turret. 
* Test Statistics: Absolute mean difference between total gold with and without first turret.
* Significance Level: 5%


Based on the hypothesis test performed, with a `p-value` of `0.0`, we reject the null hypothesis. This suggests that the distribution of total gold for the team that gets the first turret is NOT the same as the team that does not get the first turret.



## Framing a Prediction Problem
...


## Baseline Model
For the baseline model, I used a Random Forest Classifier, with the following features: 


## Final Model
...


## Fairness Analysis
...
