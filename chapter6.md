# Chapter 6
Three sources of errors
1. Bias:
2. Variance:
3. Noise:

$$
E[(y_i - \hat f[x_i])^2] = (E[\hat f[x_i] - f[x_i]])^2 + V[\hat f[x_i]] + \sigma^2
$$

Bootsrap aggregation
1. Generate N training datasets by random sampling with replacement
2. Fit N estimators, one on each training set
3. Ensemble forecast is the simple average of the individual forecasts from N models
4. In the case of categorical variables, the probability that an observation belongs to a class is given by the proportion of estimators that classify that that observation as member of that class (major voting)

Variance Reduction:
$$
V[\frac{1}{N}\sum_{i=1}^N\phi_i[c]] = \frac{1}{N^2}
$$

Improved Accuracy
$$
P[X > \frac{N}{K}] = 1 - P[X \leq \frac{N}{K}]
$$
