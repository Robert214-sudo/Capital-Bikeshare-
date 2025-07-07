# Capital Bikeshare Demand Forecasting & Resource Allocation
This project is a predictive modeling exercise using Capital Bikeshare data from Feb-Apr 2024. The goal is to build models that forecast daily bike pickups and drop-offs at the "22nd &amp; H St NW" station, leveraging features like weather, day-of-week, and trip data to optimize bike/dock allocation. Includes Jupyter notebook and detailed report. 

## Basic Information
* Person or organization developing model: Robert Bhero, robert.bhero@gwu.edu 
* Model date: June 2025
* Model version: 1.0
* License: MIT
* Model implementation:
## Intended Use
* Primary intended uses: This project serves as an educational example of applying machine learning models to forecast daily bike pickups and drop-offs at Capital Bikeshare's "22nd & H St NW" station, using features like weather, day-of-week, and trip data from Feb-Apr 2024. It is intended to demonstrate the comparison and application of various predictive models, such as Random Forest, Neural Network, KNN and Regression Tree, for optimizing resource allocation in urban transportation systems.
* Primary intended users: Students and those that want to learn about modeling
* Out of scope use cases: Any use beyond an educational example is out of scope
## Training Data
*Data Dictionary*

| Column Name         | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| `tavg`, `tmin`, `tmax` | Daily average, min, and max temperature (°F)                             |
| `wspd`, `pres`      | Wind speed and pressure readings                                            |
| `weekday`, `month`  | Encoded time indicators (0–6 for day of week; 1–12 for month)               |
| `holiday`, `workingday` | Binary indicators for federal holidays and non-weekend workdays         |
| `PU_ct`, `DO_ct`    | Target variables: Pickups and Drop-offs at station                          |
* Source of training data: Capital Bikeshare and NOAA weather datasets (https://opendata.dc.gov/datasets/DCGIS::capital-bikeshare-locations, )  
* Contact: robert.bhero@gwu.edu for more information

## Data Splitting

* How data was split:  
  Separate models were trained and tuned for `PU_ct` and `DO_ct` using an 80/20 train-test split.

* Approximate row count:  
  - Training rows: ~250  
  - Validation rows: ~60

## Test Data

- The original dataset was split, so no external test file was used.  
- Performance was assessed on a **held-out 20% validation set**, using both **RMSE** and a **custom business cost function**.

## Model Details

### Inputs to Final Model

| Feature              | Purpose                                              |
|----------------------|------------------------------------------------------|
| `tavg`, `tmin`, `tmax` | Weather-driven predictors of bike usage            |
| `wspd`, `pres`       | Additional weather context                          |
| `weekday`, `month`   | Captures seasonality and weekly travel behavior     |
| `holiday`, `workingday` | Captures usage shifts on holidays and weekends   |

### Model Overview

- **Target columns:** `PU_ct`, `DO_ct`  
- **Model type:** Random Forest Regressor  
- **Library:** Scikit-learn  
- **Cross-validation:** 5-fold  
- **Hyperparameters (optimized via GridSearchCV):**  
  - `n_estimators`: 100  
  - `max_depth`: 8–10  
  - `min_samples_split`: 2  
  - `random_state`: 42  

## 📈 Evaluation Metrics

- **Root Mean Squared Error (RMSE)**  
  - PU model RMSE: ~2.91  
  - DO model RMSE: ~3.18

- **Business Cost Function:**  
  Used a penalty-based custom cost function:
  - Over-prediction penalty: \$5 per unused bike
  - Under-prediction penalty: \$20 per missing bike

- **Selected Model:**  
  Random Forest minimized both RMSE and the **average cost penalty**, making it the best operational choice.

## Ethical Considerations

- **Data Bias:** Forecasts based on historical weather and usage may not reflect unexpected policy, event, or infrastructure changes.
- **Equity Risk:** Over-optimization for high-traffic stations may result in poor service for less active areas unless scaled responsibly.
- **Temporal Fragility:** Models assume similar future patterns to training periods — large deviations (e.g., COVID-era behavior) may lead to failure.


## Limitations

- Small dataset (limited to one station and time window)
- Did not incorporate real-time rebalancing or multi-station dynamics
- No use of external signals like special events or ride promotions
- Static model — does not update over time unless retrained


## What I Learned

- How to evaluate regression models both statistically and financially
- How to engineer weather/time features for mobility demand
- How to frame business tradeoffs as a cost function
- How to interpret model results for operational decisions


## Future Improvements

- Expand to multiple stations with clustering or spatial models
- Add dynamic rebalancing logic with reinforcement learning
- Deploy as a Streamlit dashboard for daily forecasting
- Use real-time weather APIs for live demand prediction
