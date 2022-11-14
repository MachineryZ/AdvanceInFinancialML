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
    4. Time-sampled series often exhibit poor statistical properties, like serial correlation, heteroscedasticity, and non-normality of returns.
2. Tick Bars:
    1. Sample variable listed will be extracted each time a pre-defined number of transactions takes place e.g. 1000 ticks (就是，当一定数量的 tick 到达之后，才进行 bar 的合成)
    2. Reason behind tick bars: price changes over a fixed number of transactions may have a Gauussian distribution. Price changes over a fixed time period may follow a stable Paretian distribution, whose variance is infinite
    3. When constructing tick bars, yyouu need to be aware of outliers.
    4. Tick Bars problem: order for a size of 10, we buy 10 lots, our one order will be recorded as one tick; if instead on the offer there are 10 orders of size 1, our one buy will be recorded as 10 separate transactions. In addition, matching engine protocols can fuurther split one fill into multiple artificial partial fills, as a matter of operational convenience
3. Volume Bars
    1. Volume bars circuumvent that problem by sampling every time a pre-defined amount of the security's units (shares, futuures contracts, etc.) have been exchanged.  For example, we could sample prices every time a futures contrace exchanges 1000 units, regardless of the number of ticks involved.
4. Dollar Bars
    1. Dollar bars are formed by sampling an observation every time a pre-defined market value is exchanged. 
    2. Example: 
5. Information-driven bars:
    1. in this section we will explore how to use various indices of information arrival to samle bars

1. Tick Imbalance Bars
2. Volume/Dollars Imbalance Bars
3. Tick Runs Bars
4. Volume/Dollar Runs Bars
5. 


