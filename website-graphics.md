website-graphics
================
Dr. Maria Tackett
12.17.2018

This document contains the code required to make the background images for the STA 199 course webiste. The data used for this background is the [Capital Bikeshare dataset](https://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset), obtained from the UCI Machine Learning Repository.

``` r
library(tidyverse)
library(readr)
library(cowplot)
```

``` r
bikeshare <- read_csv("https://raw.githubusercontent.com/matackett/data/master/capital-bikeshare.csv")                          
```

``` r
bikeshare <- bikeshare %>%
  mutate(season = case_when(
    season==1 ~ "Winter",
    season==2 ~ "Spring",
    season==3 ~ "Summer",
    season==4 ~ "Fall"
  ))
```

``` r
# create four clusters based on bike count and temperature
x <- bikeshare %>%
  select(atemp,cnt)
cl <- kmeans(x,4)
```

``` r
a.level = 0.5
x.plot <- bind_cols(x,data.frame(cl$cluster))

p1 <- ggplot(data=x.plot,aes(x=cnt,y=atemp,color=factor(cl.cluster))) +
  geom_point(alpha=a.level) +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )

p2 <- ggplot(data=bikeshare,aes(x=cnt,fill=season)) +
  geom_density(alpha=a.level) +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )

p5 <- ggplot(data=bikeshare,aes(x=season,y=cnt,fill=season)) +
  geom_boxplot(alpha=a.level) +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )

p6 <- ggplot(data=bikeshare,aes(x=cnt,fill=season)) +
  geom_area(stat="bin",alpha=a.level,color="black") +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )

p3 <- ggplot(data=bikeshare,aes(x=cnt,y=atemp,color=season)) +
  geom_density2d() +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )

p4 <- ggplot(data=bikeshare,aes(x=atemp,y=cnt,color=season)) +
  geom_smooth(se=FALSE) +
  theme_void() +
  theme(legend.position="none",
        axis.title=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank()
      )
#plot_grid(p1,p2,p2,p1,ncol=2)
plot_grid(p1,p2,p3,p4,p5,p6,ncol=3)
```

![](website-graphics_files/figure-markdown_github/cluster-plot-1.png)

``` r
ggsave("bikeshare-density.png",scale=2)
```