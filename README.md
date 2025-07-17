# NBA Player Performance Analysis

This analysis explores historical NBA player performance using a structured set of questions. The primary goals were to assess trends in scoring, relationships between physical traits and statistics, and to build a predictive model for next season performance. The project was completed in an interactive notebook, with this README serving as the written report.

#### Part 1 - Data Cleaning & Exploration
#### Part 2 - Aggregation & Filtering
#### Part 3 - Correlation & Visualization
#### Part 4 - Comparative Analysis
#### Part 5 - Prediction
---

## Part 1 – Data Cleaning & Exploration

### 1. Is there any missing data? Strategy for handling them?

- **Missing**: Null values were only found in the college column, indicating that the player did not attend college (typically a high school or international prospect)
- **Strategy**: Missing college values were filled with `No College` to clearly indicate the player did not attend college

**Cleaning Draft Info:**
- Replaced 'Undrafted' entries in draft info with 0 and converted draft year/round/number to integers
- Alternate approach for draft year: draft year could be set to the player’s first recorded season in the dataset, assuming the player’s first season aligns with their draft year (by splitting the season column and using the earlier year, ex. 1999 for 1999-00 because the draft has traditionally been held in June before the season begins in the fall)

---

### 2. What do the summary statistics reveal about the dataset?

- Summary statistics provided a general numerical overview of the dataset, including central tendencies and dispersion
- For example, the average player is 27 years old (according to the mean) with a standard deviation of 4.33 years, meaning about 2/3 of players are between ages 23 and 31
- Can also be visualized with box and whisker plots if needed

---

## Part 2 — Aggregation & Filtering

### 3. How has average scoring changed over time?

- Average points per game (PPG) was calculated per season  
  *(note: this creates an average of averages)*
- A line plot shows that average scoring has varied slightly year to year, with a notable upward trend
- For visual clarity, the y-axis does not start at 0; however, this would be reconsidered in a formal publication or presentation

---

### 4. Who are the top 10 scorers across all seasons?

- Players were ranked using the `min` method to ensure tied scores received the same (lower) rank
- Players with a rank less than or equal to 10 were selected

---

### 5. Which colleges produced the most players averaging 20+ PPG (top 5)?

- Players who scored 20+ PPG in a season and attended college were selected
- Duplicates were dropped to ensure each player was only counted once
- Ranked using the `min` method to ensure ties were included

---

## Part 3 — Correlation & Visualization

### 6. Does player height strongly correlate with rebounding success?

- Total rebound percentage was calculated as the average of offensive and defensive percentages  
  *(This metric does not reveal the players’ true total rebounding percentage due to different amounts of offensive and defensive rebounding opportunities, but it does provide a fair approximation)*

**Correlations were calculated between height and:**
- Rebounds per game → 0.42 (weak to moderate, positive)
- Offensive rebound percentage → 0.59 (moderate to strong, positive)
- Defensive rebound percentage → 0.61 (moderate to strong, positive)
- Total rebound percentage → 0.68 (strongest correlation, positive)

→ Height correlates more strongly with rebound percentages (strongest with total rebound percentage) than with raw rebounding numbers

---

### 7. Do high scorers also tend to have high assists?

- Correlation coefficient: 0.66 (moderate to strong positive correlation)
- Suggests that higher scorers also tend to assist more frequently

---

## Part 4 — Comparative Analysis

### 8. Is there a significant difference in PPG between top-10 draft picks and other players?

**Test:** independent samples t-test (Welch’s)

**Assumptions:**
- Normality: assumed due to Central Limit Theorem and confirmed with histograms  
- Independence: reasonable to assume  
- Variances: unequal (based on Levene’s test, p = 4.33e-29), so Welch’s t-test was used  

**Hypotheses:**
- Null Hypothesis: there is no significant difference in ppg between top-10 draft picks and other players  
- Alternate Hypothesis: there is a significant difference in ppg between top-10 draft picks and other players  
- Alpha: 0.05  

**Results:**
- p-value: 7.81e-69 → reject null hypothesis, there is a significant difference  
- Cohen’s d = 1.54 → large effect size (means are approximately 1.54 standard deviations apart)

> **Note:** Career PPG were used for this test and results will vary if every season for each player was included instead

---

## Part 5 — Prediction

### 9. Predict PPG for next season

**Model:** Linear Regression  
**Features:**  
- Age  
- Player height  
- Draft number  
- Current season points per game  
- Assists  
- Usage percentage  
- True shooting percentage  
- Assist percentage  

**Target:** Next Season PPG  
> Using current season PPG as the target would introduce data leakage since most features are recorded at the same time

**Preprocessing:**
- A `next_ppg` column was created using `.shift(-1)` within each player’s group
- Final seasons and one-season careers were excluded (no target to predict)

**Evaluation Metrics:**  
- **Test set R²:** 0.76  
  → Approximately 76% of the variation in next season’s PPG is explained by the model  
  → This is relatively strong and within the acceptable range for athlete performance modeling (0.7–0.9)  

- **RMSE:** 2.98  
  → On average, the prediction is off by +/- 2.98 points  
  → Acceptable for this context, but accuracy expectations may vary depending on use case

---

### Future Considerations

1. **Comparing Models**: Try other regression models like Random Forest, XGBoost, Ridge/Lasso regression  
2. **Feature Engineering**: Create novel features (e.g., year-over-year changes, interaction terms)  
3. **Hyperparameter Tuning**: Unnecessary for linear regression but important for other models; can use Grid Search, Random Search, etc.

---

## Project Files

- `nba_player_analysis.ipynb`: Jupyter notebook containing all code, charts, and outputs  
- `README.md`: Written report and project summary  

---

## Data Source

Raw data file: `all_seasons.csv`  
Source: [Basketball Player Stats](https://www.kaggle.com/datasets/justinas/nba-players-data) – Kaggle

---

## Author

Brandon DiChiacchio — [GitHub](https://github.com/bdichiacchio)
