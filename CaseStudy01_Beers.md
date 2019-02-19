---
title: "Evaluation of Craft Breweries in the <br> United States of America <br>"
author: "<br>Heindel Adu, Amy Markmum and Gabriel Gonzalez <br>- AGH Data Consult Inc."
date: "Spring 2019"
output: 
  html_document: 
    highlight: pygments
    keep_md: yes
    theme: united
---
#
#<br>
#

#### Load the necessary library packages prior to loading the data. I used the library(tidyr) and library(dplyr) . This will not show the code in the markdown file output in html.


# <br>
##### 1. First read the beers data file Beers.csv and inspect the dataframe.


```r
library(readr)
Beers <- read_csv("Beers.csv")
```

```
## Parsed with column specification:
## cols(
##   Name = col_character(),
##   Beer_ID = col_double(),
##   ABV = col_double(),
##   IBU = col_double(),
##   Brewery_id = col_double(),
##   Style = col_character(),
##   Ounces = col_double()
## )
```

```r
head(Beers)
```

```
## # A tibble: 6 x 7
##   Name           Beer_ID   ABV   IBU Brewery_id Style                Ounces
##   <chr>            <dbl> <dbl> <dbl>      <dbl> <chr>                 <dbl>
## 1 Pub Beer          1436 0.05     NA        409 American Pale Lager      12
## 2 Devil's Cup       2265 0.066    NA        178 American Pale Ale (…     12
## 3 Rise of the P…    2264 0.071    NA        178 American IPA             12
## 4 Sinister          2263 0.09     NA        178 American Double / I…     12
## 5 Sex and Candy     2262 0.075    NA        178 American IPA             12
## 6 Black Exodus      2261 0.077    NA        178 Oatmeal Stout            12
```


```r
Beers
```

```
## # A tibble: 2,410 x 7
##    Name           Beer_ID   ABV   IBU Brewery_id Style               Ounces
##    <chr>            <dbl> <dbl> <dbl>      <dbl> <chr>                <dbl>
##  1 Pub Beer          1436 0.05     NA        409 American Pale Lager     12
##  2 Devil's Cup       2265 0.066    NA        178 American Pale Ale …     12
##  3 Rise of the P…    2264 0.071    NA        178 American IPA            12
##  4 Sinister          2263 0.09     NA        178 American Double / …     12
##  5 Sex and Candy     2262 0.075    NA        178 American IPA            12
##  6 Black Exodus      2261 0.077    NA        178 Oatmeal Stout           12
##  7 Lake Street E…    2260 0.045    NA        178 American Pale Ale …     12
##  8 Foreman           2259 0.065    NA        178 American Porter         12
##  9 Jade              2258 0.055    NA        178 American Pale Ale …     12
## 10 Cone Crusher      2131 0.086    NA        178 American Double / …     12
## # … with 2,400 more rows
```

```r
library(readr)
Breweries <- read_csv("Breweries.csv")
```

```
## Parsed with column specification:
## cols(
##   Brew_ID = col_double(),
##   Name = col_character(),
##   City = col_character(),
##   State = col_character()
## )
```

```r
head(Breweries)
```

```
## # A tibble: 6 x 4
##   Brew_ID Name                      City          State
##     <dbl> <chr>                     <chr>         <chr>
## 1       1 NorthGate Brewing         Minneapolis   MN   
## 2       2 Against the Grain Brewery Louisville    KY   
## 3       3 Jack's Abby Craft Lagers  Framingham    MA   
## 4       4 Mike Hess Brewing Company San Diego     CA   
## 5       5 Fort Point Beer Company   San Francisco CA   
## 6       6 COAST Brewing Company     Charleston    SC
```


```r
summary(Beers)
```

```
##      Name              Beer_ID            ABV               IBU        
##  Length:2410        Min.   :   1.0   Min.   :0.00100   Min.   :  4.00  
##  Class :character   1st Qu.: 808.2   1st Qu.:0.05000   1st Qu.: 21.00  
##  Mode  :character   Median :1453.5   Median :0.05600   Median : 35.00  
##                     Mean   :1431.1   Mean   :0.05977   Mean   : 42.71  
##                     3rd Qu.:2075.8   3rd Qu.:0.06700   3rd Qu.: 64.00  
##                     Max.   :2692.0   Max.   :0.12800   Max.   :138.00  
##                                      NA's   :62        NA's   :1005    
##    Brewery_id       Style               Ounces     
##  Min.   :  1.0   Length:2410        Min.   : 8.40  
##  1st Qu.: 94.0   Class :character   1st Qu.:12.00  
##  Median :206.0   Mode  :character   Median :12.00  
##  Mean   :232.7                      Mean   :13.59  
##  3rd Qu.:367.0                      3rd Qu.:16.00  
##  Max.   :558.0                      Max.   :32.00  
## 
```

```r
summary(Breweries)
```

```
##     Brew_ID          Name               City              State          
##  Min.   :  1.0   Length:558         Length:558         Length:558        
##  1st Qu.:140.2   Class :character   Class :character   Class :character  
##  Median :279.5   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :279.5                                                           
##  3rd Qu.:418.8                                                           
##  Max.   :558.0
```


```r
names(Beers)
```

```
## [1] "Name"       "Beer_ID"    "ABV"        "IBU"        "Brewery_id"
## [6] "Style"      "Ounces"
```

```r
names(Breweries)
```

```
## [1] "Brew_ID" "Name"    "City"    "State"
```

```r
Beers <- rename(Beers, Brew_ID = Brewery_id)
names(Beers)
```

```
## [1] "Name"    "Beer_ID" "ABV"     "IBU"     "Brew_ID" "Style"   "Ounces"
```


```r
fullData <- full_join(Breweries, Beers, by = "Brew_ID")
```


```r
fullData
```

```
## # A tibble: 2,410 x 10
##    Brew_ID Name.x   City   State Name.y  Beer_ID   ABV   IBU Style   Ounces
##      <dbl> <chr>    <chr>  <chr> <chr>     <dbl> <dbl> <dbl> <chr>    <dbl>
##  1       1 NorthGa… Minne… MN    Get To…    2692 0.045    50 Americ…     16
##  2       1 NorthGa… Minne… MN    Maggie…    2691 0.049    26 Milk /…     16
##  3       1 NorthGa… Minne… MN    Wall's…    2690 0.048    19 Englis…     16
##  4       1 NorthGa… Minne… MN    Pumpion    2689 0.06     38 Pumpki…     16
##  5       1 NorthGa… Minne… MN    Strong…    2688 0.06     25 Americ…     16
##  6       1 NorthGa… Minne… MN    Parape…    2687 0.056    47 Extra …     16
##  7       2 Against… Louis… KY    Citra …    2686 0.08     68 Americ…     16
##  8       2 Against… Louis… KY    London…    2685 0.125    80 Englis…     16
##  9       2 Against… Louis… KY    35 K       2684 0.077    25 Milk /…     16
## 10       2 Against… Louis… KY    A Beer     2683 0.042    42 Americ…     16
## # … with 2,400 more rows
```

```r
names(fullData)
```

```
##  [1] "Brew_ID" "Name.x"  "City"    "State"   "Name.y"  "Beer_ID" "ABV"    
##  [8] "IBU"     "Style"   "Ounces"
```

```r
fullData <- rename(fullData, BreweryName = Name.x, BeerName = Name.y)
names(fullData)
```

```
##  [1] "Brew_ID"     "BreweryName" "City"        "State"       "BeerName"   
##  [6] "Beer_ID"     "ABV"         "IBU"         "Style"       "Ounces"
```

```r
fullData %>%
  group_by(State) %>%
  summarise(count = n())
```

```
## # A tibble: 51 x 2
##    State count
##    <chr> <int>
##  1 AK       25
##  2 AL       10
##  3 AR        5
##  4 AZ       47
##  5 CA      183
##  6 CO      265
##  7 CT       27
##  8 DC        8
##  9 DE        2
## 10 FL       58
## # … with 41 more rows
```

```r
BrewStatesC <- Breweries%>%
  group_by(State)%>%
  summarise(count = n())
```

```r
head(BrewStatesC)
```

```
## # A tibble: 6 x 2
##   State count
##   <chr> <int>
## 1 AK        7
## 2 AL        3
## 3 AR        2
## 4 AZ       11
## 5 CA       39
## 6 CO       47
```

```r
BrewStatesC
```

```
## # A tibble: 51 x 2
##    State count
##    <chr> <int>
##  1 AK        7
##  2 AL        3
##  3 AR        2
##  4 AZ       11
##  5 CA       39
##  6 CO       47
##  7 CT        8
##  8 DC        1
##  9 DE        2
## 10 FL       15
## # … with 41 more rows
```

```r
state.freq <- table(Breweries$State)
barplot(state.freq[order(state.freq)], 
        horiz = T,
        border = NA,
        xlim = c(0, 100),
        main = "Number of Breweries per State",
        xlab = "Number of Brewiews",
        ylab = "States")
```

![](CaseStudy01_Beers_files/figure-html/unnamed-chunk-17-1.png)<!-- -->
#<br> 
# Number brewweries in each state:


```r
state.freq
```

```
## 
## AK AL AR AZ CA CO CT DC DE FL GA HI IA ID IL IN KS KY LA MA MD ME MI MN MO 
##  7  3  2 11 39 47  8  1  2 15  7  4  5  5 18 22  3  4  5 23  7  9 32 12  9 
## MS MT NC ND NE NH NJ NM NV NY OH OK OR PA RI SC SD TN TX UT VA VT WA WI WV 
##  2  9 19  1  5  3  3  4  2 16 15  6 29 25  5  4  1  3 28  4 16 10 23 20  1 
## WY 
##  4
```


```r
names(state.freq)
```

```
##  [1] "AK" "AL" "AR" "AZ" "CA" "CO" "CT" "DC" "DE" "FL" "GA" "HI" "IA" "ID"
## [15] "IL" "IN" "KS" "KY" "LA" "MA" "MD" "ME" "MI" "MN" "MO" "MS" "MT" "NC"
## [29] "ND" "NE" "NH" "NJ" "NM" "NV" "NY" "OH" "OK" "OR" "PA" "RI" "SC" "SD"
## [43] "TN" "TX" "UT" "VA" "VT" "WA" "WI" "WV" "WY"
```

```r
View(state.freq)
```

```
## Warning in system2("/usr/bin/otool", c("-L", shQuote(DSO)), stdout = TRUE):
## running command ''/usr/bin/otool' -L '/Library/Frameworks/R.framework/
## Resources/modules/R_de.so'' had status 1
```


```r
sDF<-data.frame(state.freq)
names(sDF)
```

```
## [1] "Var1" "Freq"
```

```r
sDF<- rename(sDF, State = Var1, NoBreweries=Freq)
```


```r
View(sDF)
```

```
## Warning in system2("/usr/bin/otool", c("-L", shQuote(DSO)), stdout = TRUE):
## running command ''/usr/bin/otool' -L '/Library/Frameworks/R.framework/
## Resources/modules/R_de.so'' had status 1
```

```r
names(Breweries)
```

```
## [1] "Brew_ID" "Name"    "City"    "State"
```
#<br>

```r
p1 <- plot_ly(Breweries, x = ~State) %>% add_histogram()
p2 <-Breweries %>%
  dplyr::count(State) %>%
  plot_ly(x = ~n, y = ~State) %>%
  add_bars(marker = list(color = 'rgba(170, 17, 19, 0.9)')) %>%
  layout(title = "Number of Breweries by State", 
         legend = list(x = 0.03, y = 1.04)) 
 

subplot(p2) 
```

<!--html_preserve--><div id="htmlwidget-85ba8a3a5999d67ffa21" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-85ba8a3a5999d67ffa21">{"x":{"data":[{"x":[7,3,2,11,39,47,8,1,2,15,7,4,5,5,18,22,3,4,5,23,7,9,32,12,9,2,9,19,1,5,3,3,4,2,16,15,6,29,25,5,4,1,3,28,4,16,10,23,20,1,4],"y":["AK","AL","AR","AZ","CA","CO","CT","DC","DE","FL","GA","HI","IA","ID","IL","IN","KS","KY","LA","MA","MD","ME","MI","MN","MO","MS","MT","NC","ND","NE","NH","NJ","NM","NV","NY","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VA","VT","WA","WI","WV","WY"],"type":"bar","marker":{"color":"rgba(170, 17, 19, 0.9)","line":{"color":"rgba(31,119,180,1)"}},"orientation":"h","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"layout":{"xaxis":{"domain":[0,1],"automargin":true,"anchor":"y"},"yaxis":{"domain":[0,1],"automargin":true,"type":"category","categoryorder":"array","categoryarray":["AK","AL","AR","AZ","CA","CO","CT","DC","DE","FL","GA","HI","IA","ID","IL","IN","KS","KY","LA","MA","MD","ME","MI","MN","MO","MS","MT","NC","ND","NE","NH","NJ","NM","NV","NY","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VA","VT","WA","WI","WV","WY"],"anchor":"x"},"margin":{"b":40,"l":60,"t":25,"r":10},"title":"Number of Breweries by State","legend":{"x":0.03,"y":1.04},"hovermode":"closest","showlegend":false},"attrs":{"b3ba6ee739e9":{"x":{},"y":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar","marker":{"color":"rgba(170, 17, 19, 0.9)"},"inherit":true}},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"subplot":true,"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->
#<br>
