# The Use of Non-linear Curve Fitting with `Python`

## Load packages

```python
# allow image to be output to a separate window
# %matplotlib qt
# ------------------------------------------------------------------
# Import packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit
# ------------------------------------------------------------------
```

## Define the non-linear model structure

```python
# Define the model structure
def boltzmann (x, a, b, c, d):
    return b+((a-b)/(1+np.exp((np.log(x)-c)/d)))
def invboltzmann (x, a, b, c, d):
    return np.exp(c+d*(np.log(((a-b)/(x-b))-1)))
# ------------------------------------------------------------------
# Provide starting values of coefficients as an array
p0 = np.array([1.9e8, 6.7e7, 4, -1.2])
# ------------------------------------------------------------------
```

## Read data from Excel file and fit to the model

```python
# Read the original data
raw_data = pd.read_excel('./nonlinear.xlsx', sheet_name='Sheet1')
xdata = raw_data['concentration']
ydata = raw_data['signal']
# ------------------------------------------------------------------
# Fit the model using the least squared method
popt, pcov = curve_fit(boltzmann, xdata, ydata, p0=p0, method='lm')
# ------------------------------------------------------------------
```

## Plot the fitted results

```python
# Plot fitting results
plt.plot(xdata, ydata, 'k.', label='actuals')
plt.plot(xdata, boltzmann(xdata, *popt), 'r-', # show the fitted curve as red solid curve
         label='fit: a=%5.3f, b=%5.3f, c=%5.3f, d=%5.3f' % tuple(popt)) # show the cofficients as number with three decimal places
plt.title('foo')
plt.xlabel('foo')
plt.ylabel('foo')
plt.legend()
plt.show()
```
