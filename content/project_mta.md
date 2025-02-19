---
title: ""
output:
  html_document:
    code_folding: hide
    df_print: paged
---

<script src="/rmarkdown-libs/kePrint/kePrint.js"></script>
<link href="/rmarkdown-libs/lightable/lightable.css" rel="stylesheet" />
<script src="/rmarkdown-libs/kePrint/kePrint.js"></script>
<link href="/rmarkdown-libs/lightable/lightable.css" rel="stylesheet" />
<script src="/rmarkdown-libs/htmlwidgets/htmlwidgets.js"></script>
<script src="/rmarkdown-libs/d3-bundle/d3-bundle.min.js"></script>
<script src="/rmarkdown-libs/d3-lasso/d3-lasso.min.js"></script>
<script src="/rmarkdown-libs/save-svg-as-png/save-svg-as-png.min.js"></script>
<script src="/rmarkdown-libs/flatbush/flatbush.min.js"></script>
<link href="/rmarkdown-libs/ggiraphjs/ggiraphjs.min.css" rel="stylesheet" />
<script src="/rmarkdown-libs/ggiraphjs/ggiraphjs.min.js"></script>
<script src="/rmarkdown-libs/girafe-binding/girafe.js"></script>

<projtitle>The decline & recovery of MTA ridership during COVID-19</projtitle>

<b>data source</b>: Metropolitan Transportation Authority (via <a href="https://mavenanalytics.io/data-playground">Maven Analytics</a>)
<br>
<b>tools</b>: R (ggplot, ggiraph)

> This page includes all code used to manipulate, analyze, and visualize this dataset.

#### Introduction

The onset of COVID-19 uprooted almost every aspect of everyday life. As lockdown orders took effect, those who could stayed at home rather than venture out for work or fun. In New York City, this manifested as a dramatic reduction in usage of all MTA services — from the subway to buses to the bridges and tunnels that connect each borough.

**The goal of this data exploration is to understand how MTA ridership was impacted by COVID-19 — and how it has since recovered.**

First, let’s load in our data.

<details>
<summary>
Set up workspace & load data
</summary>

``` r
library(tidyverse)
library(lubridate)
library(car)
library(cowplot)
library(ggiraph)
library(DT)
library(kableExtra)

plot_theme <- theme_light() +
  theme(panel.grid = element_blank(),
        strip.background = element_rect(fill = 'grey30'),
        panel.spacing = unit(1, 'lines'))
theme_set(plot_theme)

### load data
mta_data <- read.csv('datasets/MTA_Daily_Ridership.csv', stringsAsFactors = F)
mta_dict <- read.csv('datasets/MTA_data_dictionary.csv', stringsAsFactors = F)

### rename columns & fix date formatting
mta_data <- mta_data %>%
  mutate(Date = as.Date(Date))
colnames(mta_data) <- c('date', 'Subway_current', 'Subway_pre', 'Bus_current', 'Bus_pre', 'LIRR_current', 'LIRR_pre',
                        'MetroNorth_current', 'MetroNorth_pre', 'AccessARide_current', 'AccessARide_pre',
                        'BridgesAndTunnels_current', 'BridgesAndTunnels_pre', 'SIRailway_current', 'SIRailway_pre')
```

</details>

The dataset we start with

<div style="border: 1px solid #ddd; padding: 0px; border: none;overflow-y: scroll; height:auto; overflow-x: scroll; width:100%; ">

<table class=" no-border" style="font-size: 12px; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Subway_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Subway_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Bus_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Bus_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
LIRR_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
LIRR_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
MetroNorth_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
MetroNorth_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
AccessARide_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
AccessARide_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
BridgesAndTunnels_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
BridgesAndTunnels_pre
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
SIRailway_current
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
SIRailway_pre
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-01
</td>
<td style="text-align:right;">
2212965
</td>
<td style="text-align:right;">
97
</td>
<td style="text-align:right;">
984908
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
86790
</td>
<td style="text-align:right;">
100
</td>
<td style="text-align:right;">
55825
</td>
<td style="text-align:right;">
59
</td>
<td style="text-align:right;">
19922
</td>
<td style="text-align:right;">
113
</td>
<td style="text-align:right;">
786960
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:right;">
1636
</td>
<td style="text-align:right;">
52
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-02
</td>
<td style="text-align:right;">
5329915
</td>
<td style="text-align:right;">
96
</td>
<td style="text-align:right;">
2209066
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
321569
</td>
<td style="text-align:right;">
103
</td>
<td style="text-align:right;">
180701
</td>
<td style="text-align:right;">
66
</td>
<td style="text-align:right;">
30338
</td>
<td style="text-align:right;">
102
</td>
<td style="text-align:right;">
874619
</td>
<td style="text-align:right;">
95
</td>
<td style="text-align:right;">
17140
</td>
<td style="text-align:right;">
107
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-03
</td>
<td style="text-align:right;">
5481103
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:right;">
2228608
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
319727
</td>
<td style="text-align:right;">
102
</td>
<td style="text-align:right;">
190648
</td>
<td style="text-align:right;">
69
</td>
<td style="text-align:right;">
32767
</td>
<td style="text-align:right;">
110
</td>
<td style="text-align:right;">
882175
</td>
<td style="text-align:right;">
96
</td>
<td style="text-align:right;">
17453
</td>
<td style="text-align:right;">
109
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-04
</td>
<td style="text-align:right;">
5498809
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
2177165
</td>
<td style="text-align:right;">
97
</td>
<td style="text-align:right;">
311662
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
192689
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
34297
</td>
<td style="text-align:right;">
115
</td>
<td style="text-align:right;">
905558
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:right;">
17136
</td>
<td style="text-align:right;">
107
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-05
</td>
<td style="text-align:right;">
5496453
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:right;">
2244515
</td>
<td style="text-align:right;">
100
</td>
<td style="text-align:right;">
307597
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:right;">
194386
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
33209
</td>
<td style="text-align:right;">
112
</td>
<td style="text-align:right;">
929298
</td>
<td style="text-align:right;">
101
</td>
<td style="text-align:right;">
17203
</td>
<td style="text-align:right;">
108
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-06
</td>
<td style="text-align:right;">
5189447
</td>
<td style="text-align:right;">
93
</td>
<td style="text-align:right;">
2066743
</td>
<td style="text-align:right;">
92
</td>
<td style="text-align:right;">
289171
</td>
<td style="text-align:right;">
92
</td>
<td style="text-align:right;">
205056
</td>
<td style="text-align:right;">
74
</td>
<td style="text-align:right;">
30970
</td>
<td style="text-align:right;">
104
</td>
<td style="text-align:right;">
945408
</td>
<td style="text-align:right;">
103
</td>
<td style="text-align:right;">
15285
</td>
<td style="text-align:right;">
96
</td>
</tr>
</tbody>
</table>

</div>

<div align="center">

<font size=2><b>Original Data</b> (scroll left/right to see full table)</font>

</div>

Then we have to probably do something else.

<details>
<summary>
Transform data
</summary>

``` r
# to get the actual ridership pre-pandemic:
#   current ridership = pre ridership * pre %
#   pre ridership = current / pre %

# when pre % = 0, it's impossible to estimate original ridership
# this happens ~50 times with the SI railway, but nowhere else
# just removing these datapoints for now

# sum(mta_data$SIRailway_pre == 0)

mta_data_long <- mta_data %>%
  pivot_longer(cols = c(ends_with('current'), ends_with('pre')),
               names_to = c('service', '.value'),
               names_pattern = '(.*?)_(.*)') %>%
  mutate(pre_estimate = ifelse(pre != 0, current / (pre / 100), NaN)) %>%
  pivot_longer(cols = c('current', 'pre_estimate'), names_to = 'timepoint', values_to = 'traffic') %>%
  rename(pre_percentage = pre) %>%
  mutate(year = year(date), .after = 'date') %>%
  mutate(month = month(date), .after = 'year') %>%
  mutate(day = day(date), .after = 'month') %>%
  group_by(service) %>%
  ungroup()

# pull out proportion of traffic pre-pandemic
service_pre <- mta_data_long %>%
  filter(timepoint == 'pre_estimate') %>%
  group_by(service) %>%
  summarise(service_traffic = sum(traffic, na.rm = T)) %>%
  ungroup() %>%
  mutate(total_traffic = sum(service_traffic, na.rm = T),
         service_prop = service_traffic / total_traffic)

# look at proportion of traffic by service by post-pandemic year
service_year <- mta_data_long %>%
  filter(timepoint == 'current') %>%
  group_by(year, service) %>%
  summarise(service_traffic = sum(traffic, na.rm = T)) %>%
  ungroup() %>%
  group_by(year) %>%
  mutate(total_traffic = sum(service_traffic, na.rm = T),
         service_prop = service_traffic / total_traffic)

# combine
serivce_year_wpre <- service_pre %>%
  mutate(year = 2026) %>% # setting pre-pandemic estimates as 2026 for plotting purposes
  rbind(service_year) %>%
  mutate(time_bin = ifelse(year == 'pre-pandemic', 'pre', 'curr'))

# set factor level order
mta_data_long$service <- factor(mta_data_long$service, levels = c('Subway', 'Bus', 'BridgesAndTunnels', 'LIRR',
                                                                  'MetroNorth', 'AccessARide', 'SIRailway'))
```

</details>

<div style="border: 1px solid #ddd; padding: 0px; border: none;overflow-y: scroll; height:auto; overflow-x: scroll; width:100%; ">

<table class=" no-border" style="font-size: 12px; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
year
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
month
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
day
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
service
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
pre_percentage
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
timepoint
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
traffic
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
97
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
2212965
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
96
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
5329915
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
5481103
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
5498809
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
99
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
5496453
</td>
</tr>
<tr>
<td style="text-align:left;width: 100px; ">
2020-03-06
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:left;">
Subway
</td>
<td style="text-align:right;">
93
</td>
<td style="text-align:left;">
current
</td>
<td style="text-align:right;">
5189447
</td>
</tr>
</tbody>
</table>

</div>

<div align="center">

<font size=2><b>Reformatted Data</b></font>

</div>

What was the share of traffic by service before the pandemic? Did it change?

<details>
<summary>
Get traffic estimates by year
</summary>

``` r
# look at proportion of traffic pre-pandemic
service_pre <- mta_data_long %>%
  filter(timepoint == 'pre_estimate') %>%
  group_by(service) %>%
  summarise(service_traffic = sum(traffic, na.rm = T)) %>%
  ungroup() %>%
  mutate(total_traffic = sum(service_traffic, na.rm = T),
         service_prop = service_traffic / total_traffic)

# look at proportion of traffic by service by post-pandemic year
service_year <- mta_data_long %>%
  filter(timepoint == 'current') %>%
  group_by(year, service) %>%
  summarise(service_traffic = sum(traffic, na.rm = T)) %>%
  ungroup() %>%
  group_by(year) %>%
  mutate(total_traffic = sum(service_traffic, na.rm = T),
         service_prop = service_traffic / total_traffic)

# combine
serivce_year_wpre <- service_pre %>%
  mutate(year = 2026) %>% # setting pre-pandemic estimates as 2026 for plotting purposes
  rbind(service_year) %>%
  mutate(time_bin = ifelse(year == 'pre-pandemic', 'pre', 'curr'))
```

</details>

Then plot the data!

``` r
g <- ggplot(data = serivce_year_wpre, aes(x = year, y = service_prop, fill = service, alpha = time_bin,
                                          tooltip = sprintf('%s: %.2f%%', service, service_prop * 100))) +
  geom_bar_interactive(stat = 'summary', width = 0.8) +
  theme(legend.position = 'right') +
  scale_fill_viridis_d(option = 'inferno', direction = -1, begin = 0.1, end = 0.9) +
  scale_alpha_discrete(range = c(1, 0.6), guide = 'none') +
  labs(x = 'year', y = 'proportion of total traffic',
       caption = 'hover over the plot to see percentages') +
  scale_x_continuous(breaks = c(2020, 2021, 2022, 2023, 2024, 2026),
                     labels = c('2020', '2021', '2022', '2023', '2024', 'pre-pandemic')) +
  plot_theme +
  theme(text = element_text(size = 14),
        plot.caption = element_text(hjust = 0.5, size = 11, face = 'italic'))

girafe(ggobj = g, options = list(opts_sizing(rescale = T, width = .7)))
```

<div class="girafe html-widget html-fill-item" id="htmlwidget-1" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"html":"<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' class='ggiraph-svg' role='graphics-document' id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18' viewBox='0 0 504 360'>\n <defs id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_defs'>\n  <clipPath id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_c1'>\n   <rect x='0' y='0' width='504' height='360'/>\n  <\/clipPath>\n  <clipPath id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_c2'>\n   <rect x='48.06' y='5.48' width='308.16' height='301.92'/>\n  <\/clipPath>\n <\/defs>\n <g id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_rootg' class='ggiraph-svg-rootg'>\n  <g clip-path='url(#svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_c1)'>\n   <rect x='0' y='0' width='504' height='360' fill='#FFFFFF' fill-opacity='1' stroke='#FFFFFF' stroke-opacity='1' stroke-width='0.75' stroke-linejoin='round' stroke-linecap='round' class='ggiraph-svg-bg'/>\n   <rect x='0' y='0' width='504' height='360' fill='#FFFFFF' fill-opacity='1' stroke='#FFFFFF' stroke-opacity='1' stroke-width='1.07' stroke-linejoin='round' stroke-linecap='round'/>\n  <\/g>\n  <g clip-path='url(#svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_c2)'>\n   <rect x='48.06' y='5.48' width='308.16' height='301.92' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e1' x='62.07' y='19.2' width='32.96' height='134' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 48.82%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e2' x='103.27' y='19.2' width='32.96' height='137.24' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 50.00%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e3' x='144.47' y='19.2' width='32.96' height='148.53' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 54.11%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e4' x='185.67' y='19.2' width='32.96' height='154.31' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 56.22%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e5' x='226.86' y='19.2' width='32.96' height='156.17' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 56.90%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e6' x='309.26' y='19.2' width='32.96' height='160.91' fill='#F6D645' fill-opacity='1' stroke='none' title='Subway: 58.62%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e7' x='62.07' y='153.21' width='32.96' height='53.37' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 19.44%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e8' x='226.86' y='175.37' width='32.96' height='53.63' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 19.54%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e9' x='185.67' y='173.51' width='32.96' height='57.09' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 20.80%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e10' x='144.47' y='167.73' width='32.96' height='62.19' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 22.66%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e11' x='309.26' y='180.11' width='32.96' height='63.64' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 23.19%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e12' x='103.27' y='156.45' width='32.96' height='68.93' fill='#FB9507' fill-opacity='1' stroke='none' title='Bus: 25.12%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e13' x='309.26' y='243.75' width='32.96' height='31.84' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 11.60%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e14' x='226.86' y='229' width='32.96' height='44.61' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 16.25%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e15' x='185.67' y='230.6' width='32.96' height='45.08' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 16.42%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e16' x='144.47' y='229.92' width='32.96' height='47.99' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 17.49%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e17' x='103.27' y='225.38' width='32.96' height='55.67' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 20.28%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e18' x='62.07' y='206.57' width='32.96' height='74.62' fill='#E65D30' fill-opacity='1' stroke='none' title='BridgesAndTunnels: 27.19%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e19' x='103.27' y='281.05' width='32.96' height='6.37' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 2.32%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e20' x='62.07' y='281.2' width='32.96' height='6.45' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 2.35%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e21' x='144.47' y='277.92' width='32.96' height='7.63' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 2.78%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e22' x='185.67' y='275.68' width='32.96' height='8.7' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 3.17%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e23' x='309.26' y='275.59' width='32.96' height='8.69' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 3.17%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e24' x='226.86' y='273.6' width='32.96' height='9.8' fill='#BB3754' fill-opacity='1' stroke='none' title='LIRR: 3.57%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e25' x='62.07' y='287.65' width='32.96' height='4.16' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 1.52%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e26' x='103.27' y='287.42' width='32.96' height='4.78' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 1.74%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e27' x='144.47' y='285.54' width='32.96' height='6.75' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 2.46%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e28' x='185.67' y='284.38' width='32.96' height='7.79' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 2.84%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e29' x='309.26' y='284.28' width='32.96' height='8.09' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 2.95%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e30' x='226.86' y='283.4' width='32.96' height='8.53' fill='#86216B' fill-opacity='1' stroke='none' title='MetroNorth: 3.11%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e31' x='309.26' y='292.37' width='32.96' height='0.88' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.32%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e32' x='144.47' y='292.3' width='32.96' height='1.12' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.41%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e33' x='103.27' y='292.2' width='32.96' height='1.23' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.45%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e34' x='185.67' y='292.17' width='32.96' height='1.24' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.45%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e35' x='226.86' y='291.94' width='32.96' height='1.47' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.53%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e36' x='62.07' y='291.81' width='32.96' height='1.61' fill='#500E6C' fill-opacity='1' stroke='none' title='AccessARide: 0.59%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e37' x='103.27' y='293.44' width='32.96' height='0.24' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.09%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e38' x='62.07' y='293.42' width='32.96' height='0.26' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.10%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e39' x='144.47' y='293.42' width='32.96' height='0.26' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.10%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e40' x='185.67' y='293.41' width='32.96' height='0.27' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.10%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e41' x='226.86' y='293.4' width='32.96' height='0.28' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.10%'/>\n   <rect id='svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_e42' x='309.26' y='293.26' width='32.96' height='0.42' fill='#170C3A' fill-opacity='1' stroke='none' title='SIRailway: 0.15%'/>\n   <rect x='48.06' y='5.48' width='308.16' height='301.92' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='1.07' stroke-linejoin='round' stroke-linecap='round'/>\n  <\/g>\n  <g clip-path='url(#svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18_c1)'>\n   <text x='21.36' y='297.69' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>0.00<\/text>\n   <text x='21.36' y='229.07' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>0.25<\/text>\n   <text x='21.36' y='160.45' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>0.50<\/text>\n   <text x='21.36' y='91.84' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>0.75<\/text>\n   <text x='21.36' y='23.22' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>1.00<\/text>\n   <polyline points='45.32,293.68 48.06,293.68' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='45.32,225.06 48.06,225.06' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='45.32,156.44 48.06,156.44' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='45.32,87.82 48.06,87.82' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='45.32,19.20 48.06,19.20' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='78.55,310.14 78.55,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='119.75,310.14 119.75,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='160.95,310.14 160.95,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='202.14,310.14 202.14,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='243.34,310.14 243.34,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <polyline points='325.74,310.14 325.74,307.40' fill='none' stroke='#B3B3B3' stroke-opacity='1' stroke-width='0.53' stroke-linejoin='round' stroke-linecap='butt'/>\n   <text x='66.11' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>2020<\/text>\n   <text x='107.31' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>2021<\/text>\n   <text x='148.5' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>2022<\/text>\n   <text x='189.7' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>2023<\/text>\n   <text x='230.9' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>2024<\/text>\n   <text x='291.55' y='320.36' font-size='8.4pt' font-family='Helvetica' fill='#4D4D4D' fill-opacity='1'>pre-pandemic<\/text>\n   <text x='188.53' y='335.62' font-size='10.5pt' font-family='Helvetica'>year<\/text>\n   <text transform='translate(15.52,229.96) rotate(-90.00)' font-size='10.5pt' font-family='Helvetica'>proportion of total traffic<\/text>\n   <rect x='367.18' y='81.17' width='131.34' height='150.54' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <text x='372.66' y='98.24' font-size='10.5pt' font-family='Helvetica'>service<\/text>\n   <rect x='372.66' y='105.27' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='105.98' width='15.86' height='15.86' fill='#F6D645' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='122.55' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='123.26' width='15.86' height='15.86' fill='#FB9507' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='139.83' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='140.54' width='15.86' height='15.86' fill='#E65D30' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='157.11' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='157.82' width='15.86' height='15.86' fill='#BB3754' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='174.39' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='175.1' width='15.86' height='15.86' fill='#86216B' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='191.67' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='192.38' width='15.86' height='15.86' fill='#500E6C' fill-opacity='1' stroke='none'/>\n   <rect x='372.66' y='208.95' width='17.28' height='17.28' fill='#FFFFFF' fill-opacity='1' stroke='none'/>\n   <rect x='373.37' y='209.66' width='15.86' height='15.86' fill='#170C3A' fill-opacity='1' stroke='none'/>\n   <text x='395.42' y='117.92' font-size='8.4pt' font-family='Helvetica'>Subway<\/text>\n   <text x='395.42' y='135.2' font-size='8.4pt' font-family='Helvetica'>Bus<\/text>\n   <text x='395.42' y='152.48' font-size='8.4pt' font-family='Helvetica'>BridgesAndTunnels<\/text>\n   <text x='395.42' y='169.76' font-size='8.4pt' font-family='Helvetica'>LIRR<\/text>\n   <text x='395.42' y='187.04' font-size='8.4pt' font-family='Helvetica'>MetroNorth<\/text>\n   <text x='395.42' y='204.32' font-size='8.4pt' font-family='Helvetica'>AccessARide<\/text>\n   <text x='395.42' y='221.6' font-size='8.4pt' font-family='Helvetica'>SIRailway<\/text>\n   <text x='108.27' y='352.09' font-size='8.25pt' font-style='italic' font-family='Helvetica'>hover over the plot to see percentages<\/text>\n  <\/g>\n <\/g>\n<\/svg>","js":null,"uid":"svg_dd4c0c96_4add_40eb_be27_d33a4b5cde18","ratio":1.4,"settings":{"tooltip":{"css":".tooltip_SVGID_ { padding:5px;background:black;color:white;border-radius:2px;text-align:left; ; position:absolute;pointer-events:none;z-index:999;}","placement":"doc","opacity":0.9,"offx":10,"offy":10,"use_cursor_pos":true,"use_fill":false,"use_stroke":false,"delay_over":200,"delay_out":500},"hover":{"css":".hover_data_SVGID_ { fill:orange;stroke:black;cursor:pointer; }\ntext.hover_data_SVGID_ { stroke:none;fill:orange; }\ncircle.hover_data_SVGID_ { fill:orange;stroke:black; }\nline.hover_data_SVGID_, polyline.hover_data_SVGID_ { fill:none;stroke:orange; }\nrect.hover_data_SVGID_, polygon.hover_data_SVGID_, path.hover_data_SVGID_ { fill:orange;stroke:none; }\nimage.hover_data_SVGID_ { stroke:orange; }","reactive":true,"nearest_distance":null},"hover_inv":{"css":""},"hover_key":{"css":".hover_key_SVGID_ { fill:orange;stroke:black;cursor:pointer; }\ntext.hover_key_SVGID_ { stroke:none;fill:orange; }\ncircle.hover_key_SVGID_ { fill:orange;stroke:black; }\nline.hover_key_SVGID_, polyline.hover_key_SVGID_ { fill:none;stroke:orange; }\nrect.hover_key_SVGID_, polygon.hover_key_SVGID_, path.hover_key_SVGID_ { fill:orange;stroke:none; }\nimage.hover_key_SVGID_ { stroke:orange; }","reactive":true},"hover_theme":{"css":".hover_theme_SVGID_ { fill:orange;stroke:black;cursor:pointer; }\ntext.hover_theme_SVGID_ { stroke:none;fill:orange; }\ncircle.hover_theme_SVGID_ { fill:orange;stroke:black; }\nline.hover_theme_SVGID_, polyline.hover_theme_SVGID_ { fill:none;stroke:orange; }\nrect.hover_theme_SVGID_, polygon.hover_theme_SVGID_, path.hover_theme_SVGID_ { fill:orange;stroke:none; }\nimage.hover_theme_SVGID_ { stroke:orange; }","reactive":true},"select":{"css":".select_data_SVGID_ { fill:red;stroke:black;cursor:pointer; }\ntext.select_data_SVGID_ { stroke:none;fill:red; }\ncircle.select_data_SVGID_ { fill:red;stroke:black; }\nline.select_data_SVGID_, polyline.select_data_SVGID_ { fill:none;stroke:red; }\nrect.select_data_SVGID_, polygon.select_data_SVGID_, path.select_data_SVGID_ { fill:red;stroke:none; }\nimage.select_data_SVGID_ { stroke:red; }","type":"multiple","only_shiny":true,"selected":[]},"select_inv":{"css":""},"select_key":{"css":".select_key_SVGID_ { fill:red;stroke:black;cursor:pointer; }\ntext.select_key_SVGID_ { stroke:none;fill:red; }\ncircle.select_key_SVGID_ { fill:red;stroke:black; }\nline.select_key_SVGID_, polyline.select_key_SVGID_ { fill:none;stroke:red; }\nrect.select_key_SVGID_, polygon.select_key_SVGID_, path.select_key_SVGID_ { fill:red;stroke:none; }\nimage.select_key_SVGID_ { stroke:red; }","type":"single","only_shiny":true,"selected":[]},"select_theme":{"css":".select_theme_SVGID_ { fill:red;stroke:black;cursor:pointer; }\ntext.select_theme_SVGID_ { stroke:none;fill:red; }\ncircle.select_theme_SVGID_ { fill:red;stroke:black; }\nline.select_theme_SVGID_, polyline.select_theme_SVGID_ { fill:none;stroke:red; }\nrect.select_theme_SVGID_, polygon.select_theme_SVGID_, path.select_theme_SVGID_ { fill:red;stroke:none; }\nimage.select_theme_SVGID_ { stroke:red; }","type":"single","only_shiny":true,"selected":[]},"zoom":{"min":1,"max":1,"duration":300},"toolbar":{"position":"topright","pngname":"diagram","tooltips":null,"fixed":false,"hidden":[],"delay_over":200,"delay_out":500},"sizing":{"rescale":true,"width":0.7}}},"evals":[],"jsHooks":[]}</script>

``` r
ggplot(mta_data_long, aes(x = date, y = pre_percentage, color = service, fill = service)) +
  geom_line(color = 'grey80', linewidth = 0.2) +
  geom_smooth(method = 'lm', linewidth = 2, alpha = 1) +
  geom_hline(yintercept = 100, linetype = 'dashed', color = 'tomato2') +
  facet_wrap(~service, nrow = 2) +
  scale_color_viridis_d(option = 'inferno', direction = -1, begin = 0.1, end = 0.9) +
  scale_fill_viridis_d(option = 'inferno', direction = -1, begin = 0.1, end = 0.9) +
  labs(x = 'date', y = 'percentage of pre-pandemic traffic') +
  plot_theme +
  theme(text = element_text(size = 14), legend.position = 'top') +
  guides(color = guide_legend(nrow = 1))
```

<img src="/project_mta_files/figure-html/unnamed-chunk-7-1.png" width="960" style="display: block; margin: auto;" />
