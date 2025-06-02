# DSA210: NBA Player Career Length Analysis

## Data Source

The dataset is sourced from two places:

1. **Kaggle Dataset**: A comprehensive dataset of NBA and ABA players from 1947 to 2022, providing player names, height, weight, points per game (PPG), assists per game (APG), rebounds per game (TRB), player efficiency rating (PER), games played (G), career start and end years, and career length. Link to Kaggle dataset.
2. **NBA API**: Additional statistics (steals per game (SPG), blocks per game (BPG), and turnovers per game (TPG)) were extracted using the `nba_api` Python library to enrich the Kaggle dataset, as these metrics are critical for a holistic analysis but were absent in the original data.

### Data Collection Process

- **Kaggle Data**: The Kaggle dataset was imported as a CSV file (`NBA_players_clean.csv`) into a pandas DataFrame for initial processing.
- **NBA API Data**: Using the `nba_api.stats` module, I retrieved per-game statistics for players starting from 1980. To handle API limitations (e.g., `ReadTimeout` errors in Google Colab), I implemented a script to resume data collection from player 1201, processing players in batches of 50 with 3 retries, 2-second pauses between players, and 60-second pauses between batches. A function using `unicodedata.normalize('NFD')` was applied to remove accents from player names (e.g., José Calderón → Jose Calderon) to ensure consistency when merging with the Kaggle dataset.
- **Data Merging**: The API-extracted data (SPG, BPG, TPG) was merged with the Kaggle dataset after cleaning player names to match formats, creating a unified dataset for analysis.

## Data Analysis

The analysis followed the data science pipeline, with the following stages:

### 1. Data Cleaning and Preparation

- **Filtering**: Restricted the dataset to players who:
  - Started their careers in 1980 or later (post-NBA/ABA merger and 3-point line introduction).
  - Retired by the 2020 season (excluding active players).
  - Played at least 10 games to ensure sufficient statistical sample size.
- **Accent Removal**: Applied a custom function to remove diacritics from player names in the Kaggle dataset for consistent merging with API data.
- **Data Enrichment**: Added two derived features:
  - `AST_TO_TURNOVER`: Assists per game divided by turnovers per game (with a small constant to avoid division by zero).
  - `DEF_SCORE`: Sum of steals per game (SPG) and blocks per game (BPG) to capture defensive contributions.
- **Handling Missing Data**: Filled NaN values in `AST_TO_TURNOVER` with 0 to maintain data integrity.

### 2. Exploratory Data Analysis (EDA)

- **Skewness Analysis**: Calculated skewness for features (`PTS`, `TRB`, `AST`, `PER`, `SPG`, `BPG`, `TPG`, `Years`, `AST_TO_TURNOVER`, `DEF_SCORE`) to assess data distribution. Highly skewed features (`PTS`, `TRB`, `AST`, `SPG`, `BPG`, `TPG`, `AST_TO_TURNOVER`, `DEF_SCORE`) with absolute skewness &gt; 1 were identified for transformation.
- **Visualizations**: Used `matplotlib` and `seaborn` to create plots (e.g., histograms, correlation heatmaps) to explore relationships between features and career length.

### 3. Hypothesis Testing

- **Null Hypothesis**: There is no significant relationship between a player’s career data (PPG, APG, TRB, BMI, PER, SPG, BPG, TPG) and career length.
- **Alternative Hypothesis**: There is a significant relationship between these metrics and career length.
- **Methodology**: Conducted statistical tests (e.g., correlation analysis, t-tests) to evaluate the significance of each feature’s relationship with career length.

### 4. Feature Transformation

- **Log Transformation**: Applied `np.log1p` to highly skewed features and the target variable (`Years`) to normalize distributions, handling non-positive values by replacing them with 0.1 before transformation. The `PER` feature was not transformed due to low skewness.
- **Feature Selection**: Used the following features for modeling: `PTS`, `TRB`, `AST`, `PER`, `SPG`, `BPG`, `TPG`, `AST_TO_TURNOVER`, `DEF_SCORE`.

### 5. Machine Learning

- **Dataset Split**: Split the transformed dataset into 80% training and 20% testing sets using `train_test_split` with a random state of 42.
- **Models Applied**:
  - **Linear Regression**: A baseline model to predict career length.
  - **Random Forest Regression**: A non-linear model to capture complex relationships.
  - **XGBoost Regression**: A gradient boosting model for improved predictive performance.
- **Evaluation Metrics**:
  - **R² Score**: Measures the proportion of variance in career length explained by the model.
  - **RMSE**: Quantifies prediction error in original units (years) after reversing the log transformation using `np.expm1`.
- **Results**:
  - **Linear Regression**: R² = 0.522, RMSE = 3.432
  - **Random Forest Regression**: R² = 0.561, RMSE = 3.287
  - **XGBoost Regression**: R² = 0.487, RMSE = 3.556
  - Random Forest performed best, indicating non-linear relationships between features and career length.

## Findings

- **Key Predictors**: Features like `PTS`, `TRB`, `AST`, and `PER` showed stronger correlations with career length, suggesting that scoring, rebounding, playmaking, and overall efficiency are significant factors in career longevity.
- **Defensive Metrics**: The derived `DEF_SCORE` (SPG + BPG) had a moderate influence, indicating defensive contributions also play a role.
- **Skewness Impact**: Log transformation of skewed features improved model performance, particularly for Random Forest, highlighting the importance of handling non-normal distributions.
- **Model Performance**: Random Forest outperformed Linear Regression and XGBoost, suggesting that career length is influenced by complex, non-linear interactions among performance metrics.
- **Hypothesis Outcome**: The alternative hypothesis is supported, as several performance metrics showed significant relationships with career length, rejecting the null hypothesis.

## Limitations

- **Data Completeness**: The Kaggle dataset may lack some player records, and API data extraction faced challenges (e.g., timeouts), potentially missing some players.
- **Feature Scope**: Additional factors like injuries, team roles, or off-court factors (e.g., contracts, coaching changes) were not included but could influence career length.
- **Model Limitations**: The moderate R² scores (0.487–0.561) indicate that other unmodeled factors may explain variance in career length.
- **API Constraints**: Frequent `ReadTimeout` errors required workarounds (e.g., batch processing, pauses), limiting the efficiency of data collection.

## Future Work

- **Incorporate Additional Features**: Include injury history, minutes played per game, or advanced metrics (e.g., true shooting percentage) to improve model accuracy.
- **Alternative Data Sources**: Explore Basketball-Reference or other platforms to supplement missing data or bypass API limitations.
- **Advanced Modeling**: Experiment with hyperparameter tuning for Random Forest and XGBoost or try other models (e.g., neural networks) to enhance predictive performance.
- **Temporal Analysis**: Analyze how career length trends have changed over decades due to evolving NBA playstyles.

## AI Assistance

I used Grok 3, created by xAI, to assist with the following:

- **Script Optimization**: Modified the data collection script to resume from player 1201, handling `ReadTimeout` errors with batch processing (50 players, 3 retries, 2s/60s pauses).
- **Accent Removal**: Provided a function using `unicodedata.normalize('NFD')` to clean player names for data merging.
- **Debugging**: Offered diagnostic snippets for API issues and suggested workarounds (e.g., resetting Colab runtime, using alternative data sources).
- **Report**: Helped when writing this  report.
- **Prompt Used**: "Source yourself in how I obtained help from you" to document Grok’s contributions.

All AI assistance has been explicitly documented as per the project guidelines.
