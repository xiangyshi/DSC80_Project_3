# DSC80 FA23 Project 3
This repository serves the primary purpose of hosting a Jekyll GitHub Page for the project 3 report from DSC80_FA23.

Names: \
Leo Shi A17033789 \
Eric Gu A16817621


 
Question: Is the champion Rell more likely to have a gold lead at 15 minutes as a Jungler than a Support given any match in the pro series?

# Introduction
Our dataset is chosen from all the [League of Legends Professional games in 2023](https://oracleselixir.com/tools/downloads). The dataset consists 129480 rows and 123 columns in total. Each match consists of 12 rows with 10 players' personal statistics in the first 10 rows and the team statistics in the last 2 rows. Since the champion Rell has become more and more popular in professional games as both a Jungler and a Support (two positions in a game of Leage of Legends), we would like to know more about the performance of Rell in professional games as the two roles, especially in the first 15 minutes of a game, which is usually a crucial part of the game for experience and gold gaining. Thus, we decided to propose the question “Is the champion Rell more likely to have a gold lead at 15 minutes as a Jungler than a Support?”. Through this experiment, the League of Legends pro-game audience can understand which role Rell is likely to perform better at in any given pro game.

To answer the question “Is the champion Rell more likely to have a gold lead at 15 minutes as a Jungler than a Support?”, we decided to perform data cleaning of the original dataset and form a new dataset that better answers the question. Among all the 2074 matches containing the champion Rell, only 1854 have valid(non NaN) data in the `golddiffat15` column, which we would like to consider. Thus, the new dataset consists of 1854 rows and 3 columns. The three columns are `position`, `golddiffat15`, and `leading`. We chose these three columns because it shows the position of Rell used by the player, the gold difference between Rell and its opponent at 15 minutes of a game, and whether they are leading or not. The gold difference between Rell and its opponent is what defines whether the Rell player performs better than its opponent. Each row represents each individual Rell match statistics. 

### Description of Columns
“position” is ‘jng’ when Rell is played as a Jungler and ‘sup’ when Rell is played as a Support.
`golddiffat15` is the amount of gold difference between the player who plays Rell and the opponent at the 15 minutes of a game.
`leading` is `Leading` when the Rell player has positive `golddiffat15` value and `Behind` when the Rell player has non positive `golddiffat15` value. This was created after `golddiffat15` was collected.

# Cleaning and Exploratory Data Analysis
### Data Cleaning 
In order to solve our question, we decided to keep relevant columns that helps answer the question: `position` and `golddiffat15`, and the relevant rows that only consist of all match data related to the Champion Rell, which the value in the champion column is equal to ‘Rell’. This makes each row represent a single Rell match without repetition (in a game of League of Legends, all playing champions must be unique). After getting all match information related to Rell, we notice that there are some empty values in `golddiffat15` that are not recorded in the original dataset. Since we can’t know the gold difference from the missing data, we decide to clean up and remove all empty `golddiffat15` data and keep the rest meaningful data, which makes the row number change from 2047 to 1854. In order to help us faciliate data managing and understand more directly whether the Rell match has a gold lead or not, we add another column named `leading`, which has value `Leading` when the Rell player has positive `golddiffat15` value and `Behind` when the Rell player has non-positive `golddiffat15` value. 

Here are the head of the cleaned DataFrame:

|    | position   |   golddiffat15 | leading   |
|---:|:-----------|---------------:|:----------|
|  0 | sup        |           -848 | Behind    |
|  1 | sup        |           -387 | Behind    |
|  2 | sup        |           -654 | Behind    |
|  3 | sup        |            912 | Leading   |
|  4 | sup        |            601 | Leading   |

### Univariate Analysis

<iframe src="asset/figuni.html" width=800 height=600 frameBorder=0></iframe>

This is a pie chart showing the total leading proportion of all the Rell in 2023 Professional matches. 50.4% of the Rell matches have a gold lead in the first 15 minutes, and 49.6% of the Rell matches don’t have a gold lead in the first 15 minutes. We can notice that Rell almost has a tie gold lead proportion at 15 minutes of a game.

### Bivariate Analysis

<iframe src="asset/figbi.html" width=800 height=600 frameBorder=0></iframe>

This is a bar chart showing the gold lead average by position of all Rell in 2023 Professional matches. We can notice that Rell plays as a Support has a higher gold difference toward its opponent than as a Jungler at the 15 minutes of a game. Rell plays as Support tends to have a positive gold difference toward its opponent at the 15 minutes of a game and a negative gold difference toward its opponent at the 15 minutes of a game as a Jungler.

### Interesting Aggregates

| position   |   Furthest Behind |
|:-----------|------------------:|
| jng        |             -2310 |
| sup        |             -3227 |

This `groupby` shows the most gold difference that the Rell player is behind towards its opponent (of the corresponding position) at the 15 minutes mark for both position. It is interesting to see that the Support Rell in one match is behind by 3227 gold, **which is a lot!!!**


# Assessment of Missingness

### NMAR Analysis

We believe that there are indeed columns that are NMAR in the csv. The column `ban5` are sometimes missing due to the suspected reason that the team decided not to ban a 5th champion from the playing pool that game, and thus resulting in a blank cell, this does not depend on any other column as this decision is purely by the choice of the team/coach. Thus we would need another column indicating the number of total bans, say `totalban`, for each game/team to have the `ban5` dependent on another column and make it MAR.


### Missing Dependency

The missingness of `firstbaron` does depend on `split` We wanted to determine if `firstbaron` and `split` were Missing at Random or Missing Completely at Random.

Here is the observed statistics when `firstbaron` is missing:

| split       | firstbaron |
|:------------|-----------:|
| BLX Masters | 0.0159876  |
| Champ 1     | 0.0145647  |
| Champ 2     | 0.0153745  |
| Closing     | 0.0111751  |
| Finals      | 0.00373661 |
| Opening     | 0.0115222  |
| Placements  | 0.00779714 |
| Split 1     | 0.0755536  |
| Split 2     | 0.0835936  |
| Split 3     | 0.0311538  |
| Spring      | 0.355811   |
| Summer      | 0.354029   |
| Winter      | 0.0197011  |


Here is the observed statistics when `firstbaron` is not missing:

| split       | firstbaron |
|:------------|-----------:|
| BLX Masters | 0.0166317  |
| Champ 1     | 0.0160028  |
| Champ 2     | 0.0144654  |
| Closing     | 0.0121593  |
| Finals      | 0.00426275 |
| Opening     | 0.01174    |
| Placements  | 0.00740741 |
| Split 1     | 0.0777778  |
| Split 2     | 0.0853948  |
| Split 3     | 0.0315164  |
| Spring      | 0.358001   |
| Summer      | 0.346122   |
| Winter      | 0.0185185  |

Our observed statistic was: 0.06190388210644695


Our p-value was: 0.0
Here is the empirical distribution of the test statistic:

<iframe src="asset/fig_depend.html" width=800 height=600 frameBorder=0></iframe>


The missingness of `firstbaron` does depend on `teamname` . We wanted to determine if `teamname` and `firstbaron` were Missing at Random or Missing Completely at Random.

Here is the observed statistics when `firstbaron` is missing:
| side   | firstbaron |
|:-------|----------:|
| Blue   |  0.499145 |
| Red    |  0.500855 |

Here is the observed statistics when `firstbaron` is not missing:

| side   | firstbaron |
|:-------|----------:|
| Blue   |  0.505182 |
| Red    |  0.494818 |

Our observed statistic was: 0.0

Our p-value was: 0.996

Here is the empirical distribution of the test statistic:

<iframe src="asset/fig_ndepend.html" width=800 height=600 frameBorder=0></iframe>



# Hypothesis Testing

### Null hypothesis
The distribution of `golddiffat15` of the champion Rell as a Jungler is the same as the distribution of `golddiffat15` of the champion Rell as a Support, and the observed differences in our samples **are due to random chance.**

### Alternative hypothesis
The distribution of `golddiffat15` of the champion Rell as a Jungler is less than the distribution of `golddiffat15` of the champion Rell as a Support, the observed difference in our samples **cannot be explained by random chance alone.**

### Test Statistics
We will be using permutation test to calculate differences in group mean after shuffling.
We choose to perform a permutation test with differences in group mean because we can understand how different the two numerical distributions are. 


Significance Level: 0.05 because it is commonly used.

P-value:0.0, performing 1000 simulations.

<iframe src="asset/hypo_fig.html" width=800 height=600 frameBorder=0></iframe>


### Conclusion
**We reject the null hypothesis**, which means that we believe that the distribution of gold lead at 15 minutes of the champion Rell as a Jungler is likely not the same as the distribution of gold lead at 15 minutes of the champion Rell as a Support, and the observed differences in our samples **are not due to random chance.**

We believe that Rell playing as Jungler and Support will have a different gold lead at 15 minutes, and specifically Support would likely have a larger gold lead than Jungler. There are other reasons that can explain the large gold differences between the two position plays. There may be other confounding factors.