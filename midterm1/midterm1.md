---
title: "Midterm 1 W24"
author: "Ketong Zhang"
date: "2024-02-06"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt!  

Be sure to push your completed midterm to your repository. This exam is worth 30 points.  

## Background
In the data folder, you will find data related to a study on wolf mortality collected by the National Park Service. You should start by reading the `README_NPSwolfdata.pdf` file. This will provide an abstract of the study and an explanation of variables.  

The data are from: Cassidy, Kira et al. (2022). Gray wolf packs and human-caused wolf mortality. [Dryad](https://doi.org/10.5061/dryad.mkkwh713f). 

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the wolves data
In these data, the authors used `NULL` to represent missing values. I am correcting this for you below and using `janitor` to clean the column names.

```r
wolves <- read.csv("data/NPS_wolfmortalitydata.csv", na = c("NULL")) %>% clean_names()
```

## Questions
Problem 1. (1 point) Let's start with some data exploration. What are the variable (column) names?  

**The column names are: park,biolyr,pack,packcode,packsize_aug,mort_yn,mort_all,mort_lead,mort_nonlead,reprody1,persisty1**  

```r
colnames(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```

Problem 2. (1 point) Use the function of your choice to summarize the data and get an idea of its structure.  

**I chose the summary() and the str() functions**

```r
summary(wolves)
```

```
##      park               biolyr         pack              packcode     
##  Length:864         Min.   :1986   Length:864         Min.   :  2.00  
##  Class :character   1st Qu.:1999   Class :character   1st Qu.: 48.00  
##  Mode  :character   Median :2006   Mode  :character   Median : 86.50  
##                     Mean   :2005                      Mean   : 91.39  
##                     3rd Qu.:2012                      3rd Qu.:133.00  
##                     Max.   :2021                      Max.   :193.00  
##                                                                       
##   packsize_aug       mort_yn          mort_all         mort_lead      
##  Min.   : 0.000   Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000  
##  1st Qu.: 5.000   1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000  
##  Median : 8.000   Median :0.0000   Median : 0.0000   Median :0.00000  
##  Mean   : 8.789   Mean   :0.1956   Mean   : 0.3715   Mean   :0.09552  
##  3rd Qu.:12.000   3rd Qu.:0.0000   3rd Qu.: 0.0000   3rd Qu.:0.00000  
##  Max.   :37.000   Max.   :1.0000   Max.   :24.0000   Max.   :3.00000  
##  NA's   :55                                          NA's   :16       
##   mort_nonlead        reprody1        persisty1     
##  Min.   : 0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 0.0000   1st Qu.:1.0000   1st Qu.:1.0000  
##  Median : 0.0000   Median :1.0000   Median :1.0000  
##  Mean   : 0.2641   Mean   :0.7629   Mean   :0.8865  
##  3rd Qu.: 0.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :22.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :12        NA's   :71       NA's   :9
```


```r
str(wolves)
```

```
## 'data.frame':	864 obs. of  11 variables:
##  $ park        : chr  "DENA" "DENA" "DENA" "DENA" ...
##  $ biolyr      : int  1996 1991 2017 1996 1992 1994 2007 2007 1995 2003 ...
##  $ pack        : chr  "McKinley River1" "Birch Creek N" "Eagle Gorge" "East Fork" ...
##  $ packcode    : int  89 58 71 72 74 77 101 108 109 53 ...
##  $ packsize_aug: num  12 5 8 13 7 6 10 NA 9 8 ...
##  $ mort_yn     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ mort_all    : int  4 2 2 2 2 2 2 2 2 1 ...
##  $ mort_lead   : int  2 2 0 0 0 0 1 2 1 1 ...
##  $ mort_nonlead: int  2 0 2 2 2 2 1 0 1 0 ...
##  $ reprody1    : int  0 0 NA 1 NA 0 0 1 0 1 ...
##  $ persisty1   : int  0 0 1 1 1 1 0 1 0 1 ...
```


Problem 3. (3 points) Which parks/ reserves are represented in the data? Don't just use the abstract, pull this information from the data.  

**DENA, GNTP, VNP, YNP, YUCH are the parks that were represented in the data.**

```r
tabyl(wolves$park)
```

```
##  wolves$park   n    percent
##         DENA 340 0.39351852
##         GNTP  77 0.08912037
##          VNP  48 0.05555556
##          YNP 248 0.28703704
##         YUCH 151 0.17476852
```
**Alternative way**

```r
wolves %>% 
  group_by(park) %>% 
  summarize(n=n())
```

```
## # A tibble: 5 × 2
##   park      n
##   <chr> <int>
## 1 DENA    340
## 2 GNTP     77
## 3 VNP      48
## 4 YNP     248
## 5 YUCH    151
```

Problem 4. (4 points) Which park has the largest number of wolf packs?

**First determine whether there is NA in the pack data or not**

```r
anyNA(wolves$packsize_aug)
```

```
## [1] TRUE
```

**YNP has the largest number of wolf packs**

```r
wolves %>% 
  group_by(park) %>% 
  summarize(max_pack = max(packsize_aug, na.rm = T)) %>% #there are NA's so need to use na.rm = T to remove them
  arrange(desc(max_pack))
```

```
## # A tibble: 5 × 2
##   park  max_pack
##   <chr>    <dbl>
## 1 YNP       37  
## 2 DENA      33  
## 3 GNTP      26.4
## 4 YUCH      24  
## 5 VNP        7
```


Problem 5. (4 points) Which park has the highest total number of human-caused mortalities `mort_all`?
**YUCH has the highest total number of human-caused mortalities**

```r
wolves %>% 
  group_by(park) %>% 
  summarize(high_human = max(mort_all, na.rm = T)) %>% 
  arrange(desc(high_human))
```

```
## # A tibble: 5 × 2
##   park  high_human
##   <chr>      <int>
## 1 YUCH          24
## 2 DENA           4
## 3 GNTP           4
## 4 YNP            4
## 5 VNP            2
```


The wolves in [Yellowstone National Park](https://www.nps.gov/yell/learn/nature/wolf-restoration.htm) are an incredible conservation success story. Let's focus our attention on this park.  

Problem 6. (2 points) Create a new object "ynp" that only includes the data from Yellowstone National Park.  

```r
ynp <- wolves %>% 
  filter(park == "YNP")
ynp
```

```
##     park biolyr            pack packcode packsize_aug mort_yn mort_all
## 1    YNP   2009      cottonwood       23           12       1        4
## 2    YNP   2016           8mile       11           20       1        3
## 3    YNP   2017          canyon       20            2       1        3
## 4    YNP   2012        junction       33           11       1        3
## 5    YNP   2016        junction       33           15       1        3
## 6    YNP   2011       642Fgroup        5           10       1        2
## 7    YNP   2012           8mile       11           15       1        2
## 8    YNP   2012         bechler       16           12       1        2
## 9    YNP   2012           lamar       34           13       1        2
## 10   YNP   2019         phantom       41           18       1        2
## 11   YNP   2020         phantom       41           16       1        2
## 12   YNP   2017        prospect       42           10       1        2
## 13   YNP   2020          wapiti       51           23       1        2
## 14   YNP   2018      1118Fgroup        2            5       1        1
## 15   YNP   2020      1155Mgroup        3            3       1        1
## 16   YNP   2014           8mile       11           27       1        1
## 17   YNP   2017       963Fgroup       13            4       1        1
## 18   YNP   2012       blacktail       18           10       1        1
## 19   YNP   2014          cougar       24           18       1        1
## 20   YNP   2017          cougar       24            7       1        1
## 21   YNP   2014        junction       33           13       1        1
## 22   YNP   2018           lamar       34            8       1        1
## 23   YNP   2012         mollies       38            6       1        1
## 24   YNP   2019         mollies       38           12       1        1
## 25   YNP   2018         phantom       41            5       1        1
## 26   YNP   2014        prospect       42            7       1        1
## 27   YNP   2012           snake       46            4       1        1
## 28   YNP   2017           snake       46           13       1        1
## 29   YNP   2007       yelldelta       52           22       1        3
## 30   YNP   1997           druid       26            5       1        2
## 31   YNP   2019        junction       33           21       1        2
## 32   YNP   2003           agate       15           13       1        1
## 33   YNP   2009       blacktail       18           13       1        1
## 34   YNP   2011       blacktail       18           16       1        1
## 35   YNP   2013          canyon       20           10       1        1
## 36   YNP   2015          cougar       24            7       1        1
## 37   YNP   2000           druid       26           27       1        1
## 38   YNP   2004     gibbon/mary       29            9       1        1
## 39   YNP   2009     gibbon/mary       29           26       1        1
## 40   YNP   2010        grayling       30            3       1        1
## 41   YNP   2007         mollies       38           15       1        1
## 42   YNP   2011         mollies       38           23       1        1
## 43   YNP   2013         mollies       38            8       1        1
## 44   YNP   1996        nezperce       39            9       1        1
## 45   YNP   1997        nezperce       39            7       1        1
## 46   YNP   2000        nezperce       39           22       1        1
## 47   YNP   1995            rose       44            9       1        1
## 48   YNP   1998            rose       44           23       1        1
## 49   YNP   2007          slough       45           22       1        1
## 50   YNP   2009       682Mgroup        6           NA       0        0
## 51   YNP   2010       682Mgroup        6            0       0        0
## 52   YNP   2009       694Fgroup        8            0       0        0
## 53   YNP   2013       755Mgroup        9            2       0        0
## 54   YNP   2011           8mile       11           17       0        0
## 55   YNP   2013           8mile       11           19       0        0
## 56   YNP   2015           8mile       11           13       0        0
## 57   YNP   2017           8mile       11           18       0        0
## 58   YNP   2018           8mile       11           13       0        0
## 59   YNP   2020           8mile       11           22       0        0
## 60   YNP   2002           agate       15           10       0        0
## 61   YNP   2004           agate       15           13       0        0
## 62   YNP   2005           agate       15           14       0        0
## 63   YNP   2006           agate       15           13       0        0
## 64   YNP   2007           agate       15           20       0        0
## 65   YNP   2008           agate       15           14       0        0
## 66   YNP   2009           agate       15            4       0        0
## 67   YNP   2010           agate       15            9       0        0
## 68   YNP   2011           agate       15           13       0        0
## 69   YNP   2012           agate       15            0       0        0
## 70   YNP   2002         bechler       16            4       0        0
## 71   YNP   2004         biscuit       17           11       0        0
## 72   YNP   2008       blacktail       18           NA       0        0
## 73   YNP   2010       blacktail       18           17       0        0
## 74   YNP   2002     buffalofork       19            4       0        0
## 75   YNP   2003     buffalofork       19            3       0        0
## 76   YNP   2008          canyon       20            6       0        0
## 77   YNP   2009          canyon       20            4       0        0
## 78   YNP   2010          canyon       20            6       0        0
## 79   YNP   2011          canyon       20            8       0        0
## 80   YNP   2012          canyon       20            9       0        0
## 81   YNP   2014          canyon       20            5       0        0
## 82   YNP   2015          canyon       20            6       0        0
## 83   YNP   2016          canyon       20            6       0        0
## 84   YNP   2021       carnelian       21            0       0        0
## 85   YNP   2016        cinnabar       22            3       0        0
## 86   YNP   2008      cottonwood       23            4       0        0
## 87   YNP   2001          cougar       24            6       0        0
## 88   YNP   2002          cougar       24           11       0        0
## 89   YNP   2003          cougar       24           14       0        0
## 90   YNP   2004          cougar       24           15       0        0
## 91   YNP   2005          cougar       24           14       0        0
## 92   YNP   2006          cougar       24            4       0        0
## 93   YNP   2007          cougar       24            7       0        0
## 94   YNP   2008          cougar       24            5       0        0
## 95   YNP   2009          cougar       24            6       0        0
## 96   YNP   2010          cougar       24            4       0        0
## 97   YNP   2011          cougar       24            7       0        0
## 98   YNP   2012          cougar       24           11       0        0
## 99   YNP   2013          cougar       24           14       0        0
## 100  YNP   2016          cougar       24            8       0        0
## 101  YNP   2018          cougar       24           10       0        0
## 102  YNP   2019          cougar       24            6       0        0
## 103  YNP   2020          cougar       24            6       0        0
## 104  YNP   2017         crevice       25           NA       0        0
## 105  YNP   1996           druid       26            5       0        0
## 106  YNP   1998           druid       26            8       0        0
## 107  YNP   1999           druid       26            9       0        0
## 108  YNP   2001           druid       26           37       0        0
## 109  YNP   2002           druid       26           16       0        0
## 110  YNP   2003           druid       26           18       0        0
## 111  YNP   2004           druid       26           13       0        0
## 112  YNP   2005           druid       26            5       0        0
## 113  YNP   2006           druid       26           15       0        0
## 114  YNP   2007           druid       26           18       0        0
## 115  YNP   2008           druid       26           21       0        0
## 116  YNP   2009           druid       26           12       0        0
## 117  YNP   2010           druid       26            0       0        0
## 118  YNP   2008          everts       27            9       0        0
## 119  YNP   2009          everts       27           12       0        0
## 120  YNP   2002      geode/hell       28            9       0        0
## 121  YNP   2003      geode/hell       28            9       0        0
## 122  YNP   2004      geode/hell       28           12       0        0
## 123  YNP   2005      geode/hell       28            7       0        0
## 124  YNP   2003     gibbon/mary       29           NA       0        0
## 125  YNP   2005     gibbon/mary       29           10       0        0
## 126  YNP   2006     gibbon/mary       29           12       0        0
## 127  YNP   2007     gibbon/mary       29           18       0        0
## 128  YNP   2008     gibbon/mary       29           25       0        0
## 129  YNP   2010     gibbon/mary       29            8       0        0
## 130  YNP   2011     gibbon/mary       29           11       0        0
## 131  YNP   2012     gibbon/mary       29            0       0        0
## 132  YNP   2009        grayling       30            6       0        0
## 133  YNP   2004          hayden       31            4       0        0
## 134  YNP   2005          hayden       31            5       0        0
## 135  YNP   2006          hayden       31            7       0        0
## 136  YNP   2007          hayden       31            9       0        0
## 137  YNP   2019           heart       32            2       0        0
## 138  YNP   2020           heart       32            7       0        0
## 139  YNP   2015        junction       33           19       0        0
## 140  YNP   2017        junction       33            8       0        0
## 141  YNP   2018        junction       33           11       0        0
## 142  YNP   2020        junction       33           35       0        0
## 143  YNP   2010           lamar       34            7       0        0
## 144  YNP   2011           lamar       34           11       0        0
## 145  YNP   2014           lamar       34            8       0        0
## 146  YNP   2015           lamar       34           12       0        0
## 147  YNP   2016           lamar       34            4       0        0
## 148  YNP   2017           lamar       34            3       0        0
## 149  YNP   2008            lava       35            5       0        0
## 150  YNP   2009            lava       35            3       0        0
## 151  YNP   2010            lava       35            1       0        0
## 152  YNP   1996         leopold       36            5       0        0
## 153  YNP   1997         leopold       36           10       0        0
## 154  YNP   1998         leopold       36           12       0        0
## 155  YNP   1999         leopold       36           11       0        0
## 156  YNP   2000         leopold       36           15       0        0
## 157  YNP   2001         leopold       36           14       0        0
## 158  YNP   2002         leopold       36           16       0        0
## 159  YNP   2003         leopold       36           21       0        0
## 160  YNP   2004         leopold       36           28       0        0
## 161  YNP   2005         leopold       36           26       0        0
## 162  YNP   2006         leopold       36           20       0        0
## 163  YNP   2007         leopold       36           19       0        0
## 164  YNP   2008         leopold       36            7       0        0
## 165  YNP   1996        lonestar       37            0       0        0
## 166  YNP   1995         mollies       38            5       0        0
## 167  YNP   1996         mollies       38            2       0        0
## 168  YNP   1997         mollies       38            8       0        0
## 169  YNP   1998         mollies       38           16       0        0
## 170  YNP   1999         mollies       38           15       0        0
## 171  YNP   2000         mollies       38            5       0        0
## 172  YNP   2001         mollies       38           10       0        0
## 173  YNP   2002         mollies       38           13       0        0
## 174  YNP   2003         mollies       38            8       0        0
## 175  YNP   2004         mollies       38            9       0        0
## 176  YNP   2005         mollies       38            7       0        0
## 177  YNP   2006         mollies       38           11       0        0
## 178  YNP   2008         mollies       38           15       0        0
## 179  YNP   2009         mollies       38           17       0        0
## 180  YNP   2010         mollies       38           17       0        0
## 181  YNP   2014         mollies       38           12       0        0
## 182  YNP   2015         mollies       38           17       0        0
## 183  YNP   2016         mollies       38           18       0        0
## 184  YNP   2017         mollies       38           15       0        0
## 185  YNP   2018         mollies       38           10       0        0
## 186  YNP   2020         mollies       38            8       0        0
## 187  YNP   2006        nezperce       39            0       0        0
## 188  YNP   1998        nezperce       39            7       0        0
## 189  YNP   1999        nezperce       39           12       0        0
## 190  YNP   2001        nezperce       39           19       0        0
## 191  YNP   2002        nezperce       39           18       0        0
## 192  YNP   2003        nezperce       39           18       0        0
## 193  YNP   2004        nezperce       39           14       0        0
## 194  YNP   2005        nezperce       39           11       0        0
## 195  YNP   2006           oxbow       40           12       0        0
## 196  YNP   2007           oxbow       40           24       0        0
## 197  YNP   2008           oxbow       40           20       0        0
## 198  YNP   2015        prospect       42           13       0        0
## 199  YNP   2016        prospect       42           12       0        0
## 200  YNP   2007        quadrant       43           NA       0        0
## 201  YNP   2008        quadrant       43            4       0        0
## 202  YNP   2009        quadrant       43            7       0        0
## 203  YNP   2010        quadrant       43            7       0        0
## 204  YNP   1996            rose       44           11       0        0
## 205  YNP   1997            rose       44           16       0        0
## 206  YNP   1999            rose       44           22       0        0
## 207  YNP   2000            rose       44           21       0        0
## 208  YNP   2001            rose       44           10       0        0
## 209  YNP   2002            rose       44           11       0        0
## 210  YNP   2003          slough       45           10       0        0
## 211  YNP   2004          slough       45           21       0        0
## 212  YNP   2005          slough       45           15       0        0
## 213  YNP   2006          slough       45            9       0        0
## 214  YNP   2004 specimen/silver       47            5       0        0
## 215  YNP   2010 specimen/silver       47            8       0        0
## 216  YNP   2000            swan       48            7       0        0
## 217  YNP   2001            swan       48            9       0        0
## 218  YNP   2002            swan       48           18       0        0
## 219  YNP   2003            swan       48           20       0        0
## 220  YNP   2004            swan       48           10       0        0
## 221  YNP   2005            swan       48            3       0        0
## 222  YNP   1996       thorofare       49            2       0        0
## 223  YNP   1997       thorofare       49            8       0        0
## 224  YNP   1998       thorofare       49            0       0        0
## 225  YNP   2001           tower       50            4       0        0
## 226  YNP   2002           tower       50            2       0        0
## 227  YNP   2014          wapiti       51            2       0        0
## 228  YNP   2016          wapiti       51            9       0        0
## 229  YNP   2017          wapiti       51           21       0        0
## 230  YNP   2018          wapiti       51           18       0        0
## 231  YNP   2019          wapiti       51           19       0        0
## 232  YNP   1995       yelldelta       52            6       0        0
## 233  YNP   1996       yelldelta       52            5       0        0
## 234  YNP   1997       yelldelta       52            8       0        0
## 235  YNP   1998       yelldelta       52            8       0        0
## 236  YNP   1999       yelldelta       52            6       0        0
## 237  YNP   2000       yelldelta       52           15       0        0
## 238  YNP   2001       yelldelta       52           18       0        0
## 239  YNP   2002       yelldelta       52           17       0        0
## 240  YNP   2003       yelldelta       52           17       0        0
## 241  YNP   2004       yelldelta       52           19       0        0
## 242  YNP   2005       yelldelta       52           19       0        0
## 243  YNP   2006       yelldelta       52           15       0        0
## 244  YNP   2009       yelldelta       52            4       0        0
## 245  YNP   2010       yelldelta       52            9       0        0
## 246  YNP   2011       yelldelta       52           13       0        0
## 247  YNP   2012       yelldelta       52           11       0        0
## 248  YNP   2013       yelldelta       52           17       0        0
##     mort_lead mort_nonlead reprody1 persisty1
## 1           1            3        0         0
## 2           0            3        1         1
## 3           3            0        0         0
## 4           0            3        1         1
## 5           0            3        1         1
## 6           1            1        0         0
## 7           0            2        1         1
## 8           0            2        1         1
## 9           1            1        1         1
## 10          0            2        1         1
## 11          0            2        1         1
## 12          1            1        0         0
## 13          0            2        1         1
## 14          0            1        0         0
## 15          1            0        0         0
## 16          0            1        1         1
## 17          1            0        0         0
## 18          0            1        0         1
## 19          1            0        1         1
## 20          0            1        1         1
## 21          0            1        1         1
## 22          0            1        1         1
## 23          0            1        1         1
## 24          0            1        1         1
## 25          1            0        1         1
## 26          0            1        1         1
## 27          1            0        1         1
## 28         NA           NA        0         1
## 29          0            3        1         1
## 30          1            1        1         1
## 31          0            2        1         1
## 32          0            1        1         1
## 33          0            1        1         1
## 34          0            1        1         1
## 35          0            1        0         1
## 36          1            0        1         1
## 37          0            1        1         1
## 38          0            1        1         1
## 39          0            1        1         1
## 40          1            0       NA         0
## 41          0            1        1         1
## 42          0            1        0         1
## 43          0            1        1         1
## 44          0            1        1         1
## 45          0            1        1         1
## 46          0            1        1         1
## 47          0            1        1         1
## 48          0            1        1         1
## 49          1            0        1         1
## 50          0            0       NA         1
## 51          0            0       NA         0
## 52          0            0        0         0
## 53          0            0        1         0
## 54          0            0        1         1
## 55          0            0        1         1
## 56          0            0        1         1
## 57          0            0        1         1
## 58          0            0        1         1
## 59          0            0        1         1
## 60          0            0        1         1
## 61          0            0        1         1
## 62          0            0        1         1
## 63          0            0        1         1
## 64          0            0        1         1
## 65          0            0        1         1
## 66          0            0        1         1
## 67          0            0        1         1
## 68          0            0        1         1
## 69          0            0        0         0
## 70          0            0        1         1
## 71          0            0        0         1
## 72          0            0        1         1
## 73          0            0        1         1
## 74          0            0        1         1
## 75          0            0        0         0
## 76          0            0        1         1
## 77          0            0        1         1
## 78          0            0        1         1
## 79          0            0        1         1
## 80          0            0        1         1
## 81          0            0        1         1
## 82          0            0        1         1
## 83          0            0        1         1
## 84          0            0        0         0
## 85          0            0        1         1
## 86          0            0        1         1
## 87          0            0        1         1
## 88          0            0        1         1
## 89          0            0        1         1
## 90          0            0        1         1
## 91          0            0        1         1
## 92          0            0        1         1
## 93          0            0        0         1
## 94          0            0        1         1
## 95          0            0        1         1
## 96          0            0        1         1
## 97          0            0        1         1
## 98          0            0        1         1
## 99          0            0        1         1
## 100         0            0        1         1
## 101         0            0        1         1
## 102         0            0        1         1
## 103         0            0        1         1
## 104         0            0        1         1
## 105         0            0        1         1
## 106         0            0        1         1
## 107         0            0        1         1
## 108         0            0        1         1
## 109         0            0        1         1
## 110         0            0        1         1
## 111         0            0        1         1
## 112         0            0        1         1
## 113         0            0        1         1
## 114         0            0        1         1
## 115         0            0        1         1
## 116         0            0        0         0
## 117         0            0        0        NA
## 118         0            0        1         1
## 119         0            0        0         0
## 120         0            0        1         1
## 121         0            0        1         1
## 122         0            0        1         1
## 123         0            0        1         1
## 124         0            0        1         1
## 125         0            0        1         1
## 126         0            0        1         1
## 127         0            0        1         1
## 128         0            0        1         1
## 129         0            0        1         1
## 130         0            0        1         1
## 131         0            0        0         0
## 132         0            0       NA         1
## 133         0            0        1         1
## 134         0            0        1         1
## 135         0            0        1         1
## 136         0            0       NA         0
## 137         0            0        1         1
## 138         0            0        1         1
## 139         0            0        1         1
## 140         0            0        1         1
## 141         0            0        1         1
## 142         0            0        1         1
## 143         0            0        1         1
## 144         0            0        1         1
## 145         0            0        1         1
## 146         0            0        1         1
## 147         0            0        1         1
## 148         0            0        1         1
## 149         0            0        1         1
## 150         0            0        1         1
## 151         0            0        0         0
## 152         0            0        1         1
## 153         0            0        1         1
## 154         0            0        1         1
## 155         0            0        1         1
## 156         0            0        1         1
## 157         0            0        1         1
## 158         0            0        1         1
## 159         0            0        1         1
## 160         0            0        1         1
## 161         0            0        1         1
## 162         0            0        1         1
## 163         0            0        1         1
## 164         0            0        0         0
## 165         0            0        0         0
## 166         0            0        1         1
## 167         0            0        1         1
## 168         0            0        1         1
## 169         0            0        1         1
## 170         0            0        0         1
## 171         0            0        1         1
## 172         0            0        1         1
## 173         0            0        1         1
## 174         0            0        1         1
## 175         0            0        0         1
## 176         0            0        1         1
## 177         0            0        1         1
## 178         0            0        1         1
## 179         0            0        1         1
## 180         0            0        1         1
## 181         0            0        1         1
## 182         0            0        1         1
## 183         0            0        1         1
## 184         0            0        1         1
## 185         0            0        1         1
## 186         0            0        1         1
## 187         0            0        0        NA
## 188         0            0        1         1
## 189         0            0        1         1
## 190         0            0        1         1
## 191         0            0        1         1
## 192         0            0        1         1
## 193         0            0        1         1
## 194         0            0        0         0
## 195         0            0        1         1
## 196         0            0        1         1
## 197         0            0        0         0
## 198         0            0        1         1
## 199         0            0        1         1
## 200         0            0        1         1
## 201         0            0        1         1
## 202         0            0        1         1
## 203         0            0        0         1
## 204         0            0        1         1
## 205         0            0        1         1
## 206         0            0        1         1
## 207         0            0        1         1
## 208         0            0        1         1
## 209         0            0        1         1
## 210         0            0        1         1
## 211         0            0        1         1
## 212         0            0        1         1
## 213         0            0        1         1
## 214         0            0       NA         1
## 215         0            0        0         0
## 216         0            0        1         1
## 217         0            0        1         1
## 218         0            0        1         1
## 219         0            0        1         1
## 220         0            0        1         1
## 221         0            0        1         1
## 222         0            0        1         1
## 223         0            0        0         0
## 224         0            0       NA        NA
## 225         0            0        1         1
## 226         0            0        0         0
## 227         0            0        1         1
## 228         0            0        1         1
## 229         0            0        1         1
## 230         0            0        1         1
## 231         0            0        1         1
## 232         0            0        1         1
## 233         0            0        1         1
## 234         0            0        0         1
## 235         0            0        0         1
## 236         0            0        1         1
## 237         0            0        1         1
## 238         0            0        1         1
## 239         0            0        1         1
## 240         0            0        1         1
## 241         0            0        1         1
## 242         0            0        1         1
## 243         0            0        1         1
## 244         0            0        1         1
## 245         0            0        1         1
## 246         0            0        0         1
## 247         0            0        1         1
## 248         0            0        1         1
```


Problem 7. (3 points) Among the Yellowstone wolf packs, the [Druid Peak Pack](https://www.pbs.org/wnet/nature/in-the-valley-of-the-wolves-the-druid-wolf-pack-story/209/) is one of most famous. What was the average pack size of this pack for the years represented in the data?

**The average pack size of this pack is as follow**

```r
ynp %>% 
  filter(pack == "druid") %>% 
  group_by(biolyr) %>% 
  summarize(avg_pack = mean(packsize_aug, na.rm = TRUE))
```

```
## # A tibble: 15 × 2
##    biolyr avg_pack
##     <int>    <dbl>
##  1   1996        5
##  2   1997        5
##  3   1998        8
##  4   1999        9
##  5   2000       27
##  6   2001       37
##  7   2002       16
##  8   2003       18
##  9   2004       13
## 10   2005        5
## 11   2006       15
## 12   2007       18
## 13   2008       21
## 14   2009       12
## 15   2010        0
```

Problem 8. (4 points) Pack dynamics can be hard to predict- even for strong packs like the Druid Peak pack. At which year did the Druid Peak pack have the largest pack size? What do you think happened in 2010?
**2001 has the largest pack size among the recorded biological years**

```r
ynp %>% 
  filter(pack == "druid") %>% 
  group_by(biolyr) %>% 
  summarize(largest = max(packsize_aug)) %>% 
  arrange(desc(largest))
```

```
## # A tibble: 15 × 2
##    biolyr largest
##     <int>   <dbl>
##  1   2001      37
##  2   2000      27
##  3   2008      21
##  4   2003      18
##  5   2007      18
##  6   2002      16
##  7   2006      15
##  8   2004      13
##  9   2009      12
## 10   1999       9
## 11   1998       8
## 12   1996       5
## 13   1997       5
## 14   2005       5
## 15   2010       0
```

**I think in 2010, the pack size is 0 because the wolf pack might have migrated, there are zero mortalities recorded and reproduction data is also zero.  There are also possibility that the wolves are all dead due to fights and aggressive interactions with other wolves packs.**

```r
ynp %>% 
  filter(pack == "druid") %>% 
  filter(biolyr == 2010) %>% 
  group_by(packsize_aug)
```

```
## # A tibble: 1 × 11
## # Groups:   packsize_aug [1]
##   park  biolyr pack  packcode packsize_aug mort_yn mort_all mort_lead
##   <chr>  <int> <chr>    <int>        <dbl>   <int>    <int>     <int>
## 1 YNP     2010 druid       26            0       0        0         0
## # ℹ 3 more variables: mort_nonlead <int>, reprody1 <int>, persisty1 <int>
```

Problem 9. (5 points) Among the YNP wolf packs, which one has had the highest overall persistence `persisty1` for the years represented in the data? Look this pack up online and tell me what is unique about its behavior- specifically, what prey animals does this pack specialize on?  
**Pack mollies had the highest overall persistence for the years represented in the data.  This wolf pack is specialized in hunting bison and having regular interaction with bears. Reference:https://www.yellowstonewolf.org/yellowstones_wolves.php?pack_id=6**


```r
ynp %>% 
  group_by(pack) %>% 
  summarize(overall = sum(persisty1)) %>% 
  arrange(desc(overall))
```

```
## # A tibble: 46 × 2
##    pack        overall
##    <chr>         <int>
##  1 mollies          26
##  2 cougar           20
##  3 yelldelta        18
##  4 leopold          12
##  5 agate            10
##  6 8mile             9
##  7 canyon            9
##  8 gibbon/mary       9
##  9 junction          8
## 10 lamar             8
## # ℹ 36 more rows
```

Problem 10. (3 points) Perform one analysis or exploration of your choice on the `wolves` data. Your answer needs to include at least two lines of code and not be a summary function.  
**I chose to do a mean, max, min analysis on the 'wolves' data using their pack size.  First I filter out the NA's in the data set, then group the data by 'pack', follow by using summarize function to perform the mean, max, min analysis, lastly, I arrange then from the smallest to largest pack size.**

```r
wolves %>% 
  filter(!is.na(packsize_aug)) %>% 
  filter(packsize_aug >20) %>% 
  group_by(pack) %>% 
  summarize(avg_size = mean(packsize_aug),
            max_size = max(packsize_aug),
            min_size = min(packsize_aug)) %>% 
  arrange(max_size)
```

```
## # A tibble: 19 × 4
##    pack        avg_size max_size min_size
##    <chr>          <dbl>    <dbl>    <dbl>
##  1 Huckleberry     20.4     20.4     20.4
##  2 nezperce        22       22       22  
##  3 slough          21.5     22       21  
##  4 yelldelta       22       22       22  
##  5 100 Mile        23       23       23  
##  6 Birch Creek     23       23       23  
##  7 Little Bear     23       23       23  
##  8 mollies         23       23       23  
##  9 rose            22       23       21  
## 10 wapiti          22       23       21  
## 11 70 Mile         24       24       24  
## 12 oxbow           24       24       24  
## 13 gibbon/mary     25.5     26       25  
## 14 Buffalo         26.4     26.4     26.4
## 15 8mile           24.5     27       22  
## 16 leopold         25       28       21  
## 17 East Fork       30       33       27  
## 18 junction        28       35       21  
## 19 druid           28.3     37       21
```

