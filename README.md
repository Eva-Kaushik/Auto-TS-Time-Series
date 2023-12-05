# Auto-TS-Time-Series
## AutoTS Forecasting

## Overview

The AutoTS Forecasting project is a Python implementation leveraging the `autots` library for streamlined time series forecasting. This tool automates the process of generating accurate forecasts with minimal coding effort. The primary focus is on predicting future values in time series data to facilitate well-informed decision-making.

## Key Features

- **Automatic Time Series Forecasting:** Utilize the `autots` library to automate the generation of time series forecasts.
  
- **Forecast Visualization:** Visualize forecast results for different categories, providing a clear understanding of predicted trends.

- **Customizable Forecasting Model:** The code includes the creation of a forecasting model with various customizable parameters, allowing users to tailor the model to their specific needs.

## Usage

1. **Import Required Libraries:**

    ```python
    from autots import AutoTS
    import pandas as pd
    import numpy as np
    ```

2. **Load Time Series Data:**

    ```python
    # Example: Loading data from a CSV file
    df_all = pd.read_csv(r"C:\\Users\\rmehrotra\\Downloads\\temp_data.csv")
    df_all['Date'] = pd.to_datetime(df_all['Date'], format="%d-%m-%Y")
    ```

3. **Create Forecasting Model:**

    ```python
    model = AutoTS(
        forecast_length=15,
        frequency='infer',
        prediction_interval=0.9,
        ensemble='auto',
        model_list="fast_parallel",  # Options: "superfast", "default", "fast_parallel"
        transformer_list="fast",  # Options: "superfast",
        drop_most_recent=1,
        max_generations=10,
        num_validations=4,
        validation_method="backwards"
    )

    model = model.fit(
        df_all[df_all.Date < dt],
        date_col='Date' if long else None,
        value_col='Value' if long else None,
        id_col='Category' if long else None,
    )
    ```

4. **Generate Predictions:**

    ```python
    prediction = model.predict()
    ```

5. **Access Forecast Values:**

    ```python
    def getfcast_value(cat, d):
        try:
            ret_val = round(prediction.forecast.loc[d.strftime("%Y-%m-%d")][cat], 3)
            return ret_val
        except Exception as e:
            print(e)
            return np.NaN

    df_all['f1'] = df_all.apply(lambda x: getfcast_value(x['Category'], x['Date']) if x['Date'] >= dt else np.NaN, axis=1)
    ```

6. **Visualize Forecast Results:**

    ```python
    for cat in df_all.Category.unique():
        df_all[(df_all.Category == cat) & (df_all.Date > '2022-01-01')].set_index('Date').drop('Category', axis=1).plot(
            title="Category : " + cat, figsize=(12, 3)
        )
    ```

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/your-repo.git

2. Install the required dependencies:
    ```bash
   pip install autots pandas numpy
3. Run the script

