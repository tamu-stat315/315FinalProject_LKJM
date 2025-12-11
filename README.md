# Sleep Health and Lifestyle Analysis - Final Project

A comprehensive data science project analyzing how lifestyle habits, demographic characteristics, and health indicators affect sleep quality and sleep disorders.

**Authors:** Jyo Madhavarapu and Likith Kancharlapalli

## Project Overview

This project explores the relationships between lifestyle factors (stress, physical activity, BMI), demographic characteristics (age, gender, occupation), and sleep health outcomes. We use exploratory data analysis (EDA) and machine learning classification to predict sleep disorders (None, Insomnia, Sleep Apnea).

### Key Research Questions

1. Which lifestyle factors contribute to sleep disorders?
2. Does occupation correlate with sleep quality?
3. Does physical activity improve sleep quality?
4. Which measurable factors predict sleep quality?
5. How do age and gender impact sleep?
6. Which sleep disorders are easier to predict?
7. Which ML model performs best?
8. How well do models generalize?

## Dataset

**Source:** Sleep Health and Lifestyle Dataset (Kaggle)
- **Size:** 374 observations, 13 features
- **Type:** Synthetic/simulated data for academic purposes
- **Citation:** Tharmalingam, L. (2023). Sleep health and lifestyle dataset (Version 2) [Data set]. Kaggle.

### Key Variables

- **Target:** Sleep Disorder (None, Insomnia, Sleep Apnea)
- **Outcome:** Quality of Sleep (1-10 subjective rating)
- **Predictors:** Sleep Duration, Stress Level, Physical Activity Level, Daily Steps, BMI Category, Blood Pressure, Heart Rate, Age, Gender, Occupation

## Project Structure

```
sleep-health-analysis/
├── data.csv                      # Original dataset
├── sleep_analysis.ipynb         # Main analysis notebook
├── requirements.txt             # Python dependencies
├── Dockerfile                   # Docker configuration
└── README.md                    # This file
```

## Technologies Used

- **Data Processing:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Machine Learning:** Scikit-learn, XGBoost
- **Version Control:** GitHub
- **Containerization:** Docker

## Installation & Setup

### Option 1: Local Installation

#### Prerequisites

- Python 3.8 or higher
- pip or conda

#### Steps

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd sleep-health-analysis
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Ensure `data.csv` is in the project directory

5. Run Jupyter Notebook:
   ```bash
   jupyter notebook sleep_analysis.ipynb
   ```

### Option 2: Docker Installation (Recommended)

#### Prerequisites

- Docker installed on your system

#### Steps

1. Build the Docker image:
   ```bash
   docker build -t sleep-health-analysis:latest .
   ```

2. Run the Docker container:
   ```bash
   docker run -it --name sleep-analysis \
     -v $(pwd)/data.csv:/workspace/data.csv \
     -p 8888:8888 \
     sleep-health-analysis:latest
   ```

3. Access Jupyter in your browser:
   - The terminal will display a URL like: `http://127.0.0.1:8888/?token=...`
   - Copy and paste this URL into your browser

4. Open `sleep_analysis.ipynb` and run all cells

## Analysis Workflow

### 1. Data Cleaning & Preparation
- Checking for missing values (none found in this dataset)
- Feature engineering:
  - Blood Pressure split into Systolic/Diastolic
  - Creation of categorical variables (BPCategory, ActivityCategory, StepsCategory, OverallActivity)
- Ensures consistent data types and formats

### 2. Exploratory Data Analysis (EDA)

Four main visualizations:

1. **Impact of Lifestyle Factors** - Chi-square and ANOVA tests showing stress, activity, and BMI significantly relate to sleep disorders
2. **Occupation & Sleep Quality** - Heatmap revealing engineers report highest sleep quality, sales representatives lowest
3. **Measurable Predictors** - Regression plots and boxplots identifying sleep duration and stress as strong predictors
4. **Age & Gender Effects** - Line plots and boxplots showing modest age/gender differences in sleep patterns

### 3. Predictive Modeling

#### Models Evaluated
- **Logistic Regression** (multinomial)
- **Random Forest Classifier**
- **XGBoost Classifier**

#### Preprocessing Pipeline
- Numeric features: Median imputation + StandardScaler
- Categorical features: Mode imputation + One-Hot Encoding
- Target encoding: LabelEncoder (Insomnia=0, None=1, Sleep Apnea=2)

#### Model Selection Process
- 5-fold Stratified Cross-Validation
- Performance metric: Macro F1 Score (treats all classes equally)
- Hyperparameter tuning via GridSearchCV for Random Forest

#### Results
| Model | CV F1_macro | Test Accuracy | Test F1_macro |
|-------|------------|---------------|--------------|
| Logistic Regression | 0.860 (±0.043) | - | - |
| Random Forest (default) | 0.832 (±0.037) | - | - |
| Random Forest (tuned) | 0.846 | 96% | 0.935 |
| XGBoost | 0.838 (±0.034) | - | - |

**Key Finding:** Logistic Regression achieved the best cross-validated performance, suggesting largely linear relationships in the data.

#### Confusion Matrix (Test Set)
- **No Sleep Disorder:** Perfect precision and recall (100%)
- **Sleep Apnea:** High recall (0.938)
- **Insomnia:** Slightly lower recall (0.867) due to symptom overlap with Sleep Apnea

## Running the Analysis

### Full Pipeline Execution

The notebook executes sequentially through:

1. Data import and cleaning (5 cells)
2. EDA and visualizations (4 figures)
3. Feature preprocessing
4. Model training and cross-validation
5. Hyperparameter tuning
6. Test set evaluation and confusion matrix
7. Performance reporting

### Typical Runtime
- Local machine: 2-5 minutes
- Docker container: 3-7 minutes

## Key Findings

### Lifestyle Factors
- All four lifestyle factors (stress, physical activity, daily steps, BMI) significantly correlate with sleep disorders (p < 0.05)
- Higher stress and lower activity predict sleep disorders
- Overweight BMI category shows increased disorder prevalence

### Occupation Impact
- Engineers: highest sleep quality (8.0/10)
- Lawyers & Accountants: high sleep quality (7.5/10)
- Sales Representatives & Scientists: lowest sleep quality (5.0-5.5/10)

### Sleep Quality Predictors (Ranked by Effect Size)
1. **Sleep Duration** (strong positive correlation)
2. **Stress Level** (strong negative correlation)
3. **Physical Activity** (mild positive correlation)
4. **Daily Steps** (minimal correlation)
5. **BMI Category** (categorical effect)
6. **Blood Pressure** (categorical effect)

### Demographic Effects
- Modest age-related variations in sleep quality
- Women show slightly higher sleep duration and quality variability
- Gender differences are less pronounced than occupational differences

## Limitations

1. **Synthetic Data:** Dataset does not represent real clinical populations; results are demonstrative only
2. **Sample Size:** N=374 limits model complexity and generalization to larger populations
3. **Missing Predictors:** Caffeine intake, work schedules, screen time, sleep tracking device data not included
4. **Class Overlap:** Insomnia and Sleep Apnea symptoms overlap, reducing classification clarity
5. **No Temporal Data:** Single cross-sectional snapshot without longitudinal follow-up
6. **Imbalanced Classes:** Potential bias toward majority classes despite stratified approaches

## Future Improvements

- Validate findings on real clinical datasets
- Include additional sensors (wearable sleep trackers, actigraphy)
- Incorporate missing lifestyle variables (caffeine, screen time, work schedule)
- Explore advanced models (SVM, neural networks, deep learning)
- Perform stratified analysis by occupation, age group, gender
- Conduct explainability analysis (SHAP values, permutation importance)
- Implement time-series analysis for longitudinal data

## Reproducibility

To ensure exact reproducibility:

- All random states are set to 42
- StratifiedKFold with shuffle=True and random_state=42
- Train-test split with stratification and random_state=42
- Docker image locks all dependency versions in `requirements.txt`

## Dependencies

See `requirements.txt` for full list. Key packages:

```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=1.0.0
xgboost>=1.5.0
jupyter>=1.0.0
```

## Contact & Attribution

**Project Team:**
- Jyo Madhavarapu
- Likith Kancharlapalli

**Resources Used:**
- Class notes
- DataCamp tutorials
- AI features on DataLab
- Markdown documentation
- Stack Overflow
- Grammarly

**Data Source:**
Tharmalingam, L. (2023). Sleep health and lifestyle dataset (Version 2) [Data set]. Kaggle. https://www.kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset

## License

[Specify your license here - e.g., MIT, CC-BY-4.0]

---

**Last Updated:** December 2024
**Project Status:** Complete - Final Version 2