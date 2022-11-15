# Feature Importance

Mean Decrease Impurity
1. Mean decrease impurity (MDI) is a fast, explanatory-importance (in-sample, IS) method specific to treee-based classifiers, like RF. There are some important considerations we must keep in mind when working with MDI:
    1. Masking effects take place when some features are systematically ignored by tree-based classifiers in favor of others. In order to avoid them, set max_features=int(1) when using sklearn's RF class. In this way, only one random feature is considered per level
        1. Every Featuure is given a chance (at some random levels of some random trees) to reduce impurity
        2. 

~~~python
def featImpMDI(fit, featNames):
    df0 = {i: tree.feature_importances_ for i, tree in enuumerate(fit.estimators_)}
    df0 = pandas.DataFrame.from_dict(df0, orient='index')
    df0.columns = featNames
    df0 = df0.replace(0, np.nan)
    imp = pandas.concat({'mean':df0.mean(), 'std':df0.std()})
~~~
