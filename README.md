# LoL_LeeSin_Project
by Adam Tran (ant010@ucsd.edu)
This was a project for DSC 80, which explored various data science techniques such as hypothesis testing, permutation testing, and missingness

## Introduction
The data set I will be using for this project is data collected from professional League of Legends games in 2022. The data includes champions selected, banned, creep scores, gold earned, etc. With this data set I will be trying to answer this question: Does Lee Sin require a gold advantage to be an effective champion?Lee Sin is often regarded as an early game champion that needs to take initiative early in the game in order to do well. This means his kit is geared towards being stronger at the start of the game, but later on being relatively weaker to other champions as the game progresses. Since professional games can be regarded as the highest skill level, where champions are utilized to the max, this dataset will provide good insight into the posed question. 
After some data cleaning, the data has provided us with 2261 rows, meaning there are 2261 matches that we can gather our information from. The main columns we are concerned with are'result', 'gamelength', 'golddiffat15'. 'result' tells us if the Lee Sin won or lost the match. 'gamelength' tells us how long the game went. 'golddiffat15' tells us the difference in gold earned between the Lee Sin and the opposing jungler (A jungler is a role that Lee Sin is most commonly played in. Each team has five roles and the gold difference uses the role to make comparisons in between teams). 

## Cleaning and EDA
To clean the dataset, I first took the rows that had all the information I needed. This meant choosing only rows with datacompleteness. After, I filtered the data into all data involving Lee Sin. Next, I wanted the  'gamelength' in the form of a timedelta, so I converted that to the correct datatype. I did the same with 'result' so that it was in the form of booleans. I dropped all the columns with na values as I wasn't interested in any of them and I didn't want to run into any potential errors. Lastly, I calculated a new column called 'ahead' that stored a boolean that is True if the Lee Sin has a gold advantage at 15 minutes ('golddiffat15' is positive). 

## Assessment of Missingness

## Hypothesis Testing
