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
library(ggtext)
```

```
## Warning: package 'ggtext' was built under R version 4.0.4
```

```r
library(patchwork)
library(gt)            # for creating nice tables
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

#View(dataset)
```

COVID-19 Implications on Mental Health in the U.S.

Within the United States, the rise of remote learning has called for additional attention on students’ mental health as they experience a lack of social interaction, less direct support from teachers, and difficulty focusing at home. Aside from academics, the mental well-being of all youth in general has also been negatively affected as children and their families are asked to self-quarantine and in some cases, leave their jobs. Health experts are now concerned about the mental health conditions for our youth in the long run. They believe that experiencing and living in these tough situations for an extended amount of time can cause children to have anxiety and depression which is why we need to start paying close attention to negative impacts of COVID-19 on mental health.  For more information regarding this issue, you may read the article provided in this [link](https://www.writingcity.com/how-remote-learning-affects-students-mental-health.html)

Implications in India

As Americans, we have seen and experienced the pandemic’s implications on mental health within the United States, but it leaves us quite curious about the present circumstances elsewhere. While searching for new research and data sets, we stumbled upon a relatively recent research done in Delhi, the capital territory of India. Data was also collected from subjects living in the National Capital Region (NCR) which encompasses both Delhi and its surrounding area. In this study, researchers looked at different age-groups of a total of 1,182 subjects, ages ranging from 7 to 59 years old, and how several aspects of their lives were affected after the lockdown. Additionally, they recorded the different coping mechanisms adopted due to such sudden changes. The various variables such as learning hours for online classes and self-study, duration of sleep, time spent on fitness and sleep were recorded and analysed as factors related to mental health. Although the effect on students’ education, social life, physical health, and mental well-being was expected, this research suggests that the public should take necessary measures to prevent psychological problems and improve students’ experiences in and outside of academics, for our current results are not meeting the expectations of the initial government policies. For specific details on the demographics, objectives, and methods of this study, please read the research paper linked [here] (https://www.researchgate.net/publication/347935769_COVID-19_and_its_impact_on_education_social_life_and_mental_health_of_students_A_Survey) 

Although the researchers in this study did a phenomenal job at creating, designing, and interpreting their own plots, we decided to ask different questions and explore our own interests by using the same data set while still acknowledging their remarkable findings. 


**Demographics:**

Before exploring the data set, it's important to first acknowledge that there is indeed a demographic imbalance. Of the 1,182 respondents, the research paper states that 84.3% of those subjects were 7 to 22 years of age with the mean age being 20.16 years old. This bar plot visually demonstrates that most of the data comes from school-age children and teenagers. This imbalance is understandable considering that the aim of this study was to research COVID-19 implications on the education, social life, and mental health of students. However, comparing different variables across multiple age-groups gets quite complicated and can be misleading if not carefully examined.  



```r
#demographics
ggplotly(dataset %>% 
  ggplot(aes(x = Age)) +
  geom_bar(fill = "steelblue1") +
  scale_x_continuous(name = "", breaks = c(5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60)) +
  labs(title = "Number of Survey Respondents by Age", x = "Age", y = "") +
  theme(axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot", 
        strip.text.x = element_blank()))
```

```{=html}
<div id="htmlwidget-c3e946c75c98d2aac042" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c3e946c75c98d2aac042">{"x":{"data":[{"orientation":"v","width":[0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.900000000000002,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,42,43,44,45,46,50,52,59],"y":[1,2,2,4,6,29,48,48,37,67,58,66,138,211,188,93,44,23,12,12,6,7,4,8,4,10,5,10,4,5,2,4,1,14,1,2,1,1,1,1,1,1],"text":["count:   1<br />Age:  7","count:   2<br />Age:  8","count:   2<br />Age:  9","count:   4<br />Age: 10","count:   6<br />Age: 11","count:  29<br />Age: 12","count:  48<br />Age: 13","count:  48<br />Age: 14","count:  37<br />Age: 15","count:  67<br />Age: 16","count:  58<br />Age: 17","count:  66<br />Age: 18","count: 138<br />Age: 19","count: 211<br />Age: 20","count: 188<br />Age: 21","count:  93<br />Age: 22","count:  44<br />Age: 23","count:  23<br />Age: 24","count:  12<br />Age: 25","count:  12<br />Age: 26","count:   6<br />Age: 27","count:   7<br />Age: 28","count:   4<br />Age: 29","count:   8<br />Age: 30","count:   4<br />Age: 31","count:  10<br />Age: 32","count:   5<br />Age: 33","count:  10<br />Age: 34","count:   4<br />Age: 35","count:   5<br />Age: 36","count:   2<br />Age: 37","count:   4<br />Age: 38","count:   1<br />Age: 39","count:  14<br />Age: 40","count:   1<br />Age: 42","count:   2<br />Age: 43","count:   1<br />Age: 44","count:   1<br />Age: 45","count:   1<br />Age: 46","count:   1<br />Age: 50","count:   1<br />Age: 52","count:   1<br />Age: 59"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(99,184,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":28.4931506849315},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Number of Survey Respondents by Age","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[3.905,62.095],"tickmode":"array","ticktext":["5","10","15","20","25","30","35","40","45","50","55","60"],"tickvals":[5,10,15,20,25,30,35,40,45,50,55,60],"categoryorder":"array","categoryarray":["5","10","15","20","25","30","35","40","45","50","55","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-10.55,221.55],"tickmode":"array","ticktext":["0","50","100","150","200"],"tickvals":[0,50,100,150,200],"categoryorder":"array","categoryarray":["0","50","100","150","200"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"20dc77a87366":{"x":{},"type":"bar"}},"cur_data":"20dc77a87366","visdat":{"20dc77a87366":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

```r
dataset %>% 
  count(AgeCat) %>% 
  mutate(prop = n/sum(n)) %>% 
  gt(rowname_col = "AgeCat")%>% 
  tab_header("Respondents by Age Group") %>% 
  tab_stubhead(label="Age Group") %>%
  fmt_percent(columns = vars(prop), decimals = 3) %>% 
  cols_label(n = "Respondents", prop="Proportion") %>% 
  summary_rows(columns = vars(AgeCat, n),
    fns = list(
      total = ~sum(., na.rm = TRUE)),decimals = 0)
```

```{=html}
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#djghyskacf .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#djghyskacf .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#djghyskacf .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#djghyskacf .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#djghyskacf .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#djghyskacf .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#djghyskacf .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#djghyskacf .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#djghyskacf .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#djghyskacf .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#djghyskacf .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#djghyskacf .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#djghyskacf .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#djghyskacf .gt_from_md > :first-child {
  margin-top: 0;
}

#djghyskacf .gt_from_md > :last-child {
  margin-bottom: 0;
}

#djghyskacf .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#djghyskacf .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#djghyskacf .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#djghyskacf .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#djghyskacf .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#djghyskacf .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#djghyskacf .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#djghyskacf .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#djghyskacf .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#djghyskacf .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#djghyskacf .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#djghyskacf .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#djghyskacf .gt_left {
  text-align: left;
}

#djghyskacf .gt_center {
  text-align: center;
}

#djghyskacf .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#djghyskacf .gt_font_normal {
  font-weight: normal;
}

#djghyskacf .gt_font_bold {
  font-weight: bold;
}

#djghyskacf .gt_font_italic {
  font-style: italic;
}

#djghyskacf .gt_super {
  font-size: 65%;
}

#djghyskacf .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="djghyskacf" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="3" class="gt_heading gt_title gt_font_normal" style>Respondents by Age Group</th>
    </tr>
    <tr>
      <th colspan="3" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style></th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">Age Group</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">Respondents</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">Proportion</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left gt_stub">0-13</td>
      <td class="gt_row gt_center">92</td>
      <td class="gt_row gt_right">7.783&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_left gt_stub">13-18</td>
      <td class="gt_row gt_center">276</td>
      <td class="gt_row gt_right">23.350&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_left gt_stub">18-23</td>
      <td class="gt_row gt_center">674</td>
      <td class="gt_row gt_right">57.022&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_left gt_stub">23-33</td>
      <td class="gt_row gt_center">91</td>
      <td class="gt_row gt_right">7.699&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_left gt_stub">33-45</td>
      <td class="gt_row gt_center">45</td>
      <td class="gt_row gt_right">3.807&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_left gt_stub">45-60</td>
      <td class="gt_row gt_center">4</td>
      <td class="gt_row gt_right">0.338&percnt;</td>
    </tr>
    <tr>
      <td class="gt_row gt_stub gt_right gt_grand_summary_row gt_first_grand_summary_row">total</td>
      <td class="gt_row gt_center gt_grand_summary_row gt_first_grand_summary_row">1,182</td>
      <td class="gt_row gt_right gt_grand_summary_row gt_first_grand_summary_row">&mdash;</td>
    </tr>
  </tbody>
  
  
</table></div>
```






Just as we observed with the previous plot, the number of respondents per region of residence also differs. This plot displays that there is indeed quite a significant difference between Delhi-NCR respondents and respondents living outside of the Delhi-NCR. Although the online-survey was originally aimed at institutions within the Delhi-NCR region, a significant number of responses from outside of Delhi-NCR was received, for distance-learning regulations forced many students to move out of homes and institutions to reside elsewhere. Of the total number of respondents, 38.3% were living outside of the Delhi-NCR.


```r
#demographics
dataset %>% 
  ggplot(aes(x = Region)) +
  geom_bar(fill = "steelblue1") +
  labs(title = "Number of Survey Respondents by Region", x = "", y = "") +
  theme(panel.grid = element_blank(), 
        plot.title.position= "plot", 
        strip.text.x = element_blank())
```

![](dsproject_gems_files/figure-html/unnamed-chunk-3-1.png)<!-- -->





```r
#not from test file
dataset %>% 
  ggplot(aes(x = Age, y = Sleep)) +
  geom_jitter() +
  # facet_wrap(vars(Age)) +
  labs(title = "Age and Sleep")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

This plot compares the median number of hours spent in class by different age groups. For the sole purpose of this study, it is important to draw attention to how many hours students are spending sitting in front of a screen each day. From this plot, we can observe that respondents within the 0-13 and 13-18 age-groups spend the most time in class per day. The researchers emphasized that different variables (time spent on online class, self-study,fitness, sleep, and social media) changes with different age distributions. Average time spent in class and age-group was one of the relationships that were inversely proportional; As age-group value increases, the average number of hours spent online starts to decrease. 




```r
dataset %>%
  group_by(AgeCat, Connection, Region) %>% 
  ggplot(aes(x = AgeCat,
             y = OnlineClass)) +
  geom_boxplot(color = "steelblue", fill = "steelblue1", alpha = .5) +
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
sleepmedia_anim <- dataset %>% 
  group_by(SocialMedia, avgsleep, AgeCat, Sleep) %>%
  summarise(avgmedia = mean(SocialMedia)) %>%
  ggplot(aes(x = Sleep, 
             y = SocialMedia, 
             color = AgeCat, 
             group = AgeCat)) +
  geom_jitter() +
  labs(y = "", 
       x = "Hours of Sleep", 
       title = "Amount of Sleep and Corresponding Social Media Use in Hours by Age Category", 
       subtitle = "Ages: {closest_state}") +
  transition_states(AgeCat)+
  theme(axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot", 
        legend.position = "none")+
  scale_color_viridis_d() +
  exit_fade() +
  enter_recolor(color = "aliceblue") +
  exit_recolor(color = "aliceblue")
```

```
## `summarise()` has grouped output by 'SocialMedia', 'avgsleep', 'AgeCat'. You can override using the `.groups` argument.
```

```r
animate(sleepmedia_anim, duration = 20)
```

```
## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion

## Warning in lapply(data, as.numeric): NAs introduced by coercion
```

![](dsproject_gems_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("sleepmedia_anim.gif")
```


```r
knitr::include_graphics("sleepmedia_anim.gif")
```

![](sleepmedia_anim.gif)<!-- -->


```r
dataset %>%
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>%
  #filter(`What respondents miss the most` != "Other")  %>%
  count(`What respondents miss the most`, sort=TRUE) %>% 
  gt(rowname_col = "`What respondents miss the most`")%>% 
  tab_header("What Respondents Miss Most") %>% 
  cols_label(`What respondents miss the most`="",
             n = "")
```

```{=html}
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#vjglswfgtr .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#vjglswfgtr .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#vjglswfgtr .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#vjglswfgtr .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#vjglswfgtr .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#vjglswfgtr .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#vjglswfgtr .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#vjglswfgtr .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#vjglswfgtr .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#vjglswfgtr .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#vjglswfgtr .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#vjglswfgtr .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#vjglswfgtr .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#vjglswfgtr .gt_from_md > :first-child {
  margin-top: 0;
}

#vjglswfgtr .gt_from_md > :last-child {
  margin-bottom: 0;
}

#vjglswfgtr .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#vjglswfgtr .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#vjglswfgtr .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#vjglswfgtr .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#vjglswfgtr .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#vjglswfgtr .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#vjglswfgtr .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#vjglswfgtr .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#vjglswfgtr .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#vjglswfgtr .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#vjglswfgtr .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#vjglswfgtr .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#vjglswfgtr .gt_left {
  text-align: left;
}

#vjglswfgtr .gt_center {
  text-align: center;
}

#vjglswfgtr .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#vjglswfgtr .gt_font_normal {
  font-weight: normal;
}

#vjglswfgtr .gt_font_bold {
  font-weight: bold;
}

#vjglswfgtr .gt_font_italic {
  font-style: italic;
}

#vjglswfgtr .gt_super {
  font-size: 65%;
}

#vjglswfgtr .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="vjglswfgtr" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="2" class="gt_heading gt_title gt_font_normal" style>What Respondents Miss Most</th>
    </tr>
    <tr>
      <th colspan="2" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style></th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_center">School/college</td>
      <td class="gt_row gt_center">368</td>
    </tr>
    <tr>
      <td class="gt_row gt_center">Friends , relatives</td>
      <td class="gt_row gt_center">213</td>
    </tr>
    <tr>
      <td class="gt_row gt_center">Travelling</td>
      <td class="gt_row gt_center">169</td>
    </tr>
    <tr>
      <td class="gt_row gt_center">Roaming around freely</td>
      <td class="gt_row gt_center">143</td>
    </tr>
    <tr>
      <td class="gt_row gt_center">Other</td>
      <td class="gt_row gt_center">137</td>
    </tr>
    <tr>
      <td class="gt_row gt_center">Eating outside</td>
      <td class="gt_row gt_center">99</td>
    </tr>
  </tbody>
  
  
</table></div>
```

```r
ggplotly(dataset %>%
           drop_na() %>%
           mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
           ggplot()+
           geom_bar(aes(x=`What respondents miss the most`), fill="lightblue") +
           labs(x = "", 
                y = "", 
                title = "What Respondents Miss the Most") +
           theme(panel.grid = element_blank(), 
                 plot.title.position= "plot",
                 legend.position="none"))
```

```{=html}
<div id="htmlwidget-65ddcf8842a087a8689d" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-65ddcf8842a087a8689d">{"x":{"data":[{"orientation":"v","width":[0.9,0.9,0.9,0.9,0.9,0.9],"base":[0,0,0,0,0,0],"x":[1,2,3,4,5,6],"y":[99,213,143,368,169,137],"text":["count:  99<br />What respondents miss the most: Eating outside","count: 213<br />What respondents miss the most: Friends , relatives","count: 143<br />What respondents miss the most: Roaming around freely","count: 368<br />What respondents miss the most: School/college","count: 169<br />What respondents miss the most: Travelling","count: 137<br />What respondents miss the most: Other"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(173,216,230,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":28.4931506849315},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"What Respondents Miss the Most","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,6.6],"tickmode":"array","ticktext":["Eating outside","Friends , relatives","Roaming around freely","School/college","Travelling","Other"],"tickvals":[1,2,3,4,5,6],"categoryorder":"array","categoryarray":["Eating outside","Friends , relatives","Roaming around freely","School/college","Travelling","Other"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-18.4,386.4],"tickmode":"array","ticktext":["0","100","200","300"],"tickvals":[0,100,200,300],"categoryorder":"array","categoryarray":["0","100","200","300"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"20dc71a923a5":{"x":{},"type":"bar"}},"cur_data":"20dc71a923a5","visdat":{"20dc71a923a5":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```


```r
dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  ggplot()+
  geom_bar(position="dodge", aes(x=AgeCat, fill=Region))+
  facet_wrap(vars(`What respondents miss the most`)) +
  labs(
    title = "What Respondents Miss Most by Age Group
    <span style='color:#F8766D;'>in Dehli</span>
    <span>and 
    <span style='color:#00BFC4;'>Outside Dehli</span>",
    x = "Age Group", y = "") +
  theme_minimal() +
  theme(plot.title = element_markdown(),
        legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
#within AgeCat=0-13, what percentage said what:
ag1 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="0-13") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/89*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 0-13") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")

#within AgeCat=14-18, what percentage said what:
ag2 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="14-18") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/265*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 14-18") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")

#within AgeCat=19-23, what percentage said what:
ag3 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="19-23") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/645*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 19-23") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")

#within AgeCat=24-33, what percentage said what:
ag4 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="24-33") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/84*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 24-33") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")

#within AgeCat=34-45, what percentage said what:
ag5 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="34-45") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/42*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 34-45") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")

#within AgeCat=46-60, what percentage said what:
ag6 <- dataset %>% 
  drop_na() %>% 
  mutate(`What respondents miss the most` = fct_lump_n(`What you miss the most`, 5)) %>% 
  filter(AgeCat=="46-60") %>%
  group_by(AgeCat,`What respondents miss the most`) %>% 
  count(sort=TRUE)  %>%
  mutate(prop=n/4*100) %>% 
  ggplot()+
  geom_col(position="dodge", aes(x=`What respondents miss the most`, y=prop), fill="dodgerblue") +
  labs(x = "", 
       y = "", 
       title = "Ages 46-60") +
  theme(legend.position = "none",
        axis.line = element_blank(), 
        panel.grid = element_blank(), 
        plot.title.position= "plot")
```







```r
dataset %>% 
  select(AgeCat, SocialMedia, Connection) %>% 
  group_by(AgeCat, Connection) %>% 
  summarize(avg_time_sm = mean(SocialMedia)) %>% 
  ggplot(aes(x = AgeCat, y = avg_time_sm,
             fill = Connection)) +
  geom_col(position = "dodge") +
  #geom_boxplot() +
  labs(title = "Average Hours Spent On Social Media and Connection With Loved Ones Per Age Group",
       x = "",
       y = "") +
  theme(plot.title = element_text(size = 12))
```

```
## `summarise()` has grouped output by 'AgeCat'. You can override using the `.groups` argument.
```

![](dsproject_gems_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

With the pandemic, students are forced to quarantine at home which results in less face-to-face interactions and more time on social media (if it's accessible to them). In the graph below, we used the data set to see if there is a strong relationship between the average time spent on social media per day and whether or not they feel connected to their loved ones. Interestingly, the plot shows that between the different age groups, the results vary, and the average time spent on social media does not exactly determine whether or not there is a connection with loved ones. This can demonstrate that the connections made with loved ones do not merely come from social media interactions, nor are they a result of having to stay home with loved ones everyday. Indeed, that connection can be a result of anything depending on the individual. 



```r
#pacth together graphs, have the mess around with the size when knitting to make it readable, we can also make it less categories if we want
(ag1|ag2) / (ag3|ag4) / (ag5 |ag6) +
  plot_annotation("What Respondents Miss the Most: Proportionally by Age Group")
```

![](dsproject_gems_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

