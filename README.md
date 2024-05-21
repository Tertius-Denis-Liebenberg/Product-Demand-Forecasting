# Product Demand Forecasting

This project focuses on analyzing historical product demand data and forecasting future demand for the top 10 products using ARIMA models.

## Project Overview

This project includes the following steps:
1. Loading and preprocessing the data.
2. Selecting the top 10 products based on demand.
3. Plotting the demand for these products over time.
4. Forecasting future demand using ARIMA models.

## Requirements

- Python 3.x
- pandas
- matplotlib
- statsmodels

## Installation

To install the necessary libraries, run:

```bash
pip install pandas matplotlib statsmodels
```

## Data

The dataset used in this project is Historical Product Demand Changed.csv, which contains the following columns:

- **Date**: The date of the demand record.
- **Product_Code**: The code of the product.
- **Order_Demand**: The demand for the product on that date.

## Code Explanation

### Loading and Preprocessing the Data

First, we load the data using pandas and convert the `Date` column to datetime format. We then set the `Date` column as the index.

```python
    import pandas as pd

    # Load the data
    df = pd.read_csv('Historical Product Demand Changed.csv')

    # Convert the 'Date' column to datetime format
    df['Date'] = pd.to_datetime(df['Date'])

    # Set the 'Date' column as the index
    df.set_index('Date', inplace=True)
```

### Selecting the Top 10 Products

We identify the top 10 products based on their demand and filter the data to include only these products.

```python
    # Select the top 10 products
    top_10_products = df['Product_Code'].value_counts().index[:10]

    # Filter the data for these products
    df_top_10 = df[df['Product_Code'].isin(top_10_products)]
```

### Plotting the Demand Over Time

We plot the demand for the top 10 products over time.

```python
import matplotlib.pyplot as plt

# Plot the demand for these products over time
df_top_10.groupby('Product_Code')['Order_Demand'].plot()
plt.xlabel('Date')
plt.ylabel('Order Demand')
plt.title('Demand for Top 10 Products Over Time')
plt.legend(top_10_products)
plt.show()
```

### Forecasting Demand with ARIMA

We use the ARIMA model to forecast the demand for each of the top 10 products for the next quarter (12 steps ahead).

```python
from statsmodels.tsa.arima.model import ARIMA

# Forecast the demand for each product
for product in top_10_products:
    df_product = df_top_10[df_top_10['Product_Code'] == product]
    model = ARIMA(df_product['Order_Demand'], order=(5,1,0))
    model_fit = model.fit()
    forecast = model_fit.forecast(steps=12) # Forecast for the next quarter
    print(f'Forecast for product {product}:\n{forecast}\n')
```

## Author

**Tertius Denis Liebenberg**  

For any questions or feedback, please contact [tertiusliebenberg7@gmail.com].