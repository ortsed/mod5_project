# Random Football


## Predicting NFL Game Winners using Random Forests

Flatiron mod 5 project by Llew and Jon


## Question - Can we predict the winner of NFL games?

In general: not very well.  Football games are determined by many random factors with individual plays having significant impact on the results.  The results of those individual plays are often determined by slim margins (for example, did the ball cross the line to gain? or did the players shoe-touch the out-of-bounds line?)

We revised our goal to be: can we beat Vegas in picking the winner?  Vegas oddsmakers select a favorite team, implied by the point spread.  If the point spread for a team is negative, they are the favorite.


## Libraries Used



*   pandas
*   numpy
*   patsy
*   sklearn


## Data

<span style="text-decoration:underline;">NFL Scores and Stadium Data</span>

[https://www.kaggle.com/tobycrabtree/nfl-scores-and-betting-data](https://www.kaggle.com/tobycrabtree/nfl-scores-and-betting-data)



*   Game info going back to 1966 including scores, weather, &  betting spread and over/under

<span style="text-decoration:underline;">FiveThirtyEight Elo Data 	</span>			[https://projects.fivethirtyeight.com/nfl-api/nfl_elo.csv](https://projects.fivethirtyeight.com/nfl-api/nfl_elo.csv)



*   FiveThirtyEight Elo ratings for each game back to 1920

Features gathered from the kaggle dataset included:



*   Season (Year)
*   Week of Season
*   Elevation
*   Home Team
*   Away Team
*   Weather Conditions

From the FiveThirtyEight Elo Data:



*   Elo probability of home team win

Additional features were engineered from the Kaggle data.  The full dataset was rearranged for each team to calculate the following:



*   Season Win Percentage prior to the current game
*   Season Average Points Scored prior to the current game
*   Distance Travelled since last game
*   Days Elapsed since last game


## Modeling

We selected the 2017 NFL season as our initial testing data, which allowed us to have a sizeable history of training data and also have the ability to move the model forward in time and test on the 2018 NFL season.

After initial attempts to model using logistic regression, support vector machines, gradient boosting, and single decision trees, we found the most successful model was a random forest regressor.

Random forests do not require feature scaling, so our features remained in their original units.

We used a Grid Search to find the best fitting hyperparameters for the random forest.  However, we note that the Grid Search produced different results on each run due to the random nature of random forests.  We arbitrarily assigned a random state to the Grid Search to maintain a single solution set of parameters.

The key parameters of the random forest regressor include:



*   ‘n_estimators’: 50 
    *   Number of decision trees
*   'bootstrap': True 
    *   Bootstrap samples are used to build decision trees
*    'max_depth': 10
    *   The maximum depth of the tree is 10.
*    'max_features': 'sqrt' 
    *   The number of features to consider when looking for the best split is limited to sqrt(n_features)
*    'min_samples_leaf': 4
    *   The minimum number of samples required to be at a leaf node is 4.
*    'min_samples_split': 2
    *   The minimum number of samples required to split an internal node is 2.

Our model produced the following features as the most important and the associated Feature Importance Percentage:



1. Elo Probability Home - 22%
2. Away Team Average Score - 9%
3. Home Team Season Win % - 8%
4. Away Team Season Win % - 7%
5. Away Team Travel Distance - 7%

The model was run on each data in our dataset beginning with the 2012 NFL Season (which only trained on the 2011 NFL season).  We limited the training data to 4 seasons, based on both decreasing accuracy at greater numbers of seasons, and the intuition that NFL teams turn over their players and/or coaching staff in this period of time.


## Results


<table>
  <tr>
   <td><strong>Test Season</strong>
   </td>
   <td><strong>Training Season(s)</strong>
   </td>
   <td><strong>Baseline All-Home Accuracy</strong>
   </td>
   <td><strong>Target Vegas Accuracy</strong>
   </td>
   <td><strong>Random Forest Accuracy</strong>
   </td>
   <td><strong>Close-call Home Accuracy</strong>
   </td>
   <td><strong>Close-call Abstain Accuracy</strong>
   </td>
  </tr>
  <tr>
   <td><strong>2012</strong>
   </td>
   <td><strong>2011</strong>
   </td>
   <td><p style="text-align: right">
<strong>56.55%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>63.67%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>62.17%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>61.42%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>69.43%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2013</strong>
   </td>
   <td><strong>2011-2012</strong>
   </td>
   <td><p style="text-align: right">
<strong>59.18%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>64.42%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>60.30%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>58.43%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>69.60%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2014</strong>
   </td>
   <td><strong>2011-2013</strong>
   </td>
   <td><p style="text-align: right">
<strong>57.30%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>68.54%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>66.29%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>62.55%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>70.00%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2015</strong>
   </td>
   <td><strong>2011-2014</strong>
   </td>
   <td><p style="text-align: right">
<strong>54.31%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>64.04%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>63.30%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>59.18%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>70.27%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2016</strong>
   </td>
   <td><strong>2012-2015</strong>
   </td>
   <td><p style="text-align: right">
<strong>57.68%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>67.04%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>64.04%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>58.80%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>66.87%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2017</strong>
   </td>
   <td><strong>2013-2016</strong>
   </td>
   <td><p style="text-align: right">
<strong>56.93%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>60.30%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>64.04%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>62.17%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>68.39%</strong></p>

   </td>
  </tr>
  <tr>
   <td><strong>2018</strong>
   </td>
   <td><strong>2014-2017</strong>
   </td>
   <td><p style="text-align: right">
<strong>58.80%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>67.42%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>62.17%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>63.67%</strong></p>

   </td>
   <td><p style="text-align: right">
<strong>64.33%</strong></p>

   </td>
  </tr>
</table>



## Conclusion



*   Random Forest model accuracy (62%-66%) regularly beats a naive, home-team-wins strategy (54%-59%).
*   With Elos included and using a strategy of not betting on close-calls, Random Forest Regressor model accuracy (64%-70%) beat out Vegas odds (60%-68%).
