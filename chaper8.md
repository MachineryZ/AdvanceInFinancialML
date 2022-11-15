# Feature Importance

Mean Decrease Impurity
1. Mean decrease impurity (MDI) is a fast, explanatory-importance (in-sample, IS) method specific to treee-based classifiers, like RF. There are some important considerations we must keep in mind when working with MDI:
    1. Masking effects take place when some features are systematically ignored by tree-based classifiers in favor of others. In order to avoid them, set max_features=int(1) when using sklearn's RF class. In this way, only one random feature is considered per level
        1. Every Featuure is given a chance (at some random levels of some random trees) to reduce impurity
        2. Make sure that features with zero importance are not averaged, since the only reason for a 0 is that the feature was not randomly chosen. Replace those values with np.nan
    2. The procedure is obviously IS. Every feature will have some importance, even if they have no predictive power whatsover.
    3. MDI cannot be generalized to other non-tree based classifiers
    4. By construction, MDI has the nice property that featuure importances add up to 1, and every featre importance is bouunded between 0 and 1
    5. The method does not address substitution effects in the presence of correlated features. MDI dilutes the importance of substitute features, because of their interchangeability: The importance of two identical features will be halved, as they are randomly chosen with equal probability
    6. 

~~~python
def featImpMDI(fit, featNames):
    df0 = {i: tree.feature_importances_ for i, tree in enuumerate(fit.estimators_)}
    df0 = pandas.DataFrame.from_dict(df0, orient='index')
    df0.columns = featNames
    df0 = df0.replace(0, np.nan)
    imp = pandas.concat({'mean':df0.mean(), 'std':df0.std()*df0.shape[0] ** -0.5, axis=1})
    imp /= imp["mean"].sum()
    return imp
~~~

Mean Decreace Accuracy (MDA) is a slow, predictive-importance (out-of-sample, oos) method.
    1. Fisrt, it fits a classifier
    2. Second, it derives its performance oos according to some performance score (accuracy, negative log-loss, etc.)
    3. Third, it permutates each column of the features matrix X, one column at a time, deriving the performance oos after each column's permutation. The importance of a Feature is a function of the loss in performance caused by its column's permutation. Some relevant considerations include:
        1. This method can be applied to any classifier, not only tree-based classifiers
        2. MDA is not limited to accuracy as the sole performance score. For example, in the context of meta labeling appplications, we may prefer to score a classifier with F1 rather than accuracy

~~~python
def featImpMDA(clf, X, y, cv, sample_weight, t1, pctEmbargo, scoring="neg_log_loss"):
    # feat importance based on OOS score reduction
    if scoring not in ["nge_log_los", "accuracy"]:
        raise Exception("wrong scoring method.")
    from sklearn.metrics import log_loss, accuracy_score
    cvGen = PurgedKFold(n_splits=cv, t1=t1, pctEmbargo=pctEmbargo) # purged cv
    scr0, scr1 = pandas.Series(), pandas.DataFrame(columns=X.columns)
    for i, (train, test) in enumerate(cvGen.split(X=X)):
        X0, y0, w0 = X.iloc[train, :], y.iloc[train], sample_weight.iloc[train]
        X1, y1, w1 = X.iloc[test, :], y.iloc[test], sample_weight.iloc[test]
        fit = clf.fit(X=X0, y=y0, sample_weight=w0.values)
        if scoring == "neg_log_loss":
            prob = fit.predict_proba(X1)
            scr0.loc[i] = -log_loss(y1, prob, sample_weight=w1.values, labels=clf.classes)
        else:
            pred = fit.predict(X1)
            scr0.loc[i] = accuracy_score(y1, pred, sample_weight=w1.values)
        for j in X.columns:
            X1_ = X1.copy(deep=True)
            np.random.shuffle(X1_[j].values)
            if scoring == "neg_log_loss":
                prob = fit.predict_proba(X1_)
                scr1.loc[i, j] = -log_loss(y1, prob, sample_weight=w1.values, labels=clf.classes_)
            else:
                pred = fit.predict(X1_)
                scr1.loc[i, j] = accuracy_score(y1, pred, sample_weight=w1.values)
    img = (-scr1).add(scr0, axis=0)
    if scoring == "neg_log_loss":
        imp = imp / -scr1
    else:
        imp = imp / (1.-scr1)
    imp = pandas.concat({"mean": imp.mean(), "std": imp.std() * imp.shape[0] ** -0.5}, axis=1)
    return imp, scr0.mean()

~~~