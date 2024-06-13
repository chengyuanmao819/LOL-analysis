# League of Legends Strategic Resources Analysis

League of Legends Strategic Resources Analysis is an extensive data science project conducted at UCSD. This project involves several stages, including exploratory data analysis, hypothesis testing, development of baseline models, and a final fairness analysis. The main objective is to examine the importance of the "first turret" and "first mid-lane turret" events in League of Legends matches, and to assess their impact on match statistics and outcomes.

By Chengyuan Mao

# Introduction
## General Introduction
<a href="https://en.wikipedia.org/wiki/League_of_Legends">League of Legends (LoL)</a>, developed and published by <a href="https://en.wikipedia.org/wiki/Riot_Games">Riot Games, Inc.</a> in 2009, is a multiplayer online battle arena video game that has gained immense popularity worldwide. With its vast player base, it has emerged as one of the most influential and widely played esports titles in the gaming industry. The dataset we will be working with is a professional dataset curated by Oracle's Elixir, which records match data from professional LoL esports gaming events throughout the year 2023.

## Question Identification
In a game of League of Legends, 10 players are divided into two teams to compete against each other. Each player has their own role and responsibilities, but the ultimate goal is to secure as many strategic resources as possible and eventually destroy the enemy's <a href="https://leagueoflegends.fandom.com/wiki/Nexus">Nexus</a> located in their base. On the League of Legends map, <a href="https://leagueoflegends.fandom.com/wiki/Turret">turrets</a> (tower) are considered one of the most important strategic resources. Taking down an opponent's turret not only grants each player on the team additional gold but also provides control over neutral resources and vision around the turret area. More importantly, if a team manages to take the first (mid lane) turret in a game, it signifies that they have gained an early advantage, which can have a profound impact on the team's economy and overall game situation.

In this project, I aim to study the extent of the economic and situational advantages that securing the first (mid lane) turret brings to the team, and how these advantages influence the resource contention and the outcome of the game. Through this research, I hope to quantify the strategic value of the first mid lane turret and provide concrete data to help players and teams better understand and utilize this key resource, thereby enhancing the overall competitive level and tactical depth of the game.

## Introduction of Colmuns
In this dataset, important features of each match in the 2023 professional League of Legends games are recorded, containing 148,992 rows, with each match looping in 12 rows. The first ten rows of each loop represent the ten players of the two teams, and the last two rows represent the aggregated data of the two teams.

Below is a brief description of the columns related to my project and their data types:

|Column Name                  |Type    |Description|
|---                          |---     |---|
|`'result'`                   |int     |Outcome of the match for the team (e.g., 1 for win, 0 for loss).|
|`'firsttower'`               |float     |Indicator of whether the team destroyed the first tower in the game (e.g., 1 for yes, 0 for no).|
|`'firstmidtower'`            |float     |Indicator of whether the team destroyed the first middle lane tower (e.g., 1 for yes, 0 for no).|
|`'heralds'`                  |float     |Number of <a href="https://leagueoflegends.fandom.com/wiki/Rift_Herald/LoL">Rift Heralds</a> the team secured during the game.|
|`'firstherald'`              |float     |Indicator of whether the team secured the first Rift Herald (e.g., 1 for yes, 0 for no).|
|`'totalgold'`                |int     |Total gold earned by the team by the end of the game.|
|`'goldat10'`                 |float     |Total gold earned by the team at the 10-minute mark.|
|`'killsat10'`                |float     |Total number of kills achieved by the team at the 10-minute mark.|
|`'golddiffat10'`             |float     |Difference in gold between the team and the opposing team at the 10-minute mark.|
|`'goldat15'`                 |float     |Total gold earned by the team at the 15-minute mark.|
|`'killsat15'`                |float     |Total number of kills achieved by the team at the 15-minute mark.|
|`'golddiffat15'`             |float     |Difference in gold between the team and the opposing team at the 15-minute mark.|
|`'firstblood'`               |float     |Indicator of whether the team achieved the first kill (first blood) in the game (e.g., 1 for yes, 0 for no).|
|`'firstdragon'`              |float     |Indicator of whether the team secured the first dragon in the game (e.g., 1 for yes, 0 for no).|
|`'firstbaron'`               |float     |Indicator of whether the team secured the first <a href="https://leagueoflegends.fandom.com/wiki/Baron_Nashor/LoL">baron</a> in the game (e.g., 1 for yes, 0 for no).|
|`'turretplates'`             |float     |Total number of <a href="https://leagueoflegends.fandom.com/wiki/Turret#General">turret plates</a> destroyed by the team during the laning phase.|
|`'dragons (type unknown)'`   |float     |Total number of dragons of unspecified type secured by the team during the game.|

# Data Cleaning and Exploratory Data Analysis
## Data Cleaning
To save time in the further data cleaning steps, I first only keep the relevant columns: `result`, `firsttower`, `firstmidtower`, `heralds`, `firstherald`, `totalgold`, `goldat10`, `killsat10`, `golddiffat10`, `goldat15`, `killsat15`, `golddiffat15`, `firstblood`, `firstdragon`, `firstbaron`, `turretplates`, and `dragons(type unknown)`. From the last section, **"Introduction of Columns"**, I found out that a lot of columns' data types should be `bool`, but they are actually `float` or `int`, e.g., `result`, `firsttower`, `firstherald`, etc. After determining which rows should change data types, I convert all of them to the `bool` type.

Below is the head of my cleaned `lol` dataframe:

|    | result   | firsttower   | firstmidtower   |   heralds | firstherald   |   totalgold |   goldat10 |   killsat10 |   golddiffat10 |   goldat15 |   killsat15 |   golddiffat15 | firstblood   | firstdragon   |   firstbaron |   turretplates |   dragons (type unknown) |
|---:|:---------|:-------------|:----------------|----------:|:--------------|------------:|-----------:|------------:|---------------:|-----------:|------------:|---------------:|:-------------|:--------------|-------------:|---------------:|-------------------------:|
|  0 | True     | True         | True            |         2 | True          |       72807 |      14612 |           0 |             75 |      22384 |           0 |           -530 | False        | False         |            1 |              4 |                      nan |
|  1 | False    | False        | False           |         0 | False         |       62745 |      14537 |           0 |            -75 |      22914 |           1 |            530 | True         | True          |            0 |              2 |                      nan |
|  2 | False    | False        | True            |         2 | True          |       80627 |      15969 |           2 |           -361 |      24771 |           4 |            673 | False        | False         |            1 |              6 |                      nan |
|  3 | True     | True         | False           |         0 | False         |       77449 |      16330 |           2 |            361 |      24098 |           3 |           -673 | True         | True          |            0 |              2 |                      nan |
|  4 | True     | False        | True            |         0 | False         |       60938 |      14794 |           1 |          -1001 |      22945 |           2 |          -1901 | False        | False         |            0 |              3 |                      nan |

## Univariate Analysis
I plot a graph for the distribution of total gold using a histogram:

<iframe
  src="assets/total_gold.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The graph shows the distribution of total gold owned across the lol dataset. It is a histogram, where the x-axis represents the total amount of gold, and the y-axis shows the frequency or number of cases at each gold amount level.

The distribution appears to be roughly bell-shaped or normal, with the peak frequency occurring around 50,000-60,000 units of total gold. This suggests that most cases or individuals in the dataset have a moderate amount of gold around that central value.

However, there are also some cases with much lower or much higher gold amounts, as evidenced by the tails of the distribution extending to both ends of the x-axis.

Some key observations from the distribution:

1. It is unimodal, with a single prominent peak around 50,000-60,000 total gold.
2. The distribution is slightly skewed to the right, with a longer tail on the higher gold amount side.
3. The spread or variance is quite large, with gold amounts ranging from around 20,000 to over 100,000 units.
4. There are a few outliers or rare cases with extremely high gold amounts beyond 90,000 units.

This type of distribution pattern could arise in various contexts, such as wealth or resource ownership, where a majority of cases cluster around a moderate level, but there are also some cases with significantly more or less of the measured quantity.


And an overlapping graph for the total gold distribution by the first turret status:

<iframe
  src="assets/total_gold_overlap.html"
  width="800"
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

## Bivariate Analysis
I performed bivariate analysis on the 'first turret' and 'result' statistics in the dataset to visualize the impact of obtaining the first tower on the game's outcome.

<iframe
  src="assets/percent_win_w_ft.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Securing the first turret in a match grants a significant strategic edge. Statistics show that teams accomplishing this objective claim victory around 64% of the time. This notably higher win rate underscores the pivotal role of obtaining an early turret advantage. Capturing this key strategic resource evidently sets the stage for subsequent dominance, ultimately translating to an increased likelihood of overall triumph.

## Interesting Aggregates
Here are some compelling aggregates to consider for investment within the dataset:

| firsttower   |   result |   firstmidtower |   heralds |   firstherald |   totalgold |    goldat10 |   killsat10 |   golddiffat10 |    goldat15 |   killsat15 |   golddiffat15 |   firstblood |   firstdragon |   firstbaron |   turretplates |   dragons (type unknown) |
|:-------------|---------:|----------------:|----------:|--------------:|------------:|------------:|------------:|---------------:|------------:|------------:|---------------:|-------------:|--------------:|-------------:|---------------:|-------------------------:|
| False        |     2663 |            2396 |      6204 |          2974 | 4.78621e+08 | 1.35291e+08 |       15404 |   -6.74411e+06 | 2.10885e+08 |       29248 |   -1.66055e+07 |         3625 |          3786 |         2785 |          27572 |                        0 |
| True         |     7828 |            9762 |     11313 |          9180 | 7.12551e+08 | 1.42036e+08 |       22070 |    6.74411e+06 | 2.2749e+08  |       41589 |    1.66055e+07 |         6847 |          8368 |         5752 |          50590 |                     7106 |

I first groupby the cleaned data set with firsttower status and then calculate the sum of all statistics. By comparing the gaming statistics with and without first turret. From the results, the data for the team that secures the first turret is overall higher than that of the team that fails to secure the first turret.

# Assessment of Missingness
## NMAR Analysis
In my dataset, I believe the colums `a`

## Missingness Dependency
In this section, I will test whether the missingness of the `dragons (type unknown)` column depends on `firstbaron` and `result`. For both of my permutation tests, I will use a significance level of 0.5 and Total Variance Distance (TVD) as the test statistic.

### The First Permutation Test
**Null Hypothesis**: The missingness of the `dragons (type unknown)` column is **independent** of the `firstbaron`.

**Alternative Hypothesis**: The missingness of the `dragons (type unknown)` column **depends** on the `firstbaron`.

Below is the observed distribution of `firstbaron` when `dragons (type unknown)` is missing and not missing.

<iframe
  src="assets/dist_TVD1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value `0.0` is less than `0.5` significance level, we reject the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is depends on the `firstbaron` column.

### The Second Permutation Test
**Null Hypothesis**: The missingness of the `dragons (type unknown)` column is **independent** of `result`.

**Alternative Hypothesis**: The missingness of the `dragons (type unknown)` column **depends** on `result`.

Below is the observed distribution of `result` when `dragons (type unknown)` is missing and not missing.

<iframe
  src="assets/dist_TVD2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value `1.0` is greater than `0.5` significance level, we **fail to reject** the null hypothesis. Therefore, the missingness of `dragons (type unknown)` is **NOT** depends on the `result` column.

# Hypothesis Testing
**Null Hypothesis**: The distribution of total gold for the team that gets the first turret is the same as the team that does not get the first turret. 

**Alternative Hypothesis**: The distribution of total gold for the team that gets the first turret is **NOT** the same as the team that does not get the first turret. 

**Test Statistics**: Absolute mean difference between total gold with and without first turret.

**Significance Level**: 5%

<iframe
  src="assets/permu_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test performed, with a `p-value` of `0.0`, we **reject** the null hypothesis. This suggests that the distribution of total gold for the team that gets the first turret is **NOT** the same as the team that does not get the first turret.

# Framing a Prediction Problem
## Problem Identification
From the previous section, I observed that securing the first turret in a League of Legends match can have a significant impact on a team's economy, which in turn influences the outcome of the game. This indicates that the first turret is an important strategic resource in the game.

However, the first mid-lane turret is even more crucial because the mid-lane can influence both the top and bottom lanes. Additionally, the team that secures the first mid-lane turret will have a significant control advantage over other important map resources, such as jungle monsters, dragons, and key vision areas. So, what factors influence whether a team can secure the first mid-lane turret? In other words, can I predict whether a team will secure the first mid-lane turret in a match by analyzing their game statistics (e.g., first blood kills, turret plates, gold difference, and other related features)?

To address this question, I can use machine learning techniques, such as **classification algorithms**. Therefore, I establish my model based on the following **prediction problem**: Can I predict whether a team will secure the first mid-lane turret in a match based on their other game statistics? By analyzing and modeling these data, I aim to identify the key factors that influence this critical event, thereby helping teams develop more effective strategies and tactics to improve their chances of winning.

To prevent overfitting, I will split the data into two parts: 75% for training and 25% for testing. For model evaluation, I will use both accuracy and F1-score.

For each team, I will use the following features for prediction: `heralds`, `firstblood`, `turretplates`, `golddiffat10`, and `firsttower`. These statistics are collected during the game and will be used to train my model.


# Baseline Model
For the baseline model, I used a Random Forest Classifier, with the following features: `heralds`, `firstblood`, `turretplates`, `golddiffat10`, and `firsttower`. Among these five features, three of them are quantitative: `heralds`, `turretplates`, and `golddiffat10`. I utilized Stan

After fitting the model, my accuracy score is **0.9620** on the train data and **0.7220** on the test data with a **0.7671** F-1 score.


# Final Model
In my final model, I added ...

After fitting the model, my accuracy score is **0.8400** on the test data with a **0.8627** F-1 score.


# Fairness Analysis
In this section, I aim to evaluate the fairness of my model across different groups. The central question is:  **"Does my model perform worse for teams who have ... or not have ...?"** 

To address this query, I conducted a permutation test to examine the difference in accuracy between two groups:

Group `X` represents the teams who have...
Group `Y` represents those teams who do not have ... 
The evaluation metric employed is accuracy, and the significance level is set at 0.5.

By performing the permutation test and analyzing the accuracy differential, I endeavor to ascertain whether my model exhibits biased performance favoring one group over the other. This analysis is crucial for assessing the model's fairness and identifying potential areas for improvement to mitigate any undesirable disparities.

The followings are my hypothesis:

**Null Hypothesis**: My model is fair. ...

**Alternative Hypothesis**: My model is not fair. ...

**Test Statistics:**: ...

After performing the permutation test, the result p-value I got is `1`, which is greater than the 0.05 significance level. Consequently, we ... reject the null hypothesis. This outcome implies that my model predicts teams from both groups with statistically similar accuracy levels. Consequently, my model appears to be fair, exhibiting no discernble bias towords one group over the other based on the specified criteria. (need rewrite)
