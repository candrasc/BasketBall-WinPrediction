## Summary:
The purpose of this project is to predict the outcome of an NBA game based on a rolling average of the home and away team's statistics.
We achieve an accuracy of 67%, a logloss of 0.62, and an AUC of 0.70, which is better than the top voted prediction notebook on this Kaggle dataset. 

### Credits:
Download the datasets here: https://www.kaggle.com/nathanlauga/nba-games

Github Repo that was used to pull these datasets: https://github.com/Nathanlauga/nba-predictor

### Data:
For this analysis we will primarily be using the games and game_details data frames. 

Games: Has overall team statistics for each game
- We will use this for some introductory data visulization

Game_details: This includes individual player stats for each player of each team. It also includes more descriptive stats such as offensive and defensive rebounds.
- We will end up aggregating this data to a team level and use it for our predictions

## Project Info

### Data Visualization:
There is some unrelated visualization related to points/assists while teams are home and away... this is unrelated to the prediction model, but it was interesting to see so I have left it up for others. 

The important visualization is the heatmaps. These show the relationships of certain statistics with a home team win. Surprising findings include that only defensive rebounds are signficant for the home team winning. Offensive rebounds have no correlation. 

### Data Prep
Here we use a previous df where I grouped the indivudal stats by game_id to provide team stats for a game. 

Drop pesky null values for plus minus category. 

We then apply a simple 5 game moving average across all stats. This required me to loop through the data frame by each team name so that we could apply the rolling average to only one team at a time as the df was chronoloigical so different teams were playing in each row. (eg If we just applied a rolling ave, then Toronto stats would have been combined with Atlanta Stats etc). 

Finally, we create a home and away df from these stats and merge them together so we have home and away stats in the same row

### Feature Selection
Using selectKBest and f_classif, we select the strongest features. Plus minus was clearly the strongest. 

### Model Selection

Here I scaled the data and cross validated a number of different classifiers in order to determine which one we wanted to fine tune. 
Despite it's simplicity, logistic regression was the best. This is always nice as it is super fast to tune.

### Tuning Hyperparameters

I used GridSearchCV to tune the model for parameters: C, penalty, solver. These were selected for tuning based on recommendations online and looking at a number of kaggle competitions to see what was best practice. 

### Model Evaluations
Accuracy: 0.6383978998069735, log loss: 0.6427307748117226

I also have a classification report and a confusion matrix in the notebook.

### Improving Feature Signficance
Here we improve our feature quality by applying an exponential moving average to give more weight to more recent games.

Accuracy: 0.6665410290621402, log loss: 0.6136994090513114

