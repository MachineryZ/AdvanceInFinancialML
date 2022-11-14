<<<<<<< HEAD
# Financial Data Structures

这章的目的在于，将 unstructured financial data 整理成想要的形式，方便以后的处理

1. Essential types of financial data：
    1. Fundamental Data 基本面数据
        1. 表格类型的基本面数据，比如Asset，liability，cost/earnings，macro variables
        2. 这里要注意的是 backfilled 和 reinstated 的概念。前者表示 missing value 会被分配一个预先设置，后者表示修改后的正确数据
    2. Market Data
        1. CME chicago mercantile exchange
        2. BWIC bids wanted in competition

Bars
1. Standard Bars
    1. The purpose of bar methods is to transform a series of observations that arrive at irregular frequency into a homogeneous series
    2. Time bars, volume weighted average price (VWAP); Open Price; Close price; High pricel; Low price; Volume traded
    3. Markets do not process information at a constant time interval; The hour following the oipen is much more active than the hour around nooon; Time bars oversample information during low-activity periods and undersample information during high-activity periods
    4. Time-
