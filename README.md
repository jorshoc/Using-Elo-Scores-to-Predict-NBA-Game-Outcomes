
# Predicting NBA Game Outcomes Using Elo Ratings

## Goal: 
The objective of this project is to predict both NBA regular season and playoff game outcomes based on teams' elo ratings, shooting percentages, and other statistics.

### Introduction: 
The fivethirtyeight.com website uses elo ratings to measure a team's strength throughout the regular season and playoffs. Depending on the final score of a game, the location of a game (home or away), and the elo ratings of the two teams entering a game, each team's elo rating changes after every game they play.  Furthermore, elo ratings are posited as more accurate than W-L records in assessing a team's strength.  Although fivethirtyeight does not explicitly define the formula it uses to calculate elo ratings, the general equation for elo ratings is defined here:

$E_0=1300$

$E_{i+1}=K(S_{team}-\frac{1}{1+10^{\frac{E_{i,opp}-E_i}{400}}})+E_i$

The $E_0$ term is a team's first elo rating, $E_{i+1}$ is a team's elo rating after a game, $E_i$ is a team's rating before a game, and $E_{i,opp}$ is the team's opponent's elo rating before the game.  Furthermore, K takes into account the winner of the game, the margin of victory, the location of the game, and the elo ratings of both teams before the game.  Please see the following two articles for more detailed explanations of NBA elo ratings:

- https://www.ergosum.co/nate-silvers-nba-elo-algorithm/
- https://fivethirtyeight.com/features/how-we-calculate-nba-elo-ratings/

The following Toward Data Science article is the inspiration for this project: https://towardsdatascience.com/predicting-the-outcome-of-nba-games-with-machine-learning-a810bb768f20
Although the objective of this project is the same as the objective the Toward Data Science project, the methodology of this project is different in that it uses data from a different timespan, slightly different NBA statistics, and different machine learning models.  As the Toward Data Science article above notes, other data scientists have attempted to predict NBA game outcomes.  Most of their models had an accuracy between 62% and 72%.  While most of these projects aim to maximize the overall accuracy metric, my project aims to maximize the AUC metric.  As we discuss in a later section, the AUC metric presents a better assessment of a model than the overall accuracy metric, because AUC measures a model's ability to predict both home team wins and away team wins.  A model may have a high overall accuracy but have little skill at predicting away team wins, for example.  We focus on maximizing AUC instead of overall accuracy to avoid this shortcoming, but we will still compare the overall accuracy of our models to the overall accuracy of the models used in the articles below:

- https://www.researchgate.net/publication/312236952_Predicting_the_Outcome_of_NBA_Playoffs_Based_on_the_Maximum_Entropy_Principle
- https://library.ndsu.edu/ir/bitstream/handle/10365/28084/Predicting%20Outcomes%20of%20NBA%20Basketball%20Games.pdf?sequence=1&isAllowed=y
- https://towardsdatascience.com/building-my-first-machine-learning-model-nba-prediction-algorithm-dee5c5bc4cc1

### Project Sections
1. Import Packages
2. Clean Data
3. Explore Data with Visualizations
4. Train and Test Machine Learning Models
    

### Data
- Fivethirtyeight elo ratings data downloaded from https://www.kaggle.com/datasets/fivethirtyeight/fivethirtyeight-nba-elo-dataset
    - nbaallelo.csv
- NBA game statistics data downloaded from https://www.kaggle.com/datasets/nathanlauga/nba-games
    - games.csv
    - games_details.csv
- We analyze data from 5 seasons: 2010-2011, 2011-2012, 2012-2013, 2013-2014, 2014-2015
- We train/test our models on the following variables below; many of the predictor variables are new features that we create from the 3 data sets above:
    - Response Variable:
        - HOME_TEAM_WINS: 1 if home teams wins, 0 if away team wins
    - Predictor Variables:
        - is_playoffs: 1 if playoff game, 0 if regular season game
        - elo_i: home team elo before a game
        - opp_elo_i: away team elo before a game
        - home_pts_ave: home team average pts from prior 10 games
        - away_pts_ave: away team average pts from prior 10 games
        - home_fg_pct_ave: home team average field goal pct from prior 10 games
        - away_fg_pct_ave: away team average field goal pct from prior 10 games
        - home_ft_pct_ave: home team average free throw pct from prior 10 games
        - away_ft_pct_ave: away team average free throw pct from prior 10 games
        - home_fg3_pct_ave: home team average 3pt field goal pct from prior 10 games
        - away_fg3_pct_ave: away team average 3pt field goal pct from prior 10 games
        - home_assists_ave: home team average assists from prior 10 games
        - away_assists_ave: away team average assists from prior 10 games
        - home_rebounds_ave: home team average rebounds from prior 10 games
        - away_rebounds_ave: away team average rebounds from prior 10 games