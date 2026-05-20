# Anomaly Detection

The **Anomaly Detection** node identifies unusual data points in tabular numeric datasets using the [Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html) algorithm. Each detected anomaly is accompanied by a SHAP-based explanation that describes which features contributed most to the anomalous classification.

## Use cases

- Detecting fraudulent transactions or outlier financial entries
- Flagging sensor readings that deviate from expected operating ranges
- Identifying data quality issues or data entry errors in large datasets
- Spotting unusual demand patterns or inventory movements

## Example

A dataset of sales transactions is uploaded. The node runs Isolation Forest on the `quantity` and `unit_price` columns. The result contains only the rows classified as anomalous, with an `anomaly_score` indicating how strongly anomalous each row is and an `anomaly_explanation` summarising which features drove the classification.

## Prerequisites

- A configured **Forecast ML service** connection in the Flow solution.
- Input data in **CSV** or **Parquet** format.
- All feature columns must be **numeric** (non-numeric values will cause the job to fail).

## Properties

| Name | Required | Description |
|---|---|---|
| Input contents | Yes | The data to analyse. Accepts `byte[]`, `DataTable`, `IDataReader`, or `Stream`. |
| Input file format | Conditional | Required when **Input contents** is `byte[]` or `Stream`. Set to `csv` or `parquet` to tell the service how to parse the raw bytes. |
| Output file format | Yes | Format of the result file returned by the service. `csv` or `parquet`. |
| Feature columns | Yes | Comma-separated list of column names to use for anomaly detection. Only these columns are passed to the model; all other columns are preserved in the output. |
| Number of estimators | Yes | Number of trees in the Isolation Forest ensemble. Defaults to `100`. Higher values improve accuracy at the cost of longer run time. |
| Contamination | No | Expected proportion of anomalies in the dataset. Must be in the range `(0, 0.5)`. Leave empty to use `"auto"`, which lets the algorithm infer a threshold from the data. |
| Return variable name | Yes | Name of the C# variable that will hold the result (`byte[]`). Must be a valid C# identifier. |

## Input contents

The node accepts four input types:

| Type | Notes |
|---|---|
| `byte[]` | Raw file bytes. **Input file format** must be set. |
| `Stream` | File stream. **Input file format** must be set. |
| `DataTable` | Automatically serialised to Parquet before upload. **Input file format** is not required. |
| `IDataReader` | Automatically serialised to Parquet before upload. **Input file format** is not required. |

> [!IMPORTANT]
> All columns listed in **Feature columns** must exist in the input file and contain numeric data. The node validates column presence before running the model and will fail with a descriptive error if any feature column is missing.

## Output data

The result contains **only the rows classified as anomalies**. All original columns from the input are preserved, and two additional columns are appended:

| Column | Type | Description |
|---|---|---|
| `anomaly_score` | float | Anomaly score from the Isolation Forest model. Higher values indicate a stronger deviation from normal. |
| `anomaly_explanation` | string | SHAP-based feature explanation. Lists each feature with its value, whether it was above or below the dataset mean, and its SHAP impact score. Features are sorted by absolute impact, descending. |

If no anomalies are detected, the result is an **empty dataset** with the full column set (including `anomaly_score` and `anomaly_explanation`).

##### Example anomaly_explanation value
```
quantity: 9500 (above normal, impact +0.3812); unit_price: 0.01 (below normal, impact +0.1204)
```

## Configuration

### Feature columns

Specify the column names the model should use as input features, separated by commas:

```
quantity, unit_price, discount
```

Only numeric columns should be listed. The model fits exclusively on these columns, but all columns in the source data are included in the output.

### Contamination

Controls the decision threshold used to classify a row as anomalous.

| Value | Behaviour |
|---|---|
| Empty (default) | `"auto"` — threshold is derived from the training data distribution |
| `0.05` | Approximately 5 % of rows will be flagged as anomalies |
| `0.1` | Approximately 10 % of rows will be flagged |

Setting an explicit value is useful when you have domain knowledge about the expected anomaly rate. Values must be in the **open interval (0, 0.5)** — zero and 0.5 are not allowed.

### Number of estimators

The number of decision trees built by the ensemble. The default of `100` is suitable for most datasets. Increase this value for large, high-dimensional datasets where a more stable score distribution is needed.

## Returns

The node returns the result as `byte[]`. Bind it to a variable using **Return variable name**, then pass it downstream to a file-write node, a database loader, or another processing step.

The bytes are encoded in the format chosen by **Output file format** (`csv` or `parquet`).