chapter02_快速探索数据
================

- <a href="#2-快速探索数据" id="toc-2-快速探索数据">2 快速探索数据</a>
  - <a href="#21-绘制散点图" id="toc-21-绘制散点图">2.1 绘制散点图</a>
  - <a href="#22-绘制折线图" id="toc-22-绘制折线图">2.2 绘制折线图</a>
  - <a href="#23-绘制条形图" id="toc-23-绘制条形图">2.3 绘制条形图</a>
  - <a href="#24-绘制直方图" id="toc-24-绘制直方图">2.4 绘制直方图</a>
  - <a href="#25-绘制箱线图" id="toc-25-绘制箱线图">2.5 绘制箱线图</a>
  - <a href="#26-绘制函数图像" id="toc-26-绘制函数图像">2.6 绘制函数图像</a>

Source：

1.  《R数据可视化手册》，北京：人民邮电出版社，2014.5

# 2 快速探索数据

## 2.1 绘制散点图

- 使用`plot()`函数可绘制散点图，运行命令时依次传递给`plot()`函数一个向量x和一个向量y。

``` r
> plot(mtcars$wt,mtcars$mpg)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

- 对于ggplot2系统，可用`qplot()`函数得到相同的绘图结果：

``` r
> library(ggplot2)
> qplot(mtcars$wt,mtcars$mpg)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

- 如果绘图所用的两个参数向量包含在同一个数据框内，则可以运行下面的命令：

``` r
> qplot(wt,mpg,data=mtcars)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

- 或者：

``` r
> ggplot(mtcars,aes(x=wt,y=mpg)) + geom_point()
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

- 更多关于绘制散点图的详细内容可参见第5章。

## 2.2 绘制折线图

- 使用`plot()`函数绘制折线图时需向其传递一个包含x值的向量和一个包含y值的向量，并使用参数type=“l”：

``` r
> plot(pressure$temperature,pressure$pressure,type = 'l')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

- 如果要向图形中添加数据点或者多条折线，则需先用`plot()`函数绘制第一条折线，再通过`points()`函数和`lines()`函数分别添加数据点和更多折线：

``` r
> plot(pressure$temperature,pressure$pressure,type = 'l')
> points(pressure$temperature,pressure$pressure)
> lines(pressure$temperature,pressure$pressure/2,col = 'red')
> points(pressure$temperature,pressure$pressure/2,col = 'red')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

- 在ggplot2中，可以使用`qplot()`函数并将参数设定为`geom="line"`得到类似的绘图结果：

``` r
> library(ggplot2)
> qplot(pressure$temperature,pressure$pressure,geom = 'line')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

- 如果函数的两个参数向量已包含在同一个数据框中，则可以运行下面的语句：

``` r
> qplot(temperature,pressure,data = pressure,geom = 'line')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
> # 等价于
> # ggplot(pressure,aes(x=temperature,y=pressure)) + geom_line()
> 
> # 添加数据点
> qplot(temperature,pressure,data = pressure,geom = c('line','point'))
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

``` r
> # 等同于
> # ggplot(pressure,aes(x=temperature,y=pressure)) + geom_line() + geom_point()
```

- 更多关于绘制折线图的详细内容可参见第4章。

## 2.3 绘制条形图

- 对变量的值绘制条形图，可以使用`barplot()`函数，并向其传递两个向量作为参数，第一个向量用来设定条形的高度，第二个向量用来设定每个条形对应的标签（可选）。

- 如果向量中的元素已被命名，则系统会自动使用元素的名字作为条形标签：

``` r
> barplot(BOD$demand,names.arg=BOD$Time)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

- 有时候，“条形图”表示的是分组数据中各个元素的频数。这种条形图跟直方图有些类似，不过，其用离散取值的x轴替代了直方图中连续取值的x轴。要计算向量中各个类别的频数，可以使用`table()`函数。

``` r
> table(mtcars$cyl)

 4  6  8 
11  7 14 
```

- 值为4的频数为11，6的为7，8的为14。

- 只需将上面的表格结果传递给`barplot()`函数即可绘制频数条形图：

``` r
> barplot(table(mtcars$cyl))
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

- 如果参数向量包含在同一个数据框内，则可以运行下面的语句：

- **注意变量x分别为连续取值和离散取值时输出结果的差异**。

``` r
> library(ggplot2)
> # 变量值条形图，这里用BOD数据框中的Time列和demand列分别作为x和y参数
> ggplot(BOD,aes(x=Time,y=demand)) + geom_bar(stat = 'identity')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
> # 这与下面的语句等价
> # qplot(Time, demand, data=BOD, geom="bar", stat="identity") 
```

- 将x转化为因子变量，令系统将其视作离散值:

``` r
> library(ggplot2)
> ggplot(BOD,aes(x=factor(Time),y=demand)) + geom_bar(stat = 'identity')
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

- **再提醒一次，注意连续x轴和离散x轴的差异。**

``` r
> # cyl是连续变量
> qplot(mtcars$cyl)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
> # 将cyl转化为因子变量
> qplot(factor(mtcars$cyl))
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

``` r
> # 这与下面的语句等价
> # qplot (factor(cyl), data=mtcars)
> # ggplot (mtcars, aes(x=factor(cyl))) + geom_bar()
```

- 更多关于绘制条形图的详细内容可参见第3章。

## 2.4 绘制直方图

- 可以使用`hist()`函数绘制直方图，使用时需向其传递一个向量：

``` r
> hist(mtcars$mpg)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
> # 通过 breaks 参数指定大致组距
> hist(mtcars$mpg,breaks = 10)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-15-2.png)<!-- -->

- 对于ggplot2包，可以使用`qplot()`函数得到同样的绘图结果：

``` r
> qplot(mtcars$mpg)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

- 如果参数向量在同一个数据框内，则可以使用下面的语句：

``` r
> library(ggplot2)
> qplot(mpg,data = mtcars,binwidth=4)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

``` r
> # 等价于
> # ggplot(mtcars,aes(x=mpg)) + geom_histogram(binwidth = 4)
```

- 更多关于绘制直方图的内容参见6.1节和6.2节。

## 2.5 绘制箱线图

- 使用`plot()`函数绘制箱线图时向其传递两个向量：x和y。当x为因子型变量（与数值型变量对应）时，它会默认绘制箱线图：

``` r
> plot(ToothGrowth$supp,ToothGrowth$len)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

- 当两个参数向量包含在同一个数据框中时，也可以使用公式语法。公式语法允许我们在x轴上使用变量组合：

``` r
> # 公式语法
> boxplot(len~supp,data = ToothGrowth)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

``` r
> # 在x轴上引入两变量的交互
> boxplot(len~supp+dose,data = ToothGrowth)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-19-2.png)<!-- -->

- 对于ggplot2包，你可以使用`qplot()`函数绘制同样的图形，使用时将参数设定为`geom="boxplot"`：

``` r
> library(ggplot2)
> qplot (ToothGrowth$supp,ToothGrowth$len,geom="boxplot")
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

- 当两个参数向量在同一个数据框内时，则可以使用下面的语句：

``` r
> qplot(supp,len,data=ToothGrowth,geom="boxplot") 
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

``` r
> #这等价于
> # ggplot(ToothGrowth,aes(x=supp,y=len))+geom_boxplot()
```

- 使用`interaction()`函数将分组变量组合在一起也可以绘制基于多分组变量的箱线图。本例中，dose变量是数值型，因此，我们必须先将其转化为因子型变量，再将其作为分组变量：

``` r
> #使用三个独立的向量参数
> qplot(interaction(ToothGrowth$supp,ToothGrowth$dose),ToothGrowth$len,geom="boxplot") #也可以以数据框中的列作为参数
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
> qplot(interaction(supp,dose),len,data=ToothGrowth,geom="boxplot") 
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-22-2.png)<!-- -->

``` r
> #这等价于
> # ggplot(ToothGrowth,aes(x=interaction(supp,dose),y=len)) + geom_boxplot()
```

- 更多关于绘制箱线图的内容参见6.6节。

## 2.6 绘制函数图像

- 可以使用`curve()`函数绘制函数图像。使用时需向其传递一个关于变量x的表达式：

``` r
> curve(x^3-5*x,from = -4,to=4)
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

- 你可以绘制任何一个以数值型向量作为输入且以数值型向量作为输出的函数图像，包括你自己定义的函数：

- 将参数设置为`add=TRUE`可以向已有图形添加函数图像：

``` r
> #绘制用户自定义的函数图像
> myfun <- function(xvar){
+   1/(1+exp(-xvar+10))
+ }
> curve(myfun(x),from=0,to=20)
> #添加直线
> curve(1-myfun(x),add=TRUE,col ="red")
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

- 对于ggplot2，可以使用`ggplot()`函数绘制得到同样的结果。使用时需向其传递一个输入和输出皆为数值型向量的函数：

``` r
> library(ggplot2)
> #将×轴的取值范围设定为0到20
> ggplot(data.frame(x=c(0,20)),aes(x=x)) + stat_function(fun=myfun,geom="line")
```

![](chapter02_快速探索数据_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

- 更多关于绘制函数图像的内容参见13.2节。
