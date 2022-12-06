# Labeling

In the chapter 2, we discussed how to produce a matrix X financial features out of an
unstructured dataset. Unsupervised learning algorithms can learn the patterns from
that matrix X, for example whether it contains hierarchical clusters. On the other hand,
supervised learning algorithms require that the rows in X are associated with an array
of labels or values y, so that those labels or values can be predicted on unseen features
samples. In this chapter we will discuss ways to label financial data. 也就是说，我们需要找到一个标记
金融数据的方法。

Fixed-time horizon method

snippet daily volatility estimates

~~~python
def getDailyVol(close, span0=100):
    df0 = close.index.searchsorted(close.index - pandas.Timedelta(days=1))
    df0 = df0[df0>0]
    df0=pd.Series(close.index[df0–1], index=close.index[close.shape[0]-df0.shape[0]:])
    df0=close.loc[df0.index]/close.loc[df0.values].values-1 # daily returns
    df0=df0.ewm(span=span0).std()
    return df0
~~~