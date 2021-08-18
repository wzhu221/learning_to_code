# The Comprehensive Use of Machine Learning Models

## General settings and import packages

```python
## Allow image to be output to a separate window.
# %matplotlib qt
# ------------------------------------------------------------------
### Import packages and modules ###
# ------------------------------------------------------------------
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import xgboost # for xgboost model training
import tensorflow as tf # for deep learning model training
import keras # for deep learning model training
import shap # for model interpretation (global)
import lime # for model interpretation (case-wise)
# ------------------------------------------------------------------
from sklearn.metrics import confusion_matrix, accuracy_score, roc_auc_score, plot_roc_curve
from sklearn.preprocessing import LabelEncoder, StandardScaler, MinMaxScaler
from sklearn.model_selection import train_test_split 
from sklearn.decomposition import PCA
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis as LDA
from sklearn.neighbors import KNeighborsClassifier as knn
from sklearn.naive_bayes import GaussianNB as gnb
from sklearn import svm
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import cross_val_score
from sklearn.cross_decomposition import PLSRegression
# ------------------------------------------------------------------
from xgboost import XGBClassifier
# ------------------------------------------------------------------
from keras.models import Sequential
from keras.layers import Dense
from keras.models import load_model
from keras.utils import to_categorical
from keras.regularizers import l2
# ------------------------------------------------------------------
import lime.lime_tabular
```



































