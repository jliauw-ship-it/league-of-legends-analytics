# league-of-legends-analytics
# League of Legends Match Outcome Prediction

### Predicting wins and losses in professional League of Legends

**Author:** Your Name

---

# Introduction

This project analyzes professional League of Legends match data from Oracle's Elixir. The dataset contains both player-level and team-level information from professional matches, including game length, earned gold, team kills, objectives, and match results.

### Guiding Question

Can a team's in-game statistics be used to predict whether they will win or lose a professional League of Legends match?

### Why This Matters

- Shows which gameplay statistics are most associated with winning.
- Helps explain the importance of gold, kills, and objective control.
- Provides a base for predicting match outcomes using machine learning.

### Relevant Columns

| Column | Meaning |
|---|---|
| `result` | Whether the team won or lost |
| `earnedgold` | Total gold earned by the team |
| `teamkills` | Total kills secured by the team |
| `dragons` | Number of dragons secured |
| `barons` | Number of barons secured |
| `gamelength` | Length of the match |
| `position` | Used to filter for team-level rows |

---

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

The original dataset contains both player-level and team-level rows. Since this project focuses on team performance, I filtered the data to only include rows where `position == "team"`.

I then selected relevant columns such as `result`, `earnedgold`, `teamkills`, `dragons`, `barons`, and `gamelength`.

## Univariate Analysis

### Team Kills Distribution

Most professional teams record a moderate number of kills per game, while extremely high-kill games are less common.

### Game Length Distribution

Most professional matches fall within a relatively narrow range of durations. Very short and very long games are less common.

## Bivariate Analysis

### Earned Gold by Match Outcome

Winning teams generally earn more gold than losing teams, suggesting that economic advantages are strongly associated with match success.

## Interesting Aggregates

Winning teams tend to secure more objectives, earn more gold, and accumulate more kills on average than losing teams.

---

# Assessment of Missingness

## NMAR Analysis

One possible example of NMAR missingness in this dataset would be a statistic that is missing because of how the match was recorded or because the value itself affected whether it was collected. However, without more information about the data collection process, it is not possible to determine with certainty whether a column is NMAR.

## Missingness Dependency

I examined whether missingness in selected columns depended on other observed variables such as `result` and `gamelength`. This helps determine whether the missingness mechanism is more consistent with MCAR or MAR.

---

# Hypothesis Testing

## Question

Do winning teams secure more dragons than losing teams?

## Hypotheses

- **Null Hypothesis:** Winning and losing teams secure the same average number of dragons.
- **Alternative Hypothesis:** Winning teams secure more dragons on average than losing teams.

## Test Statistic

Difference in mean dragons:

`mean dragons for winning teams - mean dragons for losing teams`

## Conclusion

A permutation test was used to evaluate whether the observed difference in dragon control could occur by random chance. If the p-value is less than 0.05, I reject the null hypothesis and conclude that winning teams secure more dragons on average.

---

# Framing a Prediction Problem

## Prediction Task

The prediction task is to predict whether a team wins or loses a professional League of Legends match.

- **Task type:** Binary classification
- **Response variable:** `result`
- **Evaluation metrics:** Accuracy and F1-score

Accuracy measures the proportion of correctly classified matches, while F1-score provides a more balanced measure of precision and recall.

---

# Baseline Model

## Model Description

The baseline model is a Logistic Regression classifier.

Features used:

- `earnedgold`
- `teamkills`

Both features are quantitative variables. I used a `StandardScaler` and `LogisticRegression` in a single sklearn Pipeline.

## Performance

The baseline model performance should be reported after running the notebook.

---

# Final Model

## Feature Engineering

To improve the model, I added more gameplay features:

- `dragons`
- `barons`
- `gamelength`

I also engineered two additional features:

1. `gold_per_kill` = `earnedgold / (teamkills + 1)`
2. `objectives_secured` = `dragons + barons`

## Model Choice

The final model is a Random Forest Classifier. This model was chosen because it can capture nonlinear relationships between gold, kills, objectives, and match outcomes.

## Hyperparameter Tuning

I used GridSearchCV to tune:

- `n_estimators`
- `max_depth`
- `min_samples_split`

## Performance Comparison

The final model should be compared to the baseline model after running the notebook.

---

# Fairness Analysis

## Groups and Metric

I examined whether the final model performs equally well on short and long games.

- **Group X:** Short games, below the median game length
- **Group Y:** Long games, at or above the median game length

The evaluation metric is accuracy.

## Hypotheses

- **Null Hypothesis:** The model has equal accuracy for short and long games.
- **Alternative Hypothesis:** The model has different accuracy for short and long games.

## Conclusion

A permutation test was used to compare model accuracy across short and long games. If the p-value is less than 0.05, there is evidence that the model performs differently across the two groups.

---

# Conclusion

This project explored how professional League of Legends match statistics can be used to understand and predict match outcomes.

The analysis focuses on team-level performance and shows that gold earned, kills, and objective control are important factors associated with winning. The prediction models use these statistics to classify whether a team wins or loses, while the fairness analysis checks whether the final model performs consistently across short and long matches.
