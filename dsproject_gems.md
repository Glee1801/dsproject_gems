---
title: "Data Science Project"
author: "Gaocha Lee, Elizabeth Cain, Maya Fernandez, and Stephanie Konadu-Acheampong "
date: "3/5/2021"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
    code_folding: hide
---






```r
library(readr)
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v dplyr   1.0.3
## v tibble  3.0.5     v stringr 1.4.0
## v tidyr   1.1.2     v forcats 0.5.0
## v purrr   0.3.4
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(maps)          
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1
```

```r
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
```

```
## 
## Attaching package: 'plotly'
```

```
## The following object is masked from 'package:ggmap':
## 
##     wind
```

```
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```

```
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
library(gganimate)     # for adding animation layers to ggplots
library(transformr)
```

```
## 
## Attaching package: 'transformr'
```

```
## The following object is masked from 'package:sf':
## 
##     st_normalize
```

```r
library(ggimage)
```

```
## Warning: package 'ggimage' was built under R version 4.0.4
```

```
## 
## Attaching package: 'ggimage'
```

```
## The following object is masked from 'package:ggmap':
## 
##     theme_nothing
```

```r
theme_set(theme_minimal())
```



```r
dataset <- read_csv("Data.csv") %>% 
  mutate(AgeCat = cut(Age, breaks = c(0,13,18,23,33,45,60), labels = c("0-13", "13-18", "18-23", "23-33", "33-45", "45-60"))) %>%
  mutate(avgsleep = mean(Sleep)) %>% 
  mutate(SocialCat = cut_number(SocialMedia, n = 3))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   ID = col_character(),
##   Region = col_character(),
##   Age = col_double(),
##   OnlineClass = col_double(),
##   ClassRating = col_character(),
##   ClassMedium = col_character(),
##   SelfStudy = col_double(),
##   Fitness = col_double(),
##   Sleep = col_double(),
##   SocialMedia = col_double(),
##   SocialMediaPlatform = col_character(),
##   TV = col_character(),
##   Meals = col_double(),
##   WeightChanged = col_character(),
##   HealthIssue = col_character(),
##   `Stress busters` = col_character(),
##   `Time utilized` = col_character(),
##   Connection = col_character(),
##   `What you miss the most` = col_character()
## )
```

```r
#labels = "0-1", "1-3", "3-10" label social media 

View(dataset)
```

COVID-19 Implications on Mental Health in the U.S.

Within the United States, the rise of remote learning has called for additional attention on students’ mental health as they experience a lack of social interaction, less direct support from teachers, and difficulty focusing at home. Aside from academics, the mental well-being of all youth in general has also been negatively affected as children and their families are asked to self-quarantine and in some cases, leave their jobs. Health experts are now concerned about the mental health conditions for our youth in the long run. They believe that experiencing and living in these tough situations for an extended amount of time can cause children to have anxiety and depression which is why we need to start paying close attention to negative impacts of COVID-19 on mental health.  For more information regarding this issue, you may read the article provided in this [link](https://www.writingcity.com/how-remote-learning-affects-students-mental-health.html)

Implications in India

As Americans, we have seen and experienced the pandemic’s implications on mental health within the United States, but it leaves us quite curious about the present circumstances elsewhere. While searching for new research and data sets, we stumbled upon a relatively recent research done in Delhi, the capital territory of India. Data was also collected from subjects living in the National Capital Region (NCR) which encompasses both Delhi and its surrounding area. In this study, researchers looked at different age-groups of a total of 1,182 subjects, ages ranging from 7 to 59 years old, and how several aspects of their lives were affected after the lockdown. Additionally, they recorded the different coping mechanisms adopted due to such sudden changes. The various variables such as learning hours for online classes and self-study, duration of sleep, time spent on fitness and sleep were recorded and analysed as factors related to mental health. Although the effect on students’ education, social life, physical health, and mental well-being was expected, this research suggests that the public should take necessary measures to prevent psychological problems and improve students’ experiences in and outside of academics, for our current results are not meeting the expectations of the initial government policies. For specific details on the demographics, objectives, and methods of this study, please read the research paper linked [here] (https://www.researchgate.net/publication/347935769_COVID-19_and_its_impact_on_education_social_life_and_mental_health_of_students_A_Survey) 

Although the researchers in this study did a phenomenal job at creating, designing, and interpreting their own plots, we decided to ask different questions and explore our own interests by using the same data set while still acknowledging their remarkable findings. 


```r
#demographics
dataset %>% 
  ggplot(aes(x = Age)) +
  geom_bar(fill = "steelblue1") +
  scale_x_continuous(name = "", breaks = c(5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60)) +
  labs(title = "Number of Survey Respondents by Age", x = "Age", y = "")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

We noticed that most of the survey respondents were young, etc. From research paper: the total number of respondents was ___. 


```r
#demographics
dataset %>% 
  ggplot(aes(x = Region)) +
  geom_bar(fill = "steelblue1") +
  labs(title = "Number of Survey Respondents by Region", x = "", y = "")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

Observations about this plot. Describe NCR (National Capital Region), because Delhi is the Capital of India. The city is the center and the NCR is slightly larger than the urban area itself. 



```r
#not from test file
dataset %>% 
  ggplot(aes(x = Age, y = Sleep)) +
  geom_jitter() +
  # facet_wrap(vars(Age)) +
  labs(title = "Age and Sleep")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-4-1.png)<!-- -->




```r
#in project file 
#preferred graph 

dataset %>%
  group_by(AgeCat, Connection, Region) %>% 
  ggplot(aes(x = AgeCat,
             y = OnlineClass)) +
  geom_boxplot() +
  labs(x = "Age", 
       y = "", 
       title = "Comparing Hours Spent in Class Per Day by Age") +
  theme(axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot", 
        strip.text.x = element_blank())
```

![](dsproject_gems_files/figure-html/unnamed-chunk-5-1.png)<!-- -->





```r
dataset %>% 
  ggplot(aes(x = SocialCat, 
             y = Sleep, 
             color = AgeCat)) +
  geom_boxplot() +
  # facet_wrap(vars(AgeCat)) +
  labs(y = "", 
       x = "", 
       title = "Average Amount of Sleep and Media Use in Hours by Age Category") +
  theme(axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot", 
        legend.position = "none")+
  scale_color_viridis_d()
```

![](dsproject_gems_files/figure-html/unnamed-chunk-6-1.png)<!-- -->



```r
#scatterplot 

dataset %>% 
  group_by(SocialMedia, avgsleep, AgeCat, Sleep) %>%
  summarise(avgmedia = mean(SocialMedia)) %>%
  ggplot(aes(x = avgmedia, 
             y = avgsleep, 
             color = AgeCat)) +
  geom_jitter() +
  facet_wrap(vars(AgeCat)) +
  labs(y = "", 
       x = "Average Hours of Media Use", 
       title = "Average Amount of Sleep and Media Use in Hours by Age Category") +
  theme(axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot", 
        legend.position = "none")+
  scale_color_viridis_d()
```

```
## `summarise()` has grouped output by 'SocialMedia', 'avgsleep', 'AgeCat'. You can override using the `.groups` argument.
```

![](dsproject_gems_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


