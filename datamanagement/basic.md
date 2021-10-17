# Basic

## Setup

* [https://www.jianshu.com/p/62e71f30d0d6](https://www.jianshu.com/p/62e71f30d0d6)
* [https://wly.supernum.tech/2019/12/使用r语言绘制图形/#1-3-绘制平面几何](https://wly.supernum.tech/2019/12/%E4%BD%BF%E7%94%A8r%E8%AF%AD%E8%A8%80%E7%BB%98%E5%88%B6%E5%9B%BE%E5%BD%A2/#1-3-%E7%BB%98%E5%88%B6%E5%B9%B3%E9%9D%A2%E5%87%A0%E4%BD%95)

### 图形参数

```
1. par
2. optionname=value
```

函数par来永久地改变绘图参数, par(mfrow=c(1,1)) ,mfrow指定图形排布为1行，每行1个图 改变绘图参数时，预先保存 它们的初始值以便以后恢复十分有用

指定图形参数的第二种方法是为高级绘图函数直接提供optionname=value的键值对。这种 情况下，指定的选项仅对这幅图形本身有效。并不是所有的高级绘图函数都允许指定全部可能的图形参数。你需要参考每个特定绘图函数的帮助(如?plot、?hist或?boxplot)以确定哪些参数

#### 符号和线条

```
knitr::include_graphics("./00 Fotos/parameter 01.png")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FRNU691vdrszITFCIaNoA%2Ffile.png?alt=media)

#### 1.1.2 颜色

[Sources](http://www.stat.columbia.edu/\~tzheng/files/Rcolor.pdf)

在R中，可以通过颜色下标、颜色名称、十六进制的颜色值、RGB值或HSV值来指定颜色。 举例来说，col=1、col=“white”、col=“#FFFFFF”、col=rgb(1,1,1)和col=hsv(0,0,1) 都是表示白色的等价方式。函数rgb()可基于红—绿—蓝三色值生成颜色，而hsv()则基于色相— 饱和度—亮度值来生成颜色。请参考这些函数的帮助以了解更多细节

### 1.2 添加文本、自定义坐标轴和图例

除了图形参数，许多高级绘图函数(例如plot、hist、boxplot)也允许自行设定坐标轴 和文本标注选项。举例来说，以下代码在图形上添加了标题(main)、副标题(sub)、坐标轴标 签(xlab、ylab)并指定了坐标轴范围(xlim、ylim)

#### 1.2.1 Setting

```
标题: title()函数
坐标轴: 函数axis()来创建自定义的坐标轴，而非使用R中的默认坐标轴
参考线: abline()可以用来为图形添加参考线
图例: legend()
文本标注: 函数text()和mtext()将文本添加到图形上。text()可向绘图区域内部添加文本，而mtext()则向图形的四个边界之一添加文本
```

```
knitr::include_graphics("./00 Fotos/parameter 05.png")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FKy1VQskKfm4RovKaJlTC%2Ffile.png?alt=media)

```
knitr::include_graphics("./00 Fotos/parameter 06.png")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FuGvQ1htgE3XEw8ArCND3%2Ffile.png?alt=media)

#### 1.2.2 Beispiel

```
plot(dose, drugA, type = "b", col = "red", lty = 2, 
    pch = 2, lwd = 2, main = "Clinical Trials for Drug A", 
    sub = "This is hypothetical data", 
    xlab = "Dosage", ylab = "Drug Response", xlim = c(0, 60), 
    ylim = c(0, 70))
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FJWKbzEueNnUCIwWGVBB6%2Ffile.png?alt=media)

### 1.3 图形的组合排列

par()或layout()可以容易地组合多幅图形为一幅总括图形

#### 1.3.1 par()

```
attach(mtcars)
opar <- par(no.readonly = TRUE)
par(mfrow = c(2, 2))
plot(wt, mpg, main = "Scatterplot of wt vs. mpg")
plot(wt, disp, main = "Scatterplot of wt vs disp")
hist(wt, main = "Histogram of wt")
boxplot(wt, main = "Boxplot of wt")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FOwnf45H95WNbEPjIUD93%2Ffile.png?alt=media)

```
par(opar)
detach(mtcars)

## 3行1列排布3幅图形
attach(mtcars)
opar <- par(no.readonly = TRUE)
par(mfrow = c(3, 1))
hist(wt)
hist(mpg)
hist(disp)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Fk7xQRQtsoEwuGMcMyqyP%2Ffile.png?alt=media)

```
par(opar)
detach(mtcars)
```

#### 1.3.2 layout()

```
## layout
## 把设备划分为4个相等的部分
layout(matrix(1:4, 2, 2))  
## 看到创建的分割,自变量是子窗口的个数
layout.show(4)             
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2F6mM6j6o1Be09rwT7GUxH%2Ffile.png?alt=media)

```
layout(matrix(1:6, 2, 3))
layout.show(6)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FmDL1gdEUep5nEPEy7x21%2Ffile.png?alt=media)

```
layout(matrix(c(1:3, 3), 2, 2))
layout.show(3)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FmpoWOQH43ZYyRXLf9cHe%2Ffile.png?alt=media)

```
## 恢复设置
dev.off() 
```

```
## null device 
##           1
```

```
## 函数layout()的调用形式为layout(mat)，其中的mat是一个矩阵，它指定了所要组合的多个图形的所在位置。在以下代码中，一幅图被置于第1行，另两幅图则被置于第2行
attach(mtcars)
layout(matrix(c(1, 1, 2, 3), 2, 2, byrow = TRUE))
hist(wt)
hist(mpg)
hist(disp)
detach(mtcars)
dev.off() 
```

```
## null device 
##           1
```

## 2 Scatter plot

```
# bg 指定背景色(例如bg="red", bg="blue"; 用colors()可以显示657种可用的颜 色名)
# col 控制符号的颜色;和cex类似，还可用:col.axis, col.lab, col.main, col.sub
# font 控制文字字体的整数(1: 正常，2: 斜体，3: 粗体，4: 粗斜体);和cex类似， 还可用: font.axis, font.lab, font.main, font.sub
# lty 控制连线的线型，可以是整数(1: 实线，2: 虚线，3: 点线，4: 点虚 线，5: 长虚线，6: 双虚线)
# pch 控制符号的类型，可以是1到25的整数，也可以是""里的单个字符
x <- rnorm(10)
y <- rnorm(10)
plot_a <- plot(x,y)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FccE0505BTjWDq0mV0sTU%2Ffile.png?alt=media)

```
plot_b <- plot(x, y, xlab="Ten random values", ylab="Ten other values",
               xlim=c(-2, 2), ylim=c(-2, 2), pch=22, col="red",
               bg="yellow", bty="l", tcl=0.4,
               main="How to customize a plot with R", las=1, cex=1.5)

## low-level plotting commands 低级绘图命令 作用于现存的图形上
## low-level plotting commands 低级绘图命令 作用于现存的图形上
points(x, y)      # 添加点(可以使用选项type=)
lines(x, y)       # 同上，但是添加线

abline(0,1)       # 绘制斜率为b和截距为a的直线
abline(h=1)       # 在纵坐标y处画水平线
abline(v=1)       # 在横坐标x处画垂直线
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2F1yBsvB1MfCirJYIO9iut%2Ffile.png?alt=media)

```
# abline(lm.obj)  # 画由lm.obj确定的回归线
```

## 3 Barplot

```
dev.off()
```

```
## null device 
##           1
```

```
data(Titanic)
barplot(ftable(Titanic, row.vars=1, col.vars=4),
             names.arg=dimnames(Titanic)$Survived,
             beside=TRUE, xlab="Survived")
legend("topright", 
       legend = dimnames(Titanic)$Class, 
       fill = 1:4, ncol = 1,
       cex = 0.65)
```

## 4 Histogramm

```
## Vertical axis is count
attach(faithful)
hist(eruptions)                       
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FMsUfh3FBq0CD3ieMn1un%2Ffile.png?alt=media)

```
## Vertical axis is frequence
hist(eruptions,freq = FALSE)          
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FL0jg2Y11NFMZ3WwiaRTE%2Ffile.png?alt=media)

```
## breaks 让箱距缩小，绘制密度图
Y <- hist(eruptions, breaks=seq(1.6, 5.2, 0.2), prob=TRUE)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FfjI7j5T66316KHyCRHSl%2Ffile.png?alt=media)

```
Y$density
```

```
##  [1] 0.29411765 0.71691176 0.34926471 0.27573529 0.05514706 0.05514706
##  [7] 0.03676471 0.01838235 0.07352941 0.18382353 0.11029412 0.40441176
## [13] 0.47794118 0.53308824 0.69852941 0.44117647 0.22058824 0.05514706
```

```
Y$counts
```

```
##  [1] 16 39 19 15  3  3  2  1  4 10  6 22 26 29 38 24 12  3
```

```
## col fill the color
hist(eruptions, breaks=seq(1.6, 5.2, 0.2), 
     prob=TRUE,right=F, col="darkgray",main="Männer <= 18")

## add the density line
lines(density(eruptions, bw=0.1),col="red",lty=2)     
lines(density(eruptions, bw= "SJ" ))  # 自动的带宽bw（bandwidth)
## 显示实际的数据点
rug(eruptions)                        
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Fl0MIJLWfQYFpHRfdeHFC%2Ffile.png?alt=media)

```
## 经验累积分布（empirical cumulativedistribution）函数
plot(ecdf(eruptions), do.points=FALSE, verticals=TRUE)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FNXWwfR10L6CJ566MicW4%2Ffile.png?alt=media)

```
## 重新正态拟合爆发三分钟后的状况
long <- eruptions[eruptions > 3]
hist(long, seq(1.6, 5.2, 0.2), prob=TRUE)
## 正态分布概率密度曲线
x <- seq(3, 5.4, 0.01)
curve(dnorm(x,mean(long),sd(long)), add=TRUE)
lines(x,dnorm(x,mean(long),sd(long)),col="green") 
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Ficl9nNRw4iNLi9fpdWCw%2Ffile.png?alt=media)

```
## Experience cumulative distribution
plot(ecdf(long), do.points=FALSE, verticals=TRUE) 
lines(x, pnorm(x, mean=mean(long), sd=sqrt(var(long))), lty=3)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2F0OB8DDuDpAQbkFZ4zXuq%2Ffile.png?alt=media)

```
detach()
```

## 5 Boxplot

```
data(iris)
opar <- par()
par(mfrow=c(2,2))
for (i in 1:4){
  boxplot(iris[,i]~iris[,5], outline=FALSE, col="gray90", boxwex=0.5)
}
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FvlNQVije440EgKfFMFnA%2Ffile.png?alt=media)

## 6 Pie Chart

```
dev.off()
set.seed(123)
klasse <- sample(1:6, size = 60, replace = TRUE, prob = c(.05, .2, .5, .2, .03, .02))
table1 <- table(klasse)
relativ_table1 <- round(prop.table(table(klasse))*100,1)

pie(table1,col=c("aquamarine", "cadetblue1", "cyan1","dodgerblue", "darkslategray1", "lightskyblue"),
    labels=relativ_table1,
    radius = 0.8, clockwise = TRUE, 
    main="Präsenzaufgabe 1(c) Pieplot %")
```
