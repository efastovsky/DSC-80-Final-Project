# DSC-80-Final-Project - This is the file
# Intro
The dataset used in this project comes from Oracle's Elixir, a comprehensive source for professional League of Legends (LoL) esports match data. It contains detailed match statistics spanning multiple years, regions, and competitive leagues. This dataset is invaluable for understanding the dynamics of professional LoL games, with insights into player performance, team strategies, and game outcomes.The central question driving this project is:

**"Do tier-1 leagues have significantly different average game lengths compared to lower-tier leagues? If so can we use the game lengths to predict the Gold difference at 15 minutes"**

Gold difference at 15 minutes and game lengths is a crucial metric in LoL, as it often indicates which team is likely to secure a victory. By understanding the factors influencing this metric such as game length, teams, analysts, and fans can gain deeper insights into early-game strategies and their impact on overall performance.

**Why This Matters**
For esports analysts, players, and fans, the early game in LoL sets the stage for the rest of the match. Predicting gold at 15 minutes:

Informs Strategy: Helps teams understand which early-game decisions lead to an advantage.
Enhances Analysis: Allows analysts to identify patterns in regional playstyles or differences between leagues.
Engages Fans: Provides a data-driven perspective on the action-packed first phase of a game.
The ability to predict gold difference at 15 minutes could also help teams refine their training regimens and draft strategies.

**Dataset Overview**
The dataset contains N rows (replace N with the actual row count) and the following relevant columns for the question:

goldat15:

Description: The gold difference at the 15-minute mark between the two teams.
Relevance: This is the target variable we aim to predict.

gamelength:

Description: The total length of the game in seconds.
Relevance: Converted into minutes, this feature helps normalize early-game metrics and provides context for the pacing of the game.

league:

Description: The professional league in which the match was played (e.g., LCK, LCS, LEC, etc.).
Relevance: Different leagues often have unique playstyles that can influence early-game outcomes.

kills:

Description: The total number of kills achieved by both teams in the match.
Relevance: Combined with game length, it helps calculate the kills per minute, a key indicator of early-game aggression.

golddiffat15:

Description: The absolute gold difference between the two teams at 15 minutes.
Relevance: This column is used to engineer the gold difference per minute feature, which normalizes early-game gold leads based on game duration.

Engineered Features:

gamelength_minutes: Game length converted from seconds to minutes.
kills_per_minute: Kills divided by game length in minutes, representing early-game aggression.
gold_diff_per_minute: Gold difference at 15 minutes divided by game length in minutes, normalizing early-game leads.

# Data Cleaning & Exploratory Data Analysis
## Data Cleaning Steps

The following steps were taken to clean the dataset:

1. **Dropped Missing Target Values**: Rows with missing values in the `goldat15` column were removed to ensure the model had valid data for training.
2. **Created data frames for tier 1 leagues and not tier 1**: LPL has been left out of this data set because they do not provide the same data as the other leagues
3. **Converted Game Length**: The `gamelength` column was converted from seconds to minutes to normalize early-game metrics.
4. **Engineered Features**:
   - `kills_per_minute`: Calculated as the total kills divided by the game length in minutes to measure early-game aggression.
   - `gold_diff_per_minute`: Derived by normalizing `golddiffat15` using game length in minutes to account for pacing.
5. **Encoded Categorical Features**: The `league` column was one-hot encoded to represent regional playstyles numerically.

### Cleaned DataFrame

Below is the first five rows of the cleaned DataFrame used for analysis:
| gameid           | datacompleteness   |   url | league   |   year | split   |   playoffs | date                |   game |   patch |   participantid | side   | position   | playername   | playerid                                  | teamname          | teamid                                  | champion     | ban1    | ban2   | ban3   | ban4   | ban5     |   pick1 |   pick2 |   pick3 |   pick4 |   pick5 |   gamelength |   result |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   firstdragon |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders |   firstherald |   heralds |   opp_heralds |   void_grubs |   opp_void_grubs |   firstbaron |   barons |   opp_barons |   firsttower |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   gspd |   gpr |   total cs |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |   cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |   goldat20 |   xpat20 |   csat20 |   opp_goldat20 |   opp_xpat20 |   opp_csat20 |   golddiffat20 |   xpdiffat20 |   csdiffat20 |   killsat20 |   assistsat20 |   deathsat20 |   opp_killsat20 |   opp_assistsat20 |   opp_deathsat20 |   goldat25 |   xpat25 |   csat25 |   opp_goldat25 |   opp_xpat25 |   opp_csat25 |   golddiffat25 |   xpdiffat25 |   csdiffat25 |   killsat25 |   assistsat25 |   deathsat25 |   opp_killsat25 |   opp_assistsat25 |   opp_deathsat25 |   gamelength_minutes |   kills_per_minute |   gold_diff_per_minute | is_major_league   |
| LOLTMNT99_132542 | complete           |   nan | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |               1 | Blue   | top        | Taegyeong    | oe:player:ee75cdc0e3eb95a85c0b86439acc3a8 | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Renekton     | Orianna | Rell   | Viego  | Viktor | Malphite |     nan |     nan |     nan |     nan |     nan |  1.85957e-06 |        1 |       4 |        1 |         4 |          20 |            7 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.8299 | 1.1203 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               11806 | 489.875 |     0.192039  |                886.805 |                    947.759 |             7 | 0.2905 |             2 | 0.083  |                    3 |            17 | 0.7054 |       10762 |         7537 |      312.739 |          0.207072 |        9025 |    nan |   nan |        210 |           210 |              0 |                     nan |                       nan | 8.7137 |       3325 |     4805 |       84 |           3293 |         4921 |           87 |             32 |         -116 |           -3 |           0 |             0 |            0 |               0 |                 0 |                0 |       5229 |     7644 |      136 |           5804 |         8247 |          144 |           -575 |         -603 |           -8 |           0 |             0 |            0 |               1 |                 0 |                0 |       7414 |    10282 |      178 |           7893 |        11109 |          191 |           -479 |         -827 |          -13 |           1 |             1 |            1 |               1 |                 0 |                1 |      10762 |    13586 |      210 |           8834 |        12459 |          207 |           1928 |         1127 |            3 |           4 |             4 |            1 |               1 |                 1 |                3 |             0.401667 |            9.95851 |              -1431.54  | False             |
| LOLTMNT99_132542 | complete           |   nan | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |               2 | Blue   | jng        | CHEF         | oe:player:3a36b513e63e14a1355896711a09362 | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Xin Zhao     | Orianna | Rell   | Viego  | Viktor | Malphite |     nan |     nan |     nan |     nan |     nan |  1.85957e-06 |        1 |       5 |        1 |        11 |          20 |            7 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.8299 | 1.1203 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        1 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               12455 | 516.805 |     0.202596  |               1089.54  |                    742.656 |             7 | 0.2905 |            14 | 0.5809 |                    7 |            40 | 1.6598 |       10895 |         7670 |      318.257 |          0.210727 |        8625 |    nan |   nan |        154 |            27 |            127 |                     nan |                       nan | 6.39   |       3409 |     3166 |       67 |           3308 |         2837 |           54 |            101 |          329 |           13 |           1 |             1 |            0 |               1 |                 1 |                0 |       5366 |     5342 |       99 |           5130 |         5449 |          105 |            236 |         -107 |           -6 |           2 |             2 |            1 |               1 |                 1 |                0 |       7918 |     7822 |      141 |           6656 |         7866 |          138 |           1262 |          -44 |            3 |           4 |             3 |            1 |               1 |                 2 |                0 |      10895 |    11286 |      154 |           7952 |         9331 |          154 |           2943 |         1955 |            0 |           5 |            11 |            1 |               2 |                 3 |                2 |             0.401667 |           12.4481  |                587.552 | False             |
| LOLTMNT99_132542 | complete           |   nan | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |               3 | Blue   | mid        | Nakyung      | oe:player:d20ff5c9f6086feee4a378eb0cfc2e4 | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | LeBlanc      | Orianna | Rell   | Viego  | Viktor | Malphite |     nan |     nan |     nan |     nan |     nan |  1.85957e-06 |        1 |       1 |        3 |         8 |          20 |            7 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  1 |     0.8299 | 1.1203 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               14082 | 584.315 |     0.229061  |                679.253 |                    316.432 |             6 | 0.249  |             7 | 0.2905 |                    3 |            24 | 0.9959 |        9560 |         6335 |      262.863 |          0.174046 |        8075 |    nan |   nan |        208 |           208 |              0 |                     nan |                       nan | 8.6307 |       3310 |     4735 |       85 |           3521 |         4722 |           88 |           -211 |           13 |           -3 |           0 |             1 |            1 |               0 |                 2 |                0 |       5015 |     7406 |      133 |           5424 |         7367 |          137 |           -409 |           39 |           -4 |           0 |             1 |            1 |               0 |                 2 |                0 |       7253 |    10221 |      180 |           7553 |         9107 |          160 |           -300 |         1114 |           20 |           0 |             4 |            1 |               1 |                 3 |                1 |       9560 |    13284 |      208 |           8900 |        11061 |          190 |            660 |         2223 |           18 |           1 |             8 |            3 |               1 |                 4 |                3 |             0.401667 |            2.48963 |              -1018.26  | False             |
| LOLTMNT99_132542 | complete           |   nan | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |               4 | Blue   | bot        | Ruep         | oe:player:8784bcdcfb37672c12481c037ad8db4 | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Varus        | Orianna | Rell   | Viego  | Viktor | Malphite |     nan |     nan |     nan |     nan |     nan |  1.85957e-06 |        1 |       8 |        1 |         7 |          20 |            7 |             2 |             1 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.8299 | 1.1203 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            1 |                0 |               17295 | 717.635 |     0.281325  |                334.025 |                    273.527 |            10 | 0.4149 |             9 | 0.3734 |                    6 |            33 | 1.3693 |       13097 |         9872 |      409.627 |          0.271228 |       10350 |    nan |   nan |        234 |           230 |              4 |                     nan |                       nan | 9.7095 |       4292 |     3166 |       81 |           2866 |         3153 |           69 |           1426 |           13 |           12 |           3 |             1 |            0 |               0 |                 0 |                1 |       7296 |     5626 |      141 |           4485 |         4579 |          101 |           2811 |         1047 |           40 |           4 |             2 |            0 |               0 |                 1 |                3 |      10041 |     8503 |      198 |           6547 |         7073 |          149 |           3494 |         1430 |           49 |           5 |             4 |            1 |               0 |                 3 |                3 |      13097 |    11282 |      234 |           8081 |         8809 |          170 |           5016 |         2473 |           64 |           8 |             7 |            1 |               1 |                 4 |                5 |             0.401667 |           19.917   |               6998.34  | False             |
| LOLTMNT99_132542 | complete           |   nan | TSC      |   2024 | Winter  |          0 | 2024-01-05 14:08:39 |      1 |   14.01 |               5 | Blue   | sup        | Carry        | oe:player:a02a243c98fb6794c2a303c5014567f | BoostGate Esports | oe:team:331c852427c61f82050b618c2b442c4 | Renata Glasc | Orianna | Rell   | Viego  | Viktor | Malphite |     nan |     nan |     nan |     nan |     nan |  1.85957e-06 |        1 |       2 |        1 |        17 |          20 |            7 |             1 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.8299 | 1.1203 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |                5839 | 242.282 |     0.0949786 |                225.021 |                    271.701 |            34 | 1.4108 |             7 | 0.2905 |                   11 |            72 | 2.9876 |        8209 |         4984 |      206.805 |          0.136927 |        6650 |    nan |   nan |         25 |            25 |              0 |                     nan |                       nan | 1.0373 |       2668 |     2378 |       11 |           2652 |         2060 |           13 |             16 |          318 |           -2 |           0 |             3 |            1 |               1 |                 0 |                3 |       4128 |     4003 |       21 |           3898 |         3430 |           20 |            230 |          573 |            1 |           0 |             5 |            1 |               1 |                 1 |                3 |       5620 |     5600 |       21 |           5349 |         5135 |           30 |            271 |          465 |           -9 |           0 |             9 |            1 |               2 |                 2 |                5 |       8209 |     8891 |       25 |           6015 |         5842 |           31 |           2194 |         3049 |           -6 |           2 |            17 |            1 |               2 |                 2 |                7 |             0.401667 |            4.97925 |                572.614 | False             |

### ADD DATA FRAMES AND PLOTY FOR PT 2 

<iframe
  src="assets/game_length_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows the distribution of game lengths across all matches. The data suggests most matches last around a specific duration, with some variability.

<iframe
  src="assets/gold_vs_kills_scatter.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The scatter plot illustrates the relationship between kills at 15 minutes and gold difference at 15 minutes. Tier-1 leagues are highlighted in red and lower-tier leagues in blue. Major leagues generally show more structured performance patterns, with fewer extreme values.

Selection deleted


# Create DataFrame


# Create a pivot table based on averages for teams in major leagues and those that aren't
pivot_table = df.pivot_table(
    index='is_major_league',  # Group by 'is_major_league'
    values=['gamelength', 'goldat15', 'killsat15'],  # Columns to aggregate
    aggfunc='mean'  # Calculate averages
)

# Reset index for better readability
pivot_table = pivot_table.reset_index()

# Rename columns for clarity
pivot_table.rename(columns={
    'gamelength_minutes': 'Avg Game Length',
    'goldat15': 'Avg Gold at 15',
    'killsat15': 'Avg Kills at 15'
}, inplace=True)

# Output the pivot table
print(pivot_table.to_markdown(index=False))
| is_major_league   |   gamelength |   Avg Gold at 15 |   Avg Kills at 15 |
|:------------------|-------------:|-----------------:|------------------:|
| False             |      31.4611 |          8421.03 |           1.53914 |
| True              |      32.3992 |          8399.79 |           1.125   |

Major leagues exhibit slightly longer and more consistent game lengths, with similar early-game gold but has less average kills compared to non-major leagues, which display greater variability in early-game aggression.



# Assessment of Missingness
This data does not have NMAR data as the missing values of the columns are almost always MAR as missingness usually depends on the type of league. If a team is not in a tier 1 league their data is registered as partial and the LPL which is the Chinese tier 1 league does not report their data. So 'goldat15' missingness would be depending on the leauge. 

<iframe
  src="assets/league_permutation_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


<iframe
  src="assets/result_permutation_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


# Hypothesis Testing
## Research Question
**"Does the model perform equally well in predicting `goldat15` for matches in major leagues compared to minor leagues?"**

---

# Hypotheses

- **Null Hypothesis (\(H_0\))**:  
  The model performs equally well for matches in major and minor leagues. The mean squared error (MSE) for predictions in both groups is the same, and any observed differences are due to random variation.  
  \[
  H_0: \text{MSE}_{\text{major}} = \text{MSE}_{\text{minor}}
  \]

- **Alternative Hypothesis (\(H_a\))**:  
  The model performs unequally for matches in major and minor leagues. The mean squared error (MSE) for predictions in the two groups is significantly different.  
  \[
  H_a: \text{MSE}_{\text{major}} \neq \text{MSE}_{\text{minor}}
  \]

---

## Test Statistic and Significance Level

- **Test Statistic**: The difference in Mean Squared Error (MSE) between predictions for major leagues and minor leagues:  
  \[
  \text{Difference in MSE} = \text{MSE}_{\text{major}} - \text{MSE}_{\text{minor}}
  \]

- **Significance Level (\(\alpha\))**: 0.05  
  This commonly used threshold balances the risk of Type I and Type II errors.

---

## Results

- **Observed Difference in MSE**: -21.25  
  This indicates that the model performs slightly better for minor leagues compared to major leagues.

- **P-Value**: 0.761  
  The p-value is greater than the significance level (\(\alpha = 0.05\)).

---

## Conclusion

At a significance level of 0.05, we **fail to reject the null hypothesis**. This means there is no sufficient evidence to suggest that the model performs differently for matches in major leagues versus minor leagues. The observed difference in MSE is likely due to random variation.

---

## Justification for Choices

1. **Choice of Hypotheses**:  
   The hypotheses directly address the fairness of the model's performance across two meaningful groups: major and minor leagues. This comparison is relevant because playstyles and competitiveness differ significantly between these groups.

2. **Choice of Test Statistic**:  
   MSE is an appropriate evaluation metric for regression models. Comparing the difference in MSE quantifies the model’s fairness across groups.

3. **Choice of Significance Level (\(\alpha = 0.05\))**:  
   This level is a standard threshold for determining statistical significance, suitable for exploratory data analysis.

4. **Permutation Test**:  
   The permutation test is non-parametric, making no assumptions about the distribution of errors. This ensures a robust comparison of model performance across the two groups.

# Framing a Prediction Problem

The baseline prediction problem aims to estimate the **gold difference at 15 minutes (`goldat15`)** in a professional League of Legends match. This is a **regression problem**, as the response variable, `goldat15`, is continuous and numeric.

---

### Response Variable: `goldat15`

**Description**:  
The `goldat15` column represents the gold difference between the two teams at the 15-minute mark in a match. Positive values indicate a lead for one team, while negative values indicate a deficit.

**Why It Was Chosen**:  
Gold difference at 15 minutes is an essential metric in League of Legends, reflecting a team’s performance during the early game. Understanding and predicting this value provides insights into early-game dynamics, which significantly influence the outcome of the match.

---

### Evaluation Metric

The baseline model is evaluated using the **Mean Squared Error (MSE)**.

#### Why MSE Was Chosen:
1. **Focus on Large Errors**:  
   MSE penalizes larger errors more than smaller ones, highlighting instances where the model's prediction significantly deviates from actual values.
   
2. **Continuous Nature of the Target**:  
   Since `goldat15` is a numeric value, MSE is a suitable metric for measuring prediction accuracy.

3. **Interpretability**:  
   MSE provides a clear measure of the average squared error, making it easy to compare the baseline model to future iterations.

#### Alternative Metrics:
- **Mean Absolute Error (MAE)**: Measures the average magnitude of errors without penalizing large deviations.
- **R² Score**: Indicates the proportion of variance in `goldat15` explained by the model.

For the baseline model, MSE is prioritized due to its sensitivity to significant prediction errors, which are critical for understanding the model’s initial limitations.

---

### Features Used for the Baseline Model

The baseline model uses the following features, all available at the **time of prediction** (at or before the 15-minute mark in the game):

1. **`gamelength_minutes`**:
   - **Description**: Game length in minutes, converted from seconds.
   - **Justification**: Provides context for game pacing, helping to normalize predictions for matches with varying durations.

2. **`league`**:
   - **Description**: The professional league in which the match was played (e.g., LCK, LCS).
   - **Justification**: Different leagues exhibit distinct playstyles, which can influence early-game dynamics and gold accumulation.

These features were chosen for simplicity and availability, making them ideal for a baseline model.

---

### Time of Prediction Justification

The baseline model adheres to the principle of only using information available at the **time of prediction**:
- `league` is a static feature known before the match begins.
- `gamelength_minutes` can be calculated during or immediately after the match duration is recorded.

This ensures the baseline model provides realistic predictions without relying on future information.

---

### Summary

The baseline model predicts `goldat15` using two features: game length (in minutes) and league. It uses the Mean Squared Error (MSE) as the primary evaluation metric to emphasize the model’s ability to minimize large errors. This simple model serves as a foundation for further improvements in feature engineering and hyperparameter tuning.

# Baseline Model

### **Model Overview**
The model used is a **Random Forest Regressor**, implemented as part of a scikit-learn pipeline. This model predicts the **gold difference at 15 minutes (`goldat15`)** in professional League of Legends matches. The pipeline integrates preprocessing steps for feature scaling and encoding, as well as the Random Forest algorithm for regression.

---

### **Features in the Model**

The model uses the following features:

1. **`gamelength_minutes`** (Quantitative):
   - **Description**: Game length in minutes, derived from converting the `gamelength` column from seconds to minutes.
   - **Processing**: Standardized using `StandardScaler` to ensure the feature has a mean of 0 and unit variance.

2. **`league`** (Nominal):
   - **Description**: The professional league in which the match was played (e.g., LCK, LCS).
   - **Processing**: One-hot encoded using `OneHotEncoder` to transform categorical values into binary columns.

---

### **Feature Summary**
- **Quantitative Features**: 1  
  - `gamelength_minutes`
- **Nominal Features**: 1  
  - `league` (encoded into multiple binary columns through one-hot encoding)
- **Ordinal Features**: 0  
  - No ordinal features were included in this model.

---

### **Performance of the Model**

The model's performance metrics are as follows:

- **Mean Absolute Error (MAE)**: 6107.71  
  Indicates the average absolute error between the predicted and actual `goldat15` values. This value shows that, on average, the model's predictions deviate by about 6107 gold.

- **Mean Squared Error (MSE)**: 69,533,586.71  
  Reflects the average squared error between predictions and actual values. The large value suggests significant errors in some predictions.

- **R² Score**: -0.18  
  A negative \(R^2\) score indicates that the model performs worse than a simple mean-based prediction. This suggests the model struggles to capture meaningful patterns in the data.

#### **Sample Predictions**
| **Predicted** | **Actual** |
|---------------|------------|
| 10,232.94     | 3,634.00   |
| 8,367.01      | 4,448.00   |
| 10,804.80     | 6,443.00   |
| 6,414.96      | 25,495.00  |
| 8,007.16      | 5,625.00   |

The sample predictions show consistent underestimation for high `goldat15` values, highlighting potential issues in the model’s ability to handle extreme cases.

---

### **Is the Model "Good"?**

The current model is **not "good"** based on the following observations:

1. **Negative \(R^2\) Score**:  
   A negative \(R^2\) value suggests the model fails to capture meaningful relationships in the data, performing worse than a simple mean-based prediction.

2. **High Error Metrics**:  
   Both MAE and MSE are large, indicating significant deviations between predictions and actual values.

3. **Failure on Extreme Values**:  
   Sample predictions show that the model consistently underestimates high `goldat15` values, suggesting it struggles with capturing patterns in extreme cases.


# Final Model

### **Features Added**
In addition to the baseline features (`gamelength_minutes` and `league`), the following features were added to improve the model:

1. **`kills_per_minute`**:
   - **Description**: The total number of kills divided by game length in minutes.
   - **Justification**: Early-game aggression, represented by kills, is a strong indicator of a team’s ability to secure a lead. Normalizing by game length ensures this feature is comparable across matches of varying durations.

2. **`gold_diff_per_minute`**:
   - **Description**: The normalized gold difference at 15 minutes, calculated by dividing `golddiffat15` by game length in minutes.
   - **Justification**: Early gold differences are crucial for understanding which team is performing better. Normalizing by game length ensures that differences reflect pacing rather than the absolute value of `goldat15`.

These features align with the **data-generating process** of professional League of Legends matches, where early-game aggression and gold control are critical factors influencing outcomes. By incorporating these features, the model better captures the relationships between early-game dynamics and the target variable (`goldat15`).

---

### **Modeling Algorithm and Hyperparameters**

**Modeling Algorithm**:  
The model used is a **Random Forest Regressor**, which is a robust ensemble learning method that performs well with structured, tabular data.

**Best Hyperparameters**:  
The following hyperparameters were selected using **manual grid search** and validated using Mean Squared Error (MSE) as the evaluation metric:
- **`n_estimators`**: 300  
  (Number of trees in the forest; higher values improve stability and accuracy.)
- **`max_depth`**: 10  
  (Limits the depth of the trees to prevent overfitting.)
- **`min_samples_split`**: 10  
  (Specifies the minimum number of samples required to split a node, enhancing generalization.)

**Hyperparameter Selection Method**:  
The model was tuned using a **manual grid search** over the hyperparameter space. Progress was tracked using `tqdm` to ensure visibility during the process. The final model was chosen based on the lowest Mean Squared Error on the test set.

---

### **Performance Comparison: Final vs. Baseline**

#### **Baseline Model Performance**:
- **Mean Absolute Error (MAE)**: 6107.71  
- **Mean Squared Error (MSE)**: 69,533,586.71  
- **R² Score**: -0.18  

#### **Final Model Performance**:
- **Mean Absolute Error (MAE)**: 1613.10  
- **Mean Squared Error (MSE)**: 12,477,583.71  
- **R² Score**: 0.79  

#### **Improvement Analysis**:
1. **Reduction in MAE**:  
   The MAE decreased significantly, from 6107.71 to 1613.10, indicating more precise predictions on average.

2. **Reduction in MSE**:  
   The MSE decreased dramatically, highlighting fewer large errors in predictions.

3. **Positive R² Score**:  
   The \(R^2\) score improved from -0.18 to 0.79, demonstrating that the final model explains 79% of the variance in `goldat15`, compared to the baseline model, which performed worse than a mean-based prediction.

These improvements suggest that the additional features and optimized hyperparameters significantly enhanced the model’s ability to predict `goldat15`.

---

# Fairness Analysis
I compared the predictive performance for `goldat15` between major league (Group X) and non-major league games (Group Y) using Mean Squared Error (MSE) as the evaluation metric. Our null hypothesis (\(H_0\)) stated that there is no difference in MSE between the groups (\(\text{MSE}_{\text{major}} - \text{MSE}_{\text{minor}} = 0\)), while the alternative hypothesis (\(H_A\)) suggested a difference. The observed test statistic (difference in MSE) was **3,179,522.84**, and the \(p\)-value from a permutation test with 100 iterations was \(p = 0.0\). Since \(p < 0.05\), we rejected \(H_0\), concluding that the model's predictive performance significantly differs between major and non-major leagues, with worse performance in major leagues.