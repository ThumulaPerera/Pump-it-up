# Pump-it-up

Github repo: https://github.com/ThumulaPerera/Pump-it-up
Index No: 170431L

Please see [Getting Started](#getting-started) section for details about running the project.

### EDA

- Exploratory Data Analysis was performed to identify the features available, missing values, unique values, outliers, coorelated features, and, features with higher potential.  
- 7 categorical columns with missing values were identified. Further, some numerical columns were also abserved to be having a large number of 0s which are most likely to be missing values as well.
- Several columns which contain mostly unique values were also identified. `id`, `wpt_name`, and `num_private` were identified as mostly unique columns based on the feature name and description. In addition, based on inspection on categorical columns, `subvillage` column was also identified as mostly unique. 
- Features `construction_year`, `funder`, `installer`, `basin`, `quality_group`, and `quantity_group` were identified as some of the features that maay have high potential.
- Features which were intuitively identified as features with high potential were further analysed with the help of countplots to identify whether there is a visble relationship with the target.
- Based on feature names and descriptions, some features were identified as related to each other. They were compared using coorelation coefficients. Based on that, several features were identified as highly correlated. eg:
   - `quantity`, and `quantity_group` were identified as complete duplicates
   - `water_quality`, and `quality_group` were highly correlated
   - `source`, and `source_type` were highly correlated
   etc.
- Features (such as `latitude`) which seemed to contain outliers were also explored to detect outlier presence.
- Complete workflow and further descriptions (including graphs) can be found in the [EDA notebook](notebooks/EDA.ipynb).

### Preprocessing

- Several preprocessing approaches were explored in the different submission attempts done.
- For categorical features `funder` and `installer`, oberving the values revelaed that there are spelling errors and capitalization errors that caused the same category to be recorded in multiple ways. Those were minimized using a an approach based Levenshtein distance ratio of similarity between words. After minimizing such errors, the top categories (i.e. categories that account for 2% of the data or more) were left and remaining categories were replaced with 'other'.
- For those 2 plus the remaining categories, missing values were imputed by replacing with most frequent category. 
- Thereafter, the categorical features were one-hot encoded.
- Outliers in selected numerical features were replaced with appropriate values
   - in `latitude`, `longitude`, and `gps_height`, outliers were replaced using the median values of regions.
   - in `population`, outliers were replaced with mean.
- Missing values in numerical colums were imputed using the mean values.
- Numerical values other than `latitude` and `longitude` were normalized using standard normalization.

### Feature Engineering

- A new feature to represent the pump age was created by mathematically fransforming the `construction_year` feature.
- New Binary features were created to represent missing values (later imputed) in `public_meeting`, `permit`, and `population`.

### Feature Selection

- Some features were disccarded as they contained mostly unique values
- For duplicate features identified in EDA, one feature from a set was kept and the others were discarded.
- For similar features identified in EDA, the generally feature with the smaller numer of categories were kept and the others were discarded.   

### Modelling

- Initially, a Random Forrest model was used.
- Later, an approach based on catboost Model was adopted since the features mostly consisted of categoricall data. This yielded an improvement in calssification rate.

### Insight Extraction

- Feature Importance of the catboost model used in attempt 3 was examined, and, `is_missing_public_meeting`, `is_missing_permit`, `is_missing_population`, `quality_group`, `source_class`, and, `public_meeting` were identified as less important to the model.

### Notebooks of the attempts made

- notebooks corresponding to the attempts made are available inside the notebooks directory.







### Getting Started

1. Clone the project locally and change to the cloned directory.

   ```bash
   git clone https://github.com/ThumulaPerera/Pump-it-up
   cd Pump-it-up
   ```

2. Install python and create a virtual environment. You can use conda, venv or any other equivalent for this.

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

   windows

   ```bash
   source .venv/Scripts/activate
   ```

3. Install the reqyurements sepcified by the `requirements.txt` file.

   ```bash
   pip install -r requirements.txt
   ```

4. Afterwards, run jupyter lab and you will find the relavent notebooks in the `notebooks` directory.
   ```bash
   jupyter lab
   ```