chapter23_lattice高级绘图
================

- <a href="#23-使用-lattice-进行高级绘图"
  id="toc-23-使用-lattice-进行高级绘图">23 使用 lattice 进行高级绘图</a>
  - <a href="#231-lattice-包" id="toc-231-lattice-包">23.1 lattice 包</a>
  - <a href="#232-调节变量" id="toc-232-调节变量">23.2 调节变量</a>
  - <a href="#233-面板函数" id="toc-233-面板函数">23.3 面板函数</a>
  - <a href="#234-分组变量" id="toc-234-分组变量">23.4 分组变量</a>
  - <a href="#235-图形参数" id="toc-235-图形参数">23.5 图形参数</a>
  - <a href="#236-自定义图形条带" id="toc-236-自定义图形条带">23.6
    自定义图形条带</a>
  - <a href="#237-页面布局" id="toc-237-页面布局">23.7 页面布局</a>

# 23 使用 lattice 进行高级绘图

- 安装lattice包。

## 23.1 lattice 包

- lattice包提供了用于可视化单变量和多变量数据的一整套图形系统，它能很容易地生成网格图形。网格图形能够展示变量的分布或变量之间的关系，每幅图代表一个或多个变量的各个水平。

- 思考下面的问题：纽约合唱团各声部的歌手身高是如何变化的？lattice包在singer数据集中提供了身高和声部的数据。在下面的代码中height是独立的变量，voice.part被称作调节变量。

``` r
> library(lattice) 
> histogram(~height | voice.part, data = singer,   
+           main="Distribution of Heights by Voice Pitch",   
+           xlab="Height (inches)")
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

- 上面的代码可以得出每个声部的直方图，似乎男高音和男低音比女低音和女高音的身高更高。

- lattice包提供了大量的函数来产生单因素图(点图、核密度图、直方图、条形图、箱线图)，二元图(散点图、条形图、平行箱线图)和多元图(3D图、散点图矩阵)。每个高水平的画图函数都服从下面的格式：

  `graph_function(formula, data=, options)`

  其中：

  - 1、graph_function是下表第2列中的一个函数；
  - 2、formula指定要展示的变量和任意的调节变量；
  - 3、data=指定数据框；
  - 4、options是用逗号分隔的参数，用来调整图形的内容、安排和注释。

- 假定小写字母代表数值型变量，大写字母代表分类型变量(因子)。高水平的画图函数通常采取的格式是：

  `y ~ x | A * B`

  其中竖线左侧的变量称为主要变量(primary
  variable)，右边的变量称为调节变量(conditioning
  variable)。主要变量将变量映射到每个面板的轴上。这里的`y~x`分别描述了在纵轴和横轴上的变量。对于单变量图，用`~x`代替`y~x`；对于3D图，用`z~x*y`代替`y~x`；对于多变量图(散点图矩阵或平行坐标曲线图)，用数据框来代替`y~x`。需要注意的是调节变量总是可选的。

  按照这个逻辑，`~x|A`表示因子A每个水平的数值变量x。`y~x|A*B`表示在给定因子A和B的水平后，数值变量y和x的关系。`A~x`表示在纵轴上的分类变量A和横轴上的数值变量x。`~x`表示数值型变量x。

| 图 类 型       | 函 数          | 公式例子   |
|----------------|----------------|------------|
| 3D等高线图     | contourplot()  | z\~x\*y    |
| 3D水平图       | levelplot()    | z\~y\*x    |
| 3D散点图       | cloud()        | z\~x\*y\|A |
| 3D线框图       | wireframe()    | z\~y\*x    |
| 条形图         | barchart()     | x\~A或A\~x |
| 箱线图         | bwplot()       | x\~A或A\~x |
| 点图           | dotplot()      | \~x\|A     |
| 柱状图         | histogram()    | \~x        |
| 核密度图       | densityplot()  | \~x\|A\*B  |
| 平行坐标曲线图 | parallelplot() | dataframe  |
| 散点图         | xyplot()       | y\~x\|A    |
| 散点图矩阵     | splom()        | dataframe  |
| 线框图         | stripplot()    | A\~x或x\~A |

- 注：在这些公式中小写字母表示数值型变量，大写字母表示分类型变量。

- 代码清单23-1 lattice画图例子

``` r
> library(lattice) 
> attach(mtcars)  
> gear <- factor(gear, levels=c(3, 4, 5),         
+                labels=c("3 gears", "4 gears", "5 gears"))
> cyl <- factor(cyl, levels=c(4, 6, 8),
+               labels=c("4 cylinders", "6 cylinders", "8 cylinders"))
> densityplot(~mpg,         
+             main="Density Plot",        
+             xlab="Miles per Gallon") 
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
> densityplot(~mpg | cyl,         
+             main="Density Plot by Number of Cylinders",     
+             xlab="Miles per Gallon")  
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

``` r
> bwplot(cyl ~ mpg | gear,      
+        main="Box Plots by Cylinders and Gears",    
+        xlab="Miles per Gallon", ylab="Cylinders") 
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-3.png)<!-- -->

``` r
> xyplot(mpg ~ wt | cyl * gear,     
+        main="Scatter Plots by Cylinders and Gears",      
+        xlab="Car Weight", ylab="Miles per Gallon") 
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-4.png)<!-- -->

``` r
> cloud(mpg ~ wt * qsec | cyl,     
+       main="3D Scatter Plots by Cylinders")  
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-5.png)<!-- -->

``` r
> dotplot(cyl ~ mpg | gear,       
+         main="Dot Plots by Number of Gears and Cylinders",    
+         xlab="Miles Per Gallon")  
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-6.png)<!-- -->

``` r
> splom(mtcars[c(1, 3, 4, 5, 6)],   
+       main="Scatter Plot Matrix for mtcars Data")  
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-2-7.png)<!-- -->

``` r
> detach(mtcars)
```

- lattice包中的高水平画图函数能产生可保存和修改的图形对象。例如，下列代码创建了一个网格密度图，并把它保存为对象mygraph10_1.1。但是没有图像展示。声明plot(mygraph10_1.1)(或仅仅是mygraph10_1.1)将会展示出这幅图。

``` r
> library(lattice) 
> mygraph10_1.1 <- densityplot(~height|voice.part, data=singer) 
> plot(mygraph10_1.1)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

- 通过调整选项很容易改变lattice图形。常见的选项列在下表中：

| 选 项              | 描 述                                                                                               |
|--------------------|-----------------------------------------------------------------------------------------------------|
| aspect             | 指定每个面板图形的纵横比(高度/宽度)的一个数字。                                                     |
| col、pch、lty、lwd | 分别指定在图中用到的颜色、符号、线类型和线宽度的向量。                                              |
| group              | 分组变量(因子)。                                                                                    |
| index.cond         | 列出展示面板顺序的列表。                                                                            |
| key(或auto.key)    | 支持分组变量中图例的函数。                                                                          |
| layout             | 指定面板设置(列数和行数)的二元素数值向量。如果需要，可以增加一个元素来表示页面数。                  |
| main、sub          | 指定主标题和副标题的字符向量。                                                                      |
| panel              | 在每个面板中生成图的函数。                                                                          |
| scales             | 列出提供坐标轴注释信息的的列表。                                                                    |
| strip              | 用于自定义面板条带的函数 split、position 数值型向量，在一页上绘制多幅。                             |
| split、position    | 数值型向量，在一页上绘制多幅图形。                                                                  |
| type               | 指定一个或多个散点图绘图选项(p=点,l=线,r=回归线，smooth=局部多项式回归拟合，g=网格图形)的字符向量。 |
| xlab、ylab         | 指定横轴和纵轴标签的字符向量。                                                                      |
| xlim、ylim         | 分别指定横轴和纵轴最小值、最大值的二元素数值向量。                                                  |

- 可以使用`update()`函数来调整lattice图形对象。继续歌手的例子：

``` r
> newgraph10_1.1 <- update(mygraph10_1.1, col="red", pch=16,      
+                    cex=.8, jitter=.05, lwd=2)
> plot(newgraph10_1.1)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

- 使用红色曲线和符号(color=“red”)，填充点(pch=16)，更小(cex=.8)更高的抖动点(jitter=.05)和双倍厚度曲线(lwd=2)来改变mygraph10_1.1。

## 23.2 调节变量

- lattice包提供的函数可以将连续的变量转化为名为shingle的数据结构。具体来说，连续变量被分成一系列(可能)重叠的范围。例如，函数：

  `myshingle <- equal.count(x, number=n, overlap=proportion)`

  将连续的变量x分成n个间隔，重叠比例是proportion，每个间隔里的观测值个数相同，并将其返回为变量myshingle(属于shingle类)。打印或画出对象(例如输入plot(mysingle))将展示shingle的间隔。

- 一旦变量转化为shingle，就可以使用它来作为一个调节变量。例如，使用mtcars数据集来探索汽车每加仑汽油的行驶英里数与以发动机排量为条件的汽车重量之间的关系。因为发动机排量是一个连续的变量，所以：

``` r
> # 首先把它转化为三水平的shingle变量
> displacement <- equal.count(mtcars$disp, number=3, overlap=0)
> # 把这个变量应用到xyplot()函数中
> xyplot(mpg~wt|displacement, data=mtcars,   
+        main = "Miles per Gallon vs. Weight by Engine Displacement",   
+        xlab = "Weight", ylab = "Miles per Gallon", 
+        layout=c(3, 1), aspect=1.5)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

- **注意：使用了选项来调整面板的布局(一行三列)和纵横比(高/宽)来让三组的对比变得更容易。**

- 可以看到上图中面板条带的标签是不一样的，图中的表现形式指出了调节变量的连续性质，较深的颜色表示给定面板中调节变量的范围。

## 23.3 面板函数

- 默认函数遵循命名规则`panel.graph_function`，其中`graph_function`指的是高水平的函数。例如：

  `xyplot(mpg~wt|displacement, data=mtcars)`

  也可以写成：

  `xyplot(mpg~wt|displacement, data=mtcars, panel=panel.xyplot)`

- 在上一节画出了以汽车发动机排量为条件的汽车重量的油耗。如想加上回归线、地毯图和网格线，可以通过创建自己的面板函数来实现它。

- 代码清单23-2 自定义面板函数xyplot

``` r
> library(lattice) 
> displacement <- equal.count(mtcars$disp, number=3, overlap=0)  
> mypanel <- function(x, y) {     
+   panel.xyplot(x, y, pch=19)         
+   panel.rug(x, y)         
+   panel.grid(h=-1, v=-1)  
+   panel.lmline(x, y, col="red", lwd=1, lty=2)     
+   }  
> xyplot(mpg~wt|displacement, data=mtcars,    
+        layout=c(3, 1),     
+        aspect=1.5,      
+        main = "Miles per Gallon vs. Weight by Engine Displacement",     
+        xlab = "Weight",   
+        ylab = "Miles per Gallon",     
+        panel = mypanel)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

- 这里将四个独立的构件函数集成到自己的`mypanel()`函数中，并通过`xyplot()`函数中的`panel=option`选项使它生效。`panel.xyplot()`函数使用一个填充的圆(pch=19)产生散点图。`panel.rug()`函数把地毯图加到x轴和y轴的每个标签上。`panel.rug(x, FALSE)`和`panel.rug(FALSE,y)`将分别把地毯加到横轴和纵轴。`panel.grid()`函数添加水平和垂直的网格线(使用负数迫使其用轴标签排队)。最后，`panel.lmline()`函数添加了被渲染成红色(col=“red”)、标准厚度(lwd=2)的虚线(lty=2)回归曲线。每个默认的面板函数都有自己的结构和选项。可以参考帮助页面来获取细节(例如输入`help(panellmline)`)。

- 在第二个例子中，画出了以汽车变速器类型为条件的油耗和发动机排量(被认为是连续型变量)之间的关系。除了画出自动和手动变速器发动机独立的面板图外，还将添加拟合曲线和水平均值曲线。

- 代码清单23-3 自定义面板函数和额外选项的xyplot

``` r
> library(lattice) 
> mtcars$transmission <- factor(mtcars$am, levels=c(0,1),    
+                               labels=c("Automatic", "Manual")) 
> panel.smoother <- function(x, y) {             
+   panel.grid(h=-1, v=-1)                
+   panel.xyplot(x, y)                 
+   panel.loess(x, y)         
+   panel.abline(h=mean(y), lwd=2, lty=2, col="darkgreen")     
+ } 
> 
> xyplot(mpg~disp|transmission,data=mtcars,   
+        scales=list(cex=.8, col="red"),    
+        panel=panel.smoother, 
+        xlab="Displacement", ylab="Miles per Gallon",    
+        main="MPG vs Displacement by Transmission Type",      
+        sub = "Dotted lines are Group Means", aspect=1)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## 23.4 分组变量

- 在lattice绘图公式中增加调节变量时，该变量每个水平的独立面板就会产生。如果想添加的结果和每个水平正好相反，可以指定该变量为分组变量。假如想利用核密度图展示使用手动和自动变速器时汽车油耗的分布，可以使用下面的代码来添加相应的图形：

``` r
> library(lattice)
> mtcars$transmission <- factor(mtcars$am, levels=c(0, 1),     
+                               labels=c("Automatic", "Manual")) 
> densityplot(~mpg, data=mtcars,        
+             group=transmission,         
+             main="MPG Distribution by Transmission Type",           
+             xlab="Miles per Gallon",       
+             auto.key=TRUE)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

- 结果上图所示。默认情况下，group=选项添加分组变量每个水平的图。点会被绘制成空心圆，线为实线，水平信息用不同的颜色表示。

- 值得注意的是，图例和关键字不会在默认情况下生成。选项`auto.key=TRUE`创建了一个基本的图例并把它放在图的上方。可以通过在列表中指定选项对自动的键值进行有限的修改。例如，修改auto.key代码将图例放在图的右侧，在单个列中呈现关键字，并添加了一个图例标题：

``` r
> library(lattice)
> mtcars$transmission <- factor(mtcars$am, levels=c(0, 1),     
+                               labels=c("Automatic", "Manual")) 
> densityplot(~mpg, data=mtcars,        
+             group=transmission,         
+             main="MPG Distribution by Transmission Type",           
+             xlab="Miles per Gallon",       
+             auto.key=list(space="right", columns=1, title="Transmission"))
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

- 如果想对图例取得更大的控制权，可以使用key=选项。

- 代码清单23-4 带有分组变量和自定义图例的核密度估计

``` r
> library(lattice) 
> mtcars$transmission <- factor(mtcars$am, levels=c(0, 1),                               labels=c("Automatic", "Manual"))
> # 指定的颜色、线和点
> colors <- c("red", "blue") 
> lines  <- c(1,2) 
> points <- c(16,17)
> # 自定义图例
> key.trans <- list(title="Transmission",     
+                   space="bottom", columns=2,       
+                   text=list(levels(mtcars$transmission)),   
+                   points=list(pch=points, col=colors),       
+                   lines=list(col=colors, lty=lines),      
+                   cex.title=1, cex=.9)
> # 密度图
> densityplot(~mpg, data=mtcars,        
+             group=transmission,       
+             main="MPG Distribution by Transmission Type",    
+             xlab="Miles per Gallon",        
+             pch=points, lty=lines, col=colors,
+             lwd=2, jitter=.005,      
+             key=key.trans)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

- 相同的图类型、线条类型和颜色由`densityplot()`函数指定。此外，增加了线条宽度和抖动来改善图形的外观。最后，键值被设定为使用之前定义的列表。

- 在单个图中包含分组和调节变量的例子：R安装时自带的CO2数据框描述了对Echinochloa
  crus-galli耐寒性的研究。这个数据描述了12种植物(Plant)在7种二氧化碳浓度(conc)下的二氧化碳吸收率(uptake)。6种植物来自魁北克(Quebec)，6种来自密西西比(Mississippi)。每个产地有3种植物在冷藏条件下研究，3种在非冷藏条件下研究。在这个例子中，Plant是分组变量，Type(魁北克/密西西比)和Treatment(冷藏/非冷藏))是调节变量。

- 代码清单23-5 带有分组和调节变量以及自定义图例的xyplot函数

``` r
> library(lattice) 
> colors <- "darkgreen" 
> symbols <- c(1:12) 
> linetype <- c(1:3) 
> key.species <- list(title="Plant",
+                     space="right",     
+                     text=list(levels(CO2$Plant)),      
+                     points=list(pch=symbols, col=colors)) 
> xyplot(uptake~conc|Type*Treatment, data=CO2,    
+        group=Plant,    
+        type="o",     
+        pch=symbols, col=colors, lty=linetype,      
+        main="Carbon Dioxide Uptake\nin Grass Plants",   
+        ylab=expression(paste("Uptake ",           
+                              bgroup("(", italic(frac("umol","m"^2)), ")"))),     
+        xlab=expression(paste("Concentration ",          
+                              bgroup("(", italic(frac(mL,L)), ")"))),     
+        sub = "Grass Species: Echinochloa crus-galli",     
+        key=key.species)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

- **注意，这里使用`\n`让标题分成两行**。使用`expression()`函数是为了将数学符号添加到坐标轴标签上。在这里，通过col=选项指定一组颜色来对组进行区分。在这个例子中，添加12种颜色矫枉过正、使人分心，难以实现简单地可视化各面板的关系。很明显，在冷藏条件下密西西比的植物有显著的不同。

## 23.5 图形参数

- lattice函数使用的图形默认设置包含在一个大的列表对象中，可以通过`trellis.par.get()`函数获得并通过`trellis.par.set()`函数更改。可以使用`show.settings()`函数来直观地展示当前的图形设置。作为一个例子，使用叠加点来改变默认符号(即包含一个组变量的图中的点)。默认值是一个开环。可以为每个组设置自己的符号。首先，查看默认的设置：

``` r
> show.settings()
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

- 把它们保存到名为mysettings的列表中，使用names()函数来查看列表的成分：

``` r
> mysettings <- trellis.par.get()
> names(mysettings)
 [1] "grid.pars"         "fontsize"          "background"       
 [4] "panel.background"  "clip"              "add.line"         
 [7] "add.text"          "plot.polygon"      "box.dot"          
[10] "box.rectangle"     "box.umbrella"      "dot.line"         
[13] "dot.symbol"        "plot.line"         "plot.symbol"      
[16] "reference.line"    "strip.background"  "strip.shingle"    
[19] "strip.border"      "superpose.line"    "superpose.symbol" 
[22] "superpose.polygon" "regions"           "shade.colors"     
[25] "axis.line"         "axis.text"         "axis.components"  
[28] "layout.heights"    "layout.widths"     "box.3d"           
[31] "par.title.text"    "par.xlab.text"     "par.ylab.text"    
[34] "par.zlab.text"     "par.main.text"     "par.sub.text"     
```

- 具体到叠加符号的默认值包含在`superpose.symbol`中：

``` r
> mysettings$superpose.symbol 
$alpha
[1] 1 1 1 1 1 1 1

$cex
[1] 0.8 0.8 0.8 0.8 0.8 0.8 0.8

$col
[1] "#0080ff"   "#ff00ff"   "darkgreen" "#ff0000"   "orange"    "#00ff00"  
[7] "brown"    

$fill
[1] "#CCFFFF" "#FFCCFF" "#CCFFCC" "#FFE5CC" "#CCE6FF" "#FFFFCC" "#FFCCCC"

$font
[1] 1 1 1 1 1 1 1

$pch
[1] 1 1 1 1 1 1 1
```

- 分组变量的每个水平使用的符号是开环(pch=1)。七个水平得到定义之后，符号会再循环。为了改变默认值，声明语句如下，可以再次使用show.settings()函数来查看改动的影响：

``` r
> mysettings$superpose.symbol$pch <- c(1:10) 
> trellis.par.set(mysettings)
> show.settings()
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

- lattice图形使用符号1(开环)代表分组变量的第一个水平，使用符号2(开三角)代表分组变量的第二个水平，以此类推。此外，符号以被定义为10个级别的分组变量，而不是7个。在图形设备关闭之前，这些变化是一直起作用的。

## 23.6 自定义图形条带

- 面板条带默认的背景是：第一个调节变量是桃红色，第二个调节变量是浅绿色，第三个调节变量是浅蓝色。

- 可以自定义颜色、字体和这些条带的其他方面。可以使用上一节描述的方法，或是加强控制，写一个自定义条带各方面的函数。

- 正如lattice中的高水平图形函数允许我们通过控制每个面板的内容指定一个面板函数一样，条带函数可以自定义条带的方方面面。图形的背景是桃红色(抑或是粉橙色)，如果想让条带变成浅灰色，条带的文本变成红色，字体变成斜体并缩小20%，可以使用下面的代码`strip = strip.custom()`来实现：

``` r
> library(lattice) 
> histogram(~height | voice.part, data = singer,    
+           strip = strip.custom(bg="lightgrey",    
+                                par.strip.text=list(col="red", cex=.8, font=3)),   
+           main="Distribution of Heights by Voice Pitch",    
+           xlab="Height (inches)")
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

- bg选项控制了背景颜色，par.strip.text允许我们控制条带文本的外观。par.strip.text选项使用一个列表去定义文本属性。col和cex控制文本的颜色和大小。font选项可以分别取数值1、2、3和4,代表正常字体、粗体、斜体和粗斜体。

- strip=选项改变了给定图中条带的外观。要改变一个R会话中所有lattice图形的外观，可以使用上一节使用的图形参数：设定了条带的背景，其中第一个条件变量是浅灰色，第二个是浅绿色。**该更改在会话的剩余部分仍然起作用，或是到设置再次更改后才失效**。

``` r
> mysettings <- trellis.par.get() 
> mysettings$strip.background$col <- c("lightgrey", "lightgreen") 
> trellis.par.set(mysettings)
> show.settings()
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## 23.7 页面布局

- 将多个lattice图形绘制在一个单独的图中。最简单的方法是把lattice图形保存成对象并使用带有split=或position=选项的`plot()`函数来保存成单个图片。

- split选项将一个页面分成指定数量的行和列，并把图放到结果矩阵的特定单元格中。split选项的格式是：

  `split=c(x, y, nx, ny)`

  也就是说在包括nx乘ny个图形的正规数组中，把当前图形放在x,
  y的位置，图形的起始位置是左上角。例如：

``` r
> library(lattice)  
> graph1 <- histogram(~height | voice.part, data = singer,     
+                     main = "Heights of Choral Singers by Voice Part" ) 
> graph2 <- bwplot(height~voice.part, data = singer) 
> plot(graph1, split = c(1, 1, 1, 2)) 
> plot(graph2, split = c(1, 2, 1, 2), newpage = FALSE)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

- 将第一幅图直接放在第二幅图的上面。具体来说，第一个`plot()`语句将页面分成了一列(nx=1)和两行(ny=2)并把图放置在第一行第一列(顺序是从上到下，从左到右)。第二个`plot()`语句用同样的方式划分页面，但是把图放在了第一列第二行。`plot()`函数默认从一个新的页面开始，可以通过`newpage=FALSE`选项抑制新的页面产生。

- 可以使用position=选项更好地控制尺寸和位置。

``` r
> library(lattice) 
> graph1 <- histogram(~height | voice.part, data = singer,        
+                     main = "Heights of Choral Singers by Voice Part")
> graph2 <- bwplot(height~voice.part, data = singer)
> plot(graph1, position=c(0, .3, 1, 1)) 
> plot(graph2, position=c(0, 0, 1, .3), newpage=FALSE)
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

- 这里的position=c(xmin, ymin, xmax,
  ymax)选项中，页面的坐标系是x轴和y轴都从0到1的矩形，原点在左下角的(0,
  0)。

- 也可以改变lattice图中面板的顺序。在高水平lattice图像函数中的`index.cond`选项能指定调节变量水平的顺序。对于`voice.part`因子来说，水平是：

``` r
> levels(singer$voice.part)
[1] "Bass 2"    "Bass 1"    "Tenor 2"   "Tenor 1"   "Alto 2"    "Alto 1"   
[7] "Soprano 2" "Soprano 1"
```

- 使用下列代码可以把声部1放在一起(Bass 1、Tenor 1……)，接着是声部2(Bass
  2、Tenor 2……)。当有两个调节变量时，在列表中包含两个向量。

``` r
> histogram(~height | voice.part, data = singer,    
+           index.cond=list(c(2, 4, 6, 8, 1, 3, 5, 7)))
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

- 在代码清单23-5中，添加`index.cond=list(c(1,2),c(2,1))`将让图中处理条件的顺序反过来。`index.cond`选项的详细信息可以通过`help(xyplot)`了解。

``` r
> library(lattice) 
> colors <- "darkgreen" 
> symbols <- c(1:12) 
> linetype <- c(1:3) 
> key.species <- list(title="Plant",
+                     space="right",     
+                     text=list(levels(CO2$Plant)),      
+                     points=list(pch=symbols, col=colors)) 
> xyplot(uptake~conc|Type*Treatment, data=CO2,    
+        group=Plant,    
+        type="o",     
+        pch=symbols, col=colors, lty=linetype,      
+        main="Carbon Dioxide Uptake\nin Grass Plants",   
+        ylab=expression(paste("Uptake ",           
+                              bgroup("(", italic(frac("umol","m"^2)), ")"))),     
+        xlab=expression(paste("Concentration ",          
+                              bgroup("(", italic(frac(mL,L)), ")"))),     
+        sub = "Grass Species: Echinochloa crus-galli",     
+        key=key.species,
+        index.cond=list(c(1,2),c(2,1)))
```

![](chapter23_lattice高级绘图_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->
