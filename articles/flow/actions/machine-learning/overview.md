# Machine Learning overview

Flow includes built-in machine learning actions for two common business problems: forecasting future values for time-varying metrics, and detecting anomalies in tabular datasets. Both actions run on the Forecast ML service and accept the same input types — `byte[]`, `Stream`, `DataTable`, or `IDataReader` — typically supplied by a file read action (CSV or Parquet from [OneDrive](../onedrive/overview.md), [Azure Blob Storage](../azure-blob-storage/overview.md), or similar) or a database query. Results are returned as files in CSV or Parquet format.

[Prophet forecast](./prophet-forecast.md) is based on the [Prophet](https://facebook.github.io/prophet/) algorithm developed by Meta. Use it to predict future values for business metrics that change over time, such as sales, demand, or operating costs. Prophet handles seasonal patterns, missing data, public holidays (by country code), custom events, and produces uncertainty bands alongside each forecast. It can also generate separate forecasts per product, region, or cost center from a single dataset. For best results, provide at least one full seasonal cycle of history (for example, 24+ months when forecasting monthly figures).

[Anomaly detection](./anomaly-detection.md) is based on the [Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html) algorithm. Use it to flag unusual rows in a numeric dataset — fraudulent transactions, sensor readings outside expected ranges, data entry errors, or unusual demand patterns. Each anomalous row is returned with an anomaly score and a SHAP-based explanation that names the features driving the classification, so reviewers can act on the result without reverse-engineering the model.

## Explore

#### Prophet forecast
Generates time series forecasts with seasonality, holidays, and uncertainty bands. Supports grouped forecasts across products, regions, or cost centers.
[Read more](./prophet-forecast.md)
<br/>

#### Anomaly detection
Identifies unusual rows in a numeric dataset using Isolation Forest, with a SHAP-based explanation of which features drove each classification.
[Read more](./anomaly-detection.md)
<br/>