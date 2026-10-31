# Ex.No: 03   COMPUTE THE AUTO FUNCTION(ACF)
Date: 31.10.2026

### AIM:
To Compute the AutoCorrelation Function (ACF) of the data for the first 35 lags to determine the model
type to fit the data.

### SOFTWARE USED:
Google Colab

### ALGORITHM:
1. Import the necessary packages
2. Find the mean, variance and then implement normalization for the data.
3. Implement the correlation using necessary logic and obtain the results
4. Store the results in an array
5. Represent the result in graphical representation as given below.
### PROGRAM:

```py

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files
from io import BytesIO

# Upload dataset
uploaded = files.upload()

# Read dataset
file_name = next(iter(uploaded))
data = pd.read_csv(BytesIO(uploaded[file_name]))

# Extract date and subscriber columns
data['Time Period'] = pd.to_datetime(
    data['Time Period'], dayfirst=True, format='mixed'
)

data['Subscribers'] = pd.to_numeric(
    data['Subscribers'], errors='coerce'
)

data = data.dropna(subset=['Time Period', 'Subscribers'])
data = data.sort_values('Time Period')

# Extract subscriber values
data = data['Subscribers'].to_numpy()
N = len(data)

# Define lags
lags = range(min(35, N))

# Mean and variance
mean = np.mean(data)
var = np.var(data)

# Normalize data
normalized = data - mean

# Pre-allocate autocorrelation values
acf_values = []

# Compute autocorrelation for each lag
for lag in lags:
    if var == 0:
        acf = 0
    else:
        num = np.sum(normalized[:N-lag] * normalized[lag:])
        den = (N - lag) * var
        acf = num / den

    acf_values.append(acf)

# Display the ACF graph
plt.figure(figsize=(10, 5))
plt.stem(lags, acf_values, basefmt=" ")
plt.xlabel("Lags")
plt.ylabel("ACF")
plt.title("Autocorrelation Function (First 35 Lags)")
plt.grid(True)
plt.show()

# Display ACF values
print("ACF values for first 35 lags:")
print(acf_values)
```


### OUTPUT:
<img width="872" height="529" alt="image" src="https://github.com/user-attachments/assets/d2d8703f-a842-4110-9d53-02a55fc1fed0" />

### RESULT:
        Thus we have successfully implemented the auto correlation function in python.
