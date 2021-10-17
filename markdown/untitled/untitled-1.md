# KableExtra



### Package Knitr 

```
library(knitr)
AG_1 <- data.frame(Variable <- c("Intercept","Weinkonsum","Rauchen","Weinkonsum × Rauchen"),
                   Schätzer <- c(-3.3,-0.3,0.6,1.2),
                   OddsRatio <- c(NA,0.74,1.82,3.32))
names(AG_1) <- c("Variable","Schätzer","Odds Ratio")
knitr::kable(AG_1,format = "pandoc”, digits=2, caption="Stopping cars")
```

### Package KableExtra

```
## kable_styling
dt <- mtcars[1:5, 1:6]

## pandoc输出
## kable_styling() will automatically apply twitter bootstrap theme to the table.same as the original pandoc output
dt %>%
  kable() %>%
  kable_styling()
  

kable(dt) %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

|                    |  mpg  |  cyl |  disp |  hp  |  drat |  wt    |
| ------------------ | ----- | ---- | ----- | ---- | ----- | ------ |
|  Mazda RX4         |  21.0 |  6   |  160  |  110 |  3.90 |  2.620 |
|  Mazda RX4 Wag     |  21.0 |  6   |  160  |  110 |  3.90 |  2.875 |
|  Datsun 710        |  22.8 |  4   |  108  |  93  |  3.85 |  2.320 |
|  Hornet 4 Drive    |  21.4 |  6   |  258  |  110 |  3.08 |  3.215 |
|  Hornet Sportabout |  18.7 |  8   |  360  |  175 |  3.15 |  3.440 |

```
  ## Table Styles:add striped lines "striped" 添加带条纹
  ## highlight the hovered row,"hover"
  ## 不希望表格太大,"condensed"
  ## 响应选项，可以适应窗口滚动--horizontally scrollable，“responsive”


kable(dt) %>%
  kable_styling(bootstrap_options = "striped", full_width = F)
```
