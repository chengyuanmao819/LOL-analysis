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
I plot a graph for the distribution of total gold using a histogram.

<iframe
  src="assets/total_gold.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

The graph shows the distribution of total gold owned across a population or dataset. It is a histogram, where the x-axis represents the total amount of gold, and the y-axis shows the frequency or number of cases at each gold amount level.

The distribution appears to be roughly bell-shaped or normal, with the peak frequency occurring around 50,000-60,000 units of total gold. This suggests that most cases or individuals in the dataset have a moderate amount of gold around that central value.

However, there are also some cases with much lower or much higher gold amounts, as evidenced by the tails of the distribution extending to both ends of the x-axis.

Some key observations from the distribution:

1. It is unimodal, with a single prominent peak around 50,000-60,000 total gold.
2. The distribution is slightly skewed to the right, with a longer tail on the higher gold amount side.
3. The spread or variance is quite large, with gold amounts ranging from around 20,000 to over 100,000 units.
4. There are a few outliers or rare cases with extremely high gold amounts beyond 90,000 units.

This type of distribution pattern could arise in various contexts, such as wealth or resource ownership, where a majority of cases cluster around a moderate level, but there are also some cases with significantly more or less of the measured quantity.


And an overlapping graph for the total gold distribution by the first turret status.

<iframe
  src="assets/total_gold_overlap.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

This graph shows an overlapping histogram of the distribution of total gold, split by a categorical variable called "First Tower Status" which has two levels: True and False.

The blue bars represent the distribution for cases where First Tower Status is True, while the orange bars represent the distribution for cases where First Tower Status is False.

A few observations can be made:

1. Both distributions are roughly bell-shaped or normal, with a peak around 50,000-60,000 total gold.

2. However, the distribution for First Tower Status = True (blue) has a higher peak and is slightly shifted towards the right compared to the False distribution (orange). This suggests that cases with First Tower Status = True tend to have somewhat higher total gold amounts on average.

3. The spread or variance of the True distribution also appears slightly wider than the False distribution, indicating more variability in total gold amounts when First Tower Status is True.

Overall, this visualization allows us to compare the total gold distributions for the two levels of the First Tower Status variable. While the shapes are broadly similar, there are discernible differences in the central tendencies, spreads, and tail behaviors that could point to an association between First Tower Status and total gold ownership patterns.

### Bivariate Analysis
I performed bivariate analysis on the 'first turret' and 'result' statistics in the dataset to visualize the impact of obtaining the first tower on the game's outcome.

<iframe
  src="assets/percent_win_w_ft.html"
  width="900"
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

#### The First Permutation Test
- Null Hypothesis: The missingness of the `dragons (type unknown)` column is independent of the `firstbaron`.
- Alternative Hypothesis: The missingness of the `dragons (type unknown)` column depends on the `firstbaron`.

Below is the observed distribution of `firstbaron` when `dragons (type unknown)` is missing and not missing.

<iframe
  src="assets/dist_TVD1.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

Since the p-value `0.0` is less than `0.5` significance level, we reject the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is depends on the `firstbaron` column.

#### The Second Permutation Test

- Null Hypothesis: The missingness of the `dragons (type unknown)` column is independent of `result`.
- Alternative Hypothesis: The missingness of the `dragons (type unknown)` column depends on `result`.

Below is the observed distribution of `result` when `dragons (type unknown)` is missing and not missing.

<iframe
  src="assets/dist_TVD2.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

Since the p-value `1.0` is greater than `0.5` significance level, we fail to reject the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is not depends on the `result` column.

## Hypothesis Testing
* Null: The distribution of total gold for the team that gets the first turret is the same as the team that does not get the first turret. 
* Alternative: The distribution of total gold for the team that gets the first turret is NOT the same as the team that does not get the first turret. 
* Test Statistics: Absolute mean difference between total gold with and without first turret.
* Significance Level: 5%

<iframe
  src="assets/permu_dist.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test performed, with a `p-value` of `0.0`, we reject the null hypothesis. This suggests that the distribution of total gold for the team that gets the first turret is NOT the same as the team that does not get the first turret.

## Framing a Prediction Problem
### Problem Identification
...


## Baseline Model
For the baseline model, I used a Random Forest Classifier, with the following features: 


## Final Model
...


## Fairness Analysis
...
