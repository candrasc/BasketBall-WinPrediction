## Summary:
The purpose of this project is to predict the outcome of NBA games using a vanilla neural network vs a logistic regression classifier. Both classifiers will use the same train and test data which I have transformed into an exponentially weighted average of home and away team stats.

I achieved the best results with the neural network, returning an accuracy of 67.8% and an AUC of 0.717 vs logistic regression which returned an accuracy of 66.6% and an AUC of 0.694

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

### Data Prep
Here we use a df where I aggregated the indivudal player stats by game_id to provide team stats for each game. 

Impute/Drop pesky null values for plus minus category. 

We then apply a simple 5 game moving average across all stats. 

Finally, we create a home and away df from these stats and merge them together so we have home and away stats in the same row.

### Feature Selection
Using selectKBest and f_classif, we select the strongest features. Plus minus was clearly the strongest. 

### Model Selection

Here I scaled the data and cross validated a number of different classifiers in order to determine which one we wanted to fine tune. 
Despite it's simplicity, logistic regression was the best. This is always nice as it is super fast to tune.

### Tuning Hyperparameters

I used GridSearchCV to tune the model for parameters: C, penalty, solver. These were selected for tuning based on recommendations online and looking at a number of kaggle competitions to see what was best practice. 

### Model Evaluations
Accuracy: 0.633, AUC: 0.66 log loss: 0.664

I also have a classification report and a confusion matrix in the notebook.

### Improving Feature Signficance
Here we improve our feature quality by applying an exponential moving average to give more weight to more recent games.

Accuracy: 0.666, AUC: 0.694 log loss: 0.616

### Improving Results with a Neural Net
Finally, I use a neural network on the preprocessed data from our log reg classifier notebook. 

I opted for a simple Keras Sequential network with 8 hidden layers. 

I then used the Keras sklearn wrapper in order to create an sklearn estimator from my model. This allows me to use sklearn's GridsearchCV to tune my neural net. I also made a custom sklearn scorer so that gridsearch would use AUC to evlaluate models, rather than accuracy.

In the end our best parameters returned an accuracy of 67.8% and AUC of 71.7. A fairly significant improvement from our tuned logistic regression model.
