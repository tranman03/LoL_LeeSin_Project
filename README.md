# How Important is Early Gold on Lee Sin
by Adam Tran (ant010@ucsd.edu)

This was a project for DSC 80, which explored various data science techniques such as hypothesis testing, permutation testing, and missingness

## Introduction
The data set I will be using for this project is data collected from professional League of Legends games in 2022. The data includes champions selected, banned, creep scores, gold earned, etc. With this data set I will be trying to answer this question: Does Lee Sin require a gold advantage to be an effective champion?Lee Sin is often regarded as an early game champion that needs to take initiative early in the game in order to do well. This means his kit is geared towards being stronger at the start of the game, but later on being relatively weaker to other champions as the game progresses. Since professional games can be regarded as the highest skill level, where champions are utilized to the max, this dataset will provide good insight into the posed question. 
After some data cleaning, the data has provided us with 2261 rows, meaning there are 2261 matches that we can gather our information from. The main columns we are concerned with are'result', 'gamelength', 'golddiffat15'. 'result' tells us if the Lee Sin won or lost the match. 'gamelength' tells us how long the game went. 'golddiffat15' tells us the difference in gold earned between the Lee Sin and the opposing jungler (A jungler is a role that Lee Sin is most commonly played in. Each team has five roles and the gold difference uses the role to make comparisons in between teams). 

## Cleaning and EDA
### Data Cleaning
To clean the dataset, I first took the rows that had all the information I needed. This meant choosing only rows with datacompleteness. After, I filtered the data into all data involving Lee Sin. Next, I wanted the  'gamelength' in the form of a timedelta, so I converted that to the correct datatype. I did the same with 'result' so that it was in the form of booleans. I dropped all the columns with na values as I wasn't interested in any of them and I didn't want to run into any potential errors. Lastly, I calculated a new column called 'ahead' that stored a boolean that is True if the Lee Sin has a gold advantage at 15 minutes ('golddiffat15' is positive). 
Since there is a lot of data, dropping some of the rows with incomplete data was not a huge issue. Changing the data types allowed me to manipulate them and calculate other values such as 'ahead' with ease. 'ahead' becomes especially useful in my hypothesis testing as I can use it to categorize between matches where Lee Sin was ahead or not at 15 minutes.

| fixed_gamelength   | result   |   golddiffat15 | ahead   |
|:-------------------|:---------|---------------:|:--------|
| 0 days 00:21:14    | False    |             -6 | False   |
| 0 days 00:22:20    | False    |            565 | True    |
| 0 days 00:20:32    | True     |            122 | True    |
| 0 days 00:21:22    | False    |             86 | True    |
| 0 days 00:21:05    | False    |          -1433 | False   |


### Univariate Analysis

<iframe src="assets/LeeSin_Gold_Diff.html" width=800 height=600 frameBorder=0></iframe>

This plot shows the distribution of the Gold Difference for Lee Sin players at 15 minutes. The plot is more or less centered around 0, which makes sense because at a pro level it is hard to gain a clear gold advantage over the other team since both teams are playing at a high level.

### Bivariate Analysis

<iframe src="assets/gold_vs_length.html" width=800 height=600 frameBorder=0></iframe>

This plot compares gold difference at 15 minutes to the game length. We notice a slight negative trend in the data. This could mean that as the gold difference is larger, the games become shorter. This makes sense as the greater the gold difference, the greater the advantage Lee Sin has, and more likely than not Lee Sin will win before a comeback can be made. On the other side, if the game is longer, this means Lee Sin probably wasn't able to take advantage of the early game to secure a quick win and had a smaller  or negative gold difference.

### Interesting Aggregate

| result   |   False |    True |
|:---------|--------:|--------:|
| False    | 19.1333 | 20.3    |
| True     | 20.4167 | 18.8167 |

This pivot table holds the value of mean game duration, with the two axises being the game result (win/loss) and ahead (positive/negative gold difference at 15 minutes). As you can see, on average shorter games were when Lee Sin lost and was in a gold deficit, or when Lee Sin won and had a gold advantage. This might suggest that if Lee Sin has gold he will be very successful and the game will end in a win quicker, but conversly, if he is losing and in a gold deficit the game will end a loss quicker.

## Assessment of Missingness

### NMAR Analysis
Looking through the dataset, there does not seem to be any NMAR data. This makes sense as it seems like the data was recorded and reported by Riot Games themself or the league the match was played in. Most of the missing data is missing by design (MD). Some rows correspond to specific players and others to the overall team. As a result, team rows won't have individual creep score and player rows won't have overall gold income. More information on how the data was gathered might provide more insight into the missing data. One specific question I had was that why does it seem like the LPL league records different data than the rest? Are there perhaps different data regulations in the region?
### Missingness Dependency

<iframe src="assets/redline_missingness.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/missingness.html" width=800 height=600 frameBorder=0></iframe>

Exploring the missing data more, I wanted to see if the missing data in specific areas was specific to the LPL region. I conducted a permutation test to test if the missingness in the first dragon data was linked to the LPL region. 

Null Hypothesis: The missingness of the first dragon data does not depend on if the team is part of the LPL

Alternative Hypothesis: The missingness of the first dragon data is linked to if the team is part of the LPL.

With a = .05 and a p-value = 0, the null was rejected in favor of the alternative hypothesis. As we can see there were no cases where the proportion of missing the data while being in the LPL region was higher than the observed. As we assumed before, this might imply a lot of the missing data, not just first dragon, were due to the specific region. Looking into data regulations in the LPL might provide us with some idea to why this might be.



## Hypothesis Testing
Null Hypothesis: Lee Sin does not require a gold lead to be effective and instead can succeed equally well when down in gold versus when he is up in gold. This would mean the proportion of games won while ahead and the proportion of games won while behind come from the same distribution and are not effected by the gold difference.
Alternative Hypothesis: Lee Sin requires a gold lead to be successful and win at a higher rate. The proportion of winning games while ahead has a different distribution that that of the proportion of winning games while behind.
For this test, the test statistic will be the difference between the proportion of games won with a gold lead and the proportion of games won while in a gold deficit.
I chose these hypothesis as they reflect the question I am trying to answer. The hypothesis will show whether or not gold in the early game is important for Lee Sin to succeed. I chose this test statistic as it captures the proportions we are comparing into one value. If the null is correct we expect some values to be as extreme as the observed.
Using the usual significance level of a = .05, the test found a p-value of 0. As a result we reject the null hypothesis, meaning that the proportion of games won while ahead does not come from the same distribution as the proportion of games won while behind. This suggest that Lee Sin requires a gold lead to be an asset to their team, and will have a better chance of contributing to a win.

<iframe src="assets/hypotest.html" width=800 height=600 frameBorder=0></iframe> 


