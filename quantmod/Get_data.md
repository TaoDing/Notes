# 1 获取数据

quantmod中从外部获取数据的途径主要有三种：

* 从网络上获取数据
* 从MySQL等数据库读入数据
* 基于read.table、read.csv等函数读取数据并转化为quantmod适宜的格式

## 1.1 从网络上获取数据

quantmod包中的getSymbols函数可以从网络上获取数据，用args()函数看一下它的用法:

```r
# 安装并载入quantmod包
install.packages("quantmod")
require(quantmod)
```

```r
> args(getSymbols)
function (Symbols = NULL, env = parent.frame(), reload.Symbols = FALSE, 
    verbose = FALSE, warnings = TRUE, src = "yahoo", symbol.lookup = TRUE, 
    auto.assign = getOption("getSymbols.auto.assign", TRUE), 
    ...) 
NULL
```

getSymbols函数各主要参数的意义如下：

参数|用途
-------|-------
Symbols|指定股票符号或者代码。
env|指定对象的创建位置。
reload.Symbols|是否在指定环境中重新载入数据，缺省设置为FALSE。
warnings|是否输出警告信息，缺省设置为TRUE。
src|指定抓取数据的网址，缺省设置为yahoo，也可以选择google。
symbol.lookup|从外部查找检索股票代码的路径。
auto.assign|是否将函数结构自动载入到工作环境。
file.path|指定文件路径的字符串。
...|其它参数。 

getSymbols函数可以从网络上常用的金融数据库中抓取各种金融数据，目前支持的数据库包括：

* yahoo
* google
* FRED
* oanda

从上面几个数据库中，可以获取上市公司的股票日交易数据、股息数据、拆股数据、财务报表数据、汇市数据、重金属交易数据以及美联储官网公布的若干经济数据。

### 1.1.1 获取股票日交易数据

getSymbols函数获取股票的日交易数据很简单。比如，想要获取中国移动通讯公司的日交易数据，可以运行下面的代码：

``` r
> getSymbols("CHL")
[1] "CHL"
> head(CHL)
           CHL.Open CHL.High CHL.Low CHL.Close CHL.Volume CHL.Adjusted
2007-01-03    45.45    46.84   45.45     46.14    3538300        35.40
2007-01-04    44.25    45.05   43.62     44.43    3210000        34.09
2007-01-05    44.99    44.99   43.12     43.24    2036300        33.17
2007-01-08    43.69    44.07   43.16     43.90    1230200        33.68
2007-01-09    42.98    42.98   41.56     41.85    2566100        32.11
2007-01-10    41.41    42.12   40.86     41.96    1987000        32.19
```

代码中的CHL是中国移动通讯公司在美国市场上的股票缩写码。同样的，可以获取苹果公司和新东方公司的日交易数据：

``` r
# 获取苹果公司的日交易数据
> getSymbols("AAPL")
[1] "AAPL"
> head(AAPL)
           AAPL.Open AAPL.High AAPL.Low AAPL.Close AAPL.Volume AAPL.Adjusted
2007-01-03     86.29     86.58    81.90      83.80   309579900         11.44
2007-01-04     84.05     85.95    83.82      85.66   211815100         11.70
2007-01-05     85.77     86.20    84.40      85.05   208685400         11.61
2007-01-08     85.96     86.53    85.28      85.47   199276700         11.67
2007-01-09     86.45     92.98    85.15      92.57   837324600         12.64
2007-01-10     94.75     97.80    93.45      97.00   738220000         13.24


# 获取新东方公司的日交易数据
> getSymbols("EDU")
[1] "EDU"
> head(EDU)
           EDU.Open EDU.High EDU.Low EDU.Close EDU.Volume EDU.Adjusted
2007-01-03    33.60    35.65   33.55     35.19    1607200         8.46
2007-01-04    35.30    35.55   34.01     34.55     902800         8.31
2007-01-05    34.65    34.96   34.00     34.06     488800         8.19
2007-01-08    34.13    34.94   34.13     34.56     290400         8.31
2007-01-09    34.86    35.05   33.60     34.00     998400         8.18
2007-01-10    33.82    35.97   33.82     35.73    1047600         8.59
```

除了上面列举的个股日交易数据之外，还可以获取各种指数的日数据。比如，可以获取一下上证指数的日数据。上证指数的符号是^SSEC，运行下面的代码可以获取上证指数的日数据：

``` r
getSymbols("^SSEC")
```

从上面几个例子中可以看到，在获取股票日交易数据之前，需要知道上市公司股票或者指数的缩写码。如果不知道，怎么办？比如，我们想获取上证 A 股指数的日交易数据或者代码为600635的股票的日交易数据，怎么办？此时可以运行下面的代码：

```
# 例子
getSymbols("600635.ss")
getSymbols("000001.ss")
```

代码中的.ss表示该公司的股票从属于上交所。类似地，可以获取上证 A 股指数（代码为 000002.ss ）、上证B股指数（代码为 000003.ss ）、上证综合指数（代码为 000008.ss ）、沪深300指数（代码为 000300.ss ）、深圳成指（代码为 399001.sz ）、三一重工（代码为 600030.ss ）等。


``` r
#例：获取上证A股指数
#上证A股指数的代码为 000002.ss
getSymbols("000002.ss")

#例：获取上证B股指数
#上证B股指数的代码为 000003.ss
getSymbols("000003.ss")

#例：获取上证综合指数
#上证综合指数的代码为 000008.ss
getSymbols("000008.ss")

#例：获取沪深300指数
#沪深300指数的代码为000300.ss。
getSymbols("000300.ss")
```

获取深交所股票数据时，把.ss替换为.sz。

``` r
#例：深圳成指
#深证成指的代码为399001.sz，其中.sz代表该指数从属于深交所。
getSymbols("399001.sz")
```

### 1.1.2 getSymbols结果的重命名


令getSymbols函数的symbols参数等于股票或者指数的代码的时候，如果不给数据命名。对其引用时十分困难。比如：

``` r
#例：获取上证A股指数
#上证A股指数的代码为 000002.sz
getSymbols("000002.sz")
```

这里无法引用000002.sz的结果。为了便于引用数据并对其进一步进行分析，可以对getSymbols的结果进行重命名。有两种方法：一是借助setSymbolLookup函数，二是借助getSymbols函数中的auto.assign参数。

```r
# 重命名000002.sz为A.Share.index
> setSymbolLookup(A.Share.index=list(name="000002.sz",src="yahoo"))
> getSymbols("A.Share.index")
[1] "A.SHARE.INDEX"
> head(A.SHARE.INDEX)
           000002.SZ.Open 000002.SZ.High 000002.SZ.Low 000002.SZ.Close
2007-01-01          15.16          15.63         15.16           15.44
2007-01-02          15.16          15.63         15.16           15.44
2007-01-03          15.16          15.63         15.16           15.44
2007-01-04          15.70          16.56         15.28           15.48
2007-01-05          15.30          15.46         14.62           14.85
2007-01-08          14.78          15.37         14.51           15.29
           000002.SZ.Volume 000002.SZ.Adjusted
2007-01-01                0               5.67
2007-01-02                0               5.67
2007-01-03                0               5.67
2007-01-04        180286800               5.69
2007-01-05        134704000               5.45
2007-01-08        181871200               5.62
```


```r
#例子
setSymbolLookup(CSI300=list(name="000300.ss",src="yahoo"))
getSymbols("CSI300")
```



### 1.1.3 获取上市公司股息数据

getDividends函数可以获取上市公司的股息数据。

```{r}
getDividends("CHL")
```

根据股息调整股票价格

adjustOHLC函数可以对股票数据进行除息调整。

```{r}
getSymbols("CHL", from="1990-01-01", src="yahoo")
head(CHL)
head(CHLL.a <- adjustOHLC(CHL))
head(CHL.uA <- adjustOHLC(CHL, use.Adjusted=TRUE))
```

### 1.1.4 获取上市公司的拆股数据

使用`getSplits()`函数获取上市公司的拆股数据：

```
getSplits("MSFT")
```

### 1.1.5 获取上市公司期权交易数据

利用getOptionChain()函数可以获取上市公司的期权交易数据：

```
#例：获取苹果公司的期权交易数据
AAPL.OPT <- getOptionChain("AAPL")
AAPL.OPTS <- getOptionChain("AAPL", NULL)
```

与此相关的还有options.expiry()函数和futures.expiry()函数：

```
#例：
getSymbols("AAPL")
options.expiry(AAPL)
futures.expiry(AAPL)
AAPL[options.expiry(AAPL)]
```

### 1.1.6 获取和查看上市公司的财务报表

quantmod中getFinancials()函数和getFin()函数可以获取上市公司的财务报表数据。看看两个函数的用法：

```
args(getFinancials)
args(getFin)
```

例子：获取中国移动公司的财务数据

```
getFinancials('CHL')
```

获取数据之后，可以通过view.Fin函数查看财务报表数据:

```
view.Fin(CHL.f)
```

### 1.1.7 从网络获取汇市数据

getFX()函数可以帮助我们从oanda获取汇率数据。

```
getFX("USD/JPY")
getFX("EUR/USD",from="2005-01-01")
```

也可以：

```
getSymbols("USD/EUR",src="oanda")
getSymbols("USD/EUR",src="oanda",from="2005-01-01")
```

### 1.1.8 获取重金属交易数据

getMetals()函数可以获取重金属的交易数据。

```
getFX(c("gold","XPD"))
getFX("plat",from="2005-01-01")
```

### 1.1.9 获取美联储经济数据

getSymbols.FRED()函数可以获取美联储主页上的美国经济数据。
```
getSymbols('CPIAUCNS',src='FRED')
```

或者：
```
setSymbolLookup(CPIAUCNS='FRED')
getSymbols('CPIAUCNS')
```

## 1.2 从数据库获取股票数据

quantmod除了支持从网络数据库直接抓取数据外，当然也支持从本地数据库读入数据。目前，能支持的数据库类型包括：

* MySQL
* SQLite
* csv
* RData

对应的函数有以下几个：

* getSymbols.MySQL()：从 MySQL 数据库读取数据
* getSymbols.SQLite()：从 SQLite 数据库读取数据
* getSymbols.csv()：从 csv 文件读取 OHLC 数据
* getSymbols.rda()：读取以 .r 格式存储的数据


## 1.3 查看和移除股票数据

### 1.3.1 查看股票数据

```
getSymbols("CHL","000023.ss")
showSymbols(env=.GlobalEnv)
```

结果如下：

### 1.3.2 移除股票数据

```
RemoveSymbols("CHL")
showSymbols(env=.GlobalEnv)
```

```
getQuote("AAPL")
getQuote("QQQQ;SPY;^VXN",what=yahooQF(c("Bid","Ask")))
standardQuote()
yahooQF()
```

```
attachSymbols()
```
