I've been using Everedoo to ask me two daily questions each day since September. "How was your day?" and "How much sleep did you get?"

``` r
library(tidyverse)
library(readr)
library(kableExtra)
library(plotly)
library(viridis)

Satisfaction <- read_csv("export/Satisfaction.csv")
Sleep <- read_csv("export/Sleep.csv")

Combined  <- merge(Sleep, Satisfaction, by = "Date")

names(Combined) <- c("Date", "Sleep.Entry", "Sleep", "Satisfaction.Entry", "Satisfaction")

Combined$betterDate <- as.Date(Combined$Date, "%m/%d/%y")

Combined$Date <- as.Date(ifelse(Combined$betterDate > Sys.Date(), 
  format(Combined$betterDate, "19%y-%m-%d"), 
  format(Combined$betterDate)))

ggplot(Combined, aes(x= Date, y = Satisfaction, color = as.integer(Date))) + geom_line(,size = 3)+ geom_point()+  ylim(0,10)+theme_classic() + scale_color_viridis_c() + theme(legend.position = "none",text = element_text(size=20)) + labs(y="Satisfaction on a 1-10 Scale", title = "How was your day? (5:00pm)")
```

![](Daily_Satisfaction_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
ggplot(Combined, aes(x= Date, y = Sleep, color = as.integer(Date))) + geom_line(size = 3)+ geom_point()+  ylim(0,10)+theme_classic() + scale_color_viridis_c() + theme(legend.position = "none",text = element_text(size=20)) + labs(y="Approximate Sleep in Hours", title = "How much sleep did you get?")
```

![](Daily_Satisfaction_files/figure-markdown_github/unnamed-chunk-1-2.png)

``` r
Gathered <- Combined %>%
  select(Date,Satisfaction,Sleep) %>%
  gather(key = Type,value = Amount,-Date)

ggplot(Gathered, aes(x=Date, y= Amount, color = Type)) + geom_line() +theme_classic() + ylim(0,10) + theme(text = element_text(size=20), legend.position = "none")
```

![](Daily_Satisfaction_files/figure-markdown_github/unnamed-chunk-1-3.png)

``` r
ggplot(Combined, aes(x= Sleep, y = Satisfaction)) + geom_jitter(aes(color = as.integer(Date)))+ xlim(0,10) + ylim(0,10) +  geom_smooth(method='lm',formula=y~x,se = FALSE, color = "lightgrey")+theme_classic() + theme(legend.position = "none",text = element_text(size=20))+ scale_color_viridis_c()
```

![](Daily_Satisfaction_files/figure-markdown_github/unnamed-chunk-1-4.png)
