# USDA: <Predicted attribute> regression
bdanalytics  

**  **    
**Date: (Sun) Mar 08, 2015**    

# Introduction:  

Data: 
Source: 
Time period: 



# Synopsis:

Based on analysis utilizing <> techniques, <conclusion heading>:  

### ![](<filename>.png)

## Potential next steps include:

# Analysis: 

```r
rm(list=ls())
set.seed(12345)
options(stringsAsFactors=FALSE)
source("~/Dropbox/datascience/R/mydsutils.R")
source("~/Dropbox/datascience/R/myplot.R")
source("~/Dropbox/datascience/R/mypetrinet.R")
# Gather all package requirements here
#suppressPackageStartupMessages(require())

#require(sos); findFn("pinv", maxPages=2, sortby="MaxScore")

# Analysis sepcific global variables
glb_separate_predict_dataset <- FALSE

script_df <- data.frame(chunk_label="import_data", chunk_step_major=1, chunk_step_minor=0)
print(script_df)
```

```
##   chunk_label chunk_step_major chunk_step_minor
## 1 import_data                1                0
```

## Step `1`: import data

```r
entity_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/USDA.csv", 
    print_diagn=TRUE)
```

```
## [1] "Reading file ./data/USDA.csv..."
## [1] "dimensions of data in ./data/USDA.csv: 7,058 rows x 16 cols"
##     ID              Description Calories Protein TotalFat Carbohydrate
## 1 1001         BUTTER,WITH SALT      717    0.85    81.11         0.06
## 2 1002 BUTTER,WHIPPED,WITH SALT      717    0.85    81.11         0.06
## 3 1003     BUTTER OIL,ANHYDROUS      876    0.28    99.48         0.00
## 4 1004              CHEESE,BLUE      353   21.40    28.74         2.34
## 5 1005             CHEESE,BRICK      371   23.24    29.68         2.79
## 6 1006              CHEESE,BRIE      334   20.75    27.68         0.45
##   Sodium SaturatedFat Cholesterol Sugar Calcium Iron Potassium VitaminC
## 1    714       51.368         215  0.06      24 0.02        24        0
## 2    827       50.489         219  0.06      24 0.16        26        0
## 3      2       61.924         256  0.00       4 0.00         5        0
## 4   1395       18.669          75  0.50     528 0.31       256        0
## 5    560       18.764          94  0.51     674 0.43       136        0
## 6    629       17.410         100  0.45     184 0.50       152        0
##   VitaminE VitaminD
## 1     2.32      1.5
## 2     2.32      1.5
## 3     2.80      1.8
## 4     0.25      0.5
## 5     0.26      0.5
## 6     0.24      0.5
##         ID                                                 Description
## 1174  6464 CAMPBELL'S CHNKY SOUPS,TANTALIZIN' TRKY - TRKY CHILI BNS SP
## 3221 11836                   POTATOES,MICROWAVED,CKD,IN SKN,SKN W/SALT
## 5089 17345                    GAME MEAT,DEER,LOIN,LN,1" STEAK,CKD,BRLD
## 5370 18436     DOUGHNUTS,YEAST-LEAVENED,GLAZED,UNENR (INCL HONEY BUNS)
## 6181 22963                         LEAN POCKETS,MEATBALLS & MOZZARELLA
## 6252 23081          BEEF,SHLDR POT RST,BNLESS,LN,0" FAT,CHOIC,CKD,BRSD
##      Calories Protein TotalFat Carbohydrate Sodium SaturatedFat
## 1174       78    6.12     1.02        11.02    359        0.408
## 3221      132    4.39     0.10        29.63    252        0.026
## 5089      150   30.20     2.38         0.00     57        0.878
## 5370      403    6.40    22.80        44.30    342        5.813
## 6181      240   10.49     7.70        32.28    557        3.087
## 6252      200   31.32     8.30         0.00     60        2.835
##      Cholesterol Sugar Calcium Iron Potassium VitaminC VitaminE VitaminD
## 1174           6  3.67      24 0.73        NA      1.0       NA       NA
## 3221           0    NA      46 5.94       650     15.3       NA      0.0
## 5089          79  0.00       6 4.09       398      0.0     0.62      0.0
## 5370           6    NA      43 0.60       108      0.1       NA       NA
## 6181          20  8.81     196 2.06       221       NA       NA       NA
## 6252          97  0.00      13 3.76       358      0.0     0.13      0.1
##         ID                Description Calories Protein TotalFat
## 7053 48052         VITAL WHEAT GLUTEN      370   75.16     1.85
## 7054 80200              FROG LEGS,RAW       73   16.40     0.30
## 7055 83110            MACKEREL,SALTED      305   18.50    25.10
## 7056 90240 SCALLOP,(BAY&SEA),CKD,STMD      111   20.54     0.84
## 7057 90560                  SNAIL,RAW       90   16.10     1.40
## 7058 93600           TURTLE,GREEN,RAW       89   19.80     0.50
##      Carbohydrate Sodium SaturatedFat Cholesterol Sugar Calcium Iron
## 7053        13.79     29        0.272           0     0     142 5.20
## 7054         0.00     58        0.076          50     0      18 1.50
## 7055         0.00   4450        7.148          95     0      66 1.40
## 7056         5.41    667        0.218          41     0      10 0.58
## 7057         2.00     70        0.361          50     0      10 3.50
## 7058         0.00     68        0.127          50     0     118 1.40
##      Potassium VitaminC VitaminE VitaminD
## 7053       100        0     0.00      0.0
## 7054       285        0     1.00      0.2
## 7055       520        0     2.38     25.2
## 7056       314        0     0.00      0.0
## 7057       382        0     5.00      0.0
## 7058       230        0     0.50      0.0
## 'data.frame':	7058 obs. of  16 variables:
##  $ ID          : int  1001 1002 1003 1004 1005 1006 1007 1008 1009 1010 ...
##  $ Description : chr  "BUTTER,WITH SALT" "BUTTER,WHIPPED,WITH SALT" "BUTTER OIL,ANHYDROUS" "CHEESE,BLUE" ...
##  $ Calories    : int  717 717 876 353 371 334 300 376 403 387 ...
##  $ Protein     : num  0.85 0.85 0.28 21.4 23.24 ...
##  $ TotalFat    : num  81.1 81.1 99.5 28.7 29.7 ...
##  $ Carbohydrate: num  0.06 0.06 0 2.34 2.79 0.45 0.46 3.06 1.28 4.78 ...
##  $ Sodium      : int  714 827 2 1395 560 629 842 690 621 700 ...
##  $ SaturatedFat: num  51.4 50.5 61.9 18.7 18.8 ...
##  $ Cholesterol : int  215 219 256 75 94 100 72 93 105 103 ...
##  $ Sugar       : num  0.06 0.06 0 0.5 0.51 0.45 0.46 NA 0.52 NA ...
##  $ Calcium     : int  24 24 4 528 674 184 388 673 721 643 ...
##  $ Iron        : num  0.02 0.16 0 0.31 0.43 0.5 0.33 0.64 0.68 0.21 ...
##  $ Potassium   : int  24 26 5 256 136 152 187 93 98 95 ...
##  $ VitaminC    : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ VitaminE    : num  2.32 2.32 2.8 0.25 0.26 0.24 0.21 NA 0.29 NA ...
##  $ VitaminD    : num  1.5 1.5 1.8 0.5 0.5 0.5 0.4 NA 0.6 NA ...
## NULL
```

```r
if (glb_separate_predict_dataset) {
    predict_df <- myimport_data(
        url="<url>", 
        print_diagn=TRUE)
} else {
    predict_df <- entity_df[sample(1:nrow(entity_df), nrow(entity_df) / 1000),]
    myprint_df(predict_df)
    str(predict_df)
}
```

```
##         ID                                                  Description
## 2295 10044 PORK,FRSH,LOIN,CNTR RIB (CHOPS OR ROASTS),BONE-IN,LN&FAT,RAW
## 3594 13384 BEEF,CHUCK,UNDER BLADE POT RST,BNLESS,LN,0" FAT,SEL,CKD,BRSD
## 5135 18051                                        BREAD,RED-CAL,OATMEAL
## 6983 43325                 PORK,CURED,HAM,BNLESS,LO NA,EX LN & REG,RSTD
## 244   2025                                                NUTMEG,GROUND
## 1075  6355             CAMPBELL'S RED & WHITE,GOLDEN MUSHROOM SOUP,COND
## 5189 18180                                      COOKIES,OATMEAL,DRY MIX
##      Calories Protein TotalFat Carbohydrate Sodium SaturatedFat
## 2295      186   20.28    11.04         0.00     56        2.365
## 3594      216   30.68     9.44         0.00     67        3.487
## 5135      210    7.60     3.50        43.30    388        0.599
## 6983      165   22.00     7.70         0.50    969        2.624
## 244       525    5.84    36.31        49.29     16       25.940
## 1075       65    1.61     2.82         8.06    524        0.806
## 5189      462    6.50    19.20        67.30    473        4.755
##      Cholesterol Sugar Calcium Iron Potassium VitaminC VitaminE VitaminD
## 2295          58  0.00      25 0.59       337      0.0     0.12      0.7
## 3594         102  0.00      14 3.18       306      0.0     0.06      0.1
## 5135           0    NA     115 2.30       124      0.2       NA       NA
## 6983          57  0.00       8 1.40       362      0.0     0.28      0.8
## 244            0 28.49     184 3.04       350      3.0     0.00      0.0
## 1075           0  0.81       0   NA       548      0.0       NA       NA
## 5189           0    NA      25 2.22       183      0.1       NA       NA
## 'data.frame':	7 obs. of  16 variables:
##  $ ID          : int  10044 13384 18051 43325 2025 6355 18180
##  $ Description : chr  "PORK,FRSH,LOIN,CNTR RIB (CHOPS OR ROASTS),BONE-IN,LN&FAT,RAW" "BEEF,CHUCK,UNDER BLADE POT RST,BNLESS,LN,0\" FAT,SEL,CKD,BRSD" "BREAD,RED-CAL,OATMEAL" "PORK,CURED,HAM,BNLESS,LO NA,EX LN & REG,RSTD" ...
##  $ Calories    : int  186 216 210 165 525 65 462
##  $ Protein     : num  20.28 30.68 7.6 22 5.84 ...
##  $ TotalFat    : num  11.04 9.44 3.5 7.7 36.31 ...
##  $ Carbohydrate: num  0 0 43.3 0.5 49.3 ...
##  $ Sodium      : int  56 67 388 969 16 524 473
##  $ SaturatedFat: num  2.365 3.487 0.599 2.624 25.94 ...
##  $ Cholesterol : int  58 102 0 57 0 0 0
##  $ Sugar       : num  0 0 NA 0 28.5 ...
##  $ Calcium     : int  25 14 115 8 184 0 25
##  $ Iron        : num  0.59 3.18 2.3 1.4 3.04 NA 2.22
##  $ Potassium   : int  337 306 124 362 350 548 183
##  $ VitaminC    : num  0 0 0.2 0 3 0 0.1
##  $ VitaminE    : num  0.12 0.06 NA 0.28 0 NA NA
##  $ VitaminD    : num  0.7 0.1 NA 0.8 0 NA NA
```

```r
script_df <- rbind(script_df, 
                   data.frame(chunk_label="inspect_data", 
                              chunk_step_major=max(script_df$chunk_step_major)+1, 
                              chunk_step_minor=1))
print(script_df)
```

```
##    chunk_label chunk_step_major chunk_step_minor
## 1  import_data                1                0
## 2 inspect_data                2                1
```

### Step `2`.`1`: inspect data

```r
#print(str(entity_df))
#View(entity_df)

# List info gathered for various columns
# steps:
# date:
# interval:

print(summary(entity_df))
```

```
##        ID        Description           Calories        Protein     
##  Min.   : 1001   Length:7058        Min.   :  0.0   Min.   : 0.00  
##  1st Qu.: 8387   Class :character   1st Qu.: 85.0   1st Qu.: 2.29  
##  Median :13294   Mode  :character   Median :181.0   Median : 8.20  
##  Mean   :14260                      Mean   :219.7   Mean   :11.71  
##  3rd Qu.:18337                      3rd Qu.:331.0   3rd Qu.:20.43  
##  Max.   :93600                      Max.   :902.0   Max.   :88.32  
##                                     NA's   :1       NA's   :1      
##     TotalFat       Carbohydrate        Sodium         SaturatedFat   
##  Min.   :  0.00   Min.   :  0.00   Min.   :    0.0   Min.   : 0.000  
##  1st Qu.:  0.72   1st Qu.:  0.00   1st Qu.:   37.0   1st Qu.: 0.172  
##  Median :  4.37   Median :  7.13   Median :   79.0   Median : 1.256  
##  Mean   : 10.32   Mean   : 20.70   Mean   :  322.1   Mean   : 3.452  
##  3rd Qu.: 12.70   3rd Qu.: 28.17   3rd Qu.:  386.0   3rd Qu.: 4.028  
##  Max.   :100.00   Max.   :100.00   Max.   :38758.0   Max.   :95.600  
##  NA's   :1        NA's   :1        NA's   :84        NA's   :301     
##   Cholesterol          Sugar           Calcium             Iron        
##  Min.   :   0.00   Min.   : 0.000   Min.   :   0.00   Min.   :  0.000  
##  1st Qu.:   0.00   1st Qu.: 0.000   1st Qu.:   9.00   1st Qu.:  0.520  
##  Median :   3.00   Median : 1.395   Median :  19.00   Median :  1.330  
##  Mean   :  41.55   Mean   : 8.257   Mean   :  73.53   Mean   :  2.828  
##  3rd Qu.:  69.00   3rd Qu.: 7.875   3rd Qu.:  56.00   3rd Qu.:  2.620  
##  Max.   :3100.00   Max.   :99.800   Max.   :7364.00   Max.   :123.600  
##  NA's   :288       NA's   :1910     NA's   :136       NA's   :123      
##    Potassium          VitaminC           VitaminE          VitaminD       
##  Min.   :    0.0   Min.   :   0.000   Min.   :  0.000   Min.   :  0.0000  
##  1st Qu.:  135.0   1st Qu.:   0.000   1st Qu.:  0.120   1st Qu.:  0.0000  
##  Median :  250.0   Median :   0.000   Median :  0.270   Median :  0.0000  
##  Mean   :  301.4   Mean   :   9.436   Mean   :  1.488   Mean   :  0.5769  
##  3rd Qu.:  348.0   3rd Qu.:   3.100   3rd Qu.:  0.710   3rd Qu.:  0.1000  
##  Max.   :16500.0   Max.   :2400.000   Max.   :149.400   Max.   :250.0000  
##  NA's   :409       NA's   :332        NA's   :2720      NA's   :2834
```

```r
print(summary(predict_df))
```

```
##        ID        Description           Calories        Protein     
##  Min.   : 2025   Length:7           Min.   : 65.0   Min.   : 1.61  
##  1st Qu.: 8200   Class :character   1st Qu.:175.5   1st Qu.: 6.17  
##  Median :13384   Mode  :character   Median :210.0   Median : 7.60  
##  Mean   :15909                      Mean   :261.3   Mean   :13.50  
##  3rd Qu.:18116                      3rd Qu.:339.0   3rd Qu.:21.14  
##  Max.   :43325                      Max.   :525.0   Max.   :30.68  
##                                                                    
##     TotalFat      Carbohydrate       Sodium       SaturatedFat   
##  Min.   : 2.82   Min.   : 0.00   Min.   : 16.0   Min.   : 0.599  
##  1st Qu.: 5.60   1st Qu.: 0.25   1st Qu.: 61.5   1st Qu.: 1.585  
##  Median : 9.44   Median : 8.06   Median :388.0   Median : 2.624  
##  Mean   :12.86   Mean   :24.06   Mean   :356.1   Mean   : 5.797  
##  3rd Qu.:15.12   3rd Qu.:46.30   3rd Qu.:498.5   3rd Qu.: 4.121  
##  Max.   :36.31   Max.   :67.30   Max.   :969.0   Max.   :25.940  
##                                                                  
##   Cholesterol        Sugar          Calcium         Iron      
##  Min.   :  0.0   Min.   : 0.00   Min.   :  0   Min.   :0.590  
##  1st Qu.:  0.0   1st Qu.: 0.00   1st Qu.: 11   1st Qu.:1.605  
##  Median :  0.0   Median : 0.00   Median : 25   Median :2.260  
##  Mean   : 31.0   Mean   : 5.86   Mean   : 53   Mean   :2.122  
##  3rd Qu.: 57.5   3rd Qu.: 0.81   3rd Qu.: 70   3rd Qu.:2.855  
##  Max.   :102.0   Max.   :28.49   Max.   :184   Max.   :3.180  
##                  NA's   :2                     NA's   :1      
##    Potassium        VitaminC         VitaminE        VitaminD    
##  Min.   :124.0   Min.   :0.0000   Min.   :0.000   Min.   :0.000  
##  1st Qu.:244.5   1st Qu.:0.0000   1st Qu.:0.045   1st Qu.:0.075  
##  Median :337.0   Median :0.0000   Median :0.090   Median :0.400  
##  Mean   :315.7   Mean   :0.4714   Mean   :0.115   Mean   :0.400  
##  3rd Qu.:356.0   3rd Qu.:0.1500   3rd Qu.:0.160   3rd Qu.:0.725  
##  Max.   :548.0   Max.   :3.0000   Max.   :0.280   Max.   :0.800  
##                                   NA's   :3       NA's   :3
```

```r
#pairs(subset(entity_df, select=-c(col_symbol)))

#   Histogram of predictor in entity_df & predict_df
# Check for predict_df & entity_df features range mismatches

# Create new features that help diagnostics
#   Convert factors to dummy variables
#   Build splines   require(splines); bsBasis <- bs(training$age, df=3)

script_df <- rbind(script_df, 
                   data.frame(chunk_label="extract_features", 
                              chunk_step_major=max(script_df$chunk_step_major)+1, 
                              chunk_step_minor=0))
print(script_df)
```

```
##        chunk_label chunk_step_major chunk_step_minor
## 1      import_data                1                0
## 2     inspect_data                2                1
## 3 extract_features                3                0
```

## Step `3`: extract features

```r
require(plyr)
```

```
## Loading required package: plyr
```

```r
entity_df <- mutate(entity_df, 
    HighSodium = as.numeric(Sodium >= mean(entity_df$Sodium, na.rm=TRUE)),
    HighProtein = as.numeric(Protein >= mean(entity_df$Protein, na.rm=TRUE)),
    HighCarbs = as.numeric(Carbohydrate >= mean(entity_df$Carbohydrate, na.rm=TRUE)),
    HighFat = as.numeric(TotalFat >= mean(entity_df$TotalFat, na.rm=TRUE))
                    )

table(entity_df$HighSodium)
```

```
## 
##    0    1 
## 4884 2090
```

```r
table(entity_df$HighSodium, entity_df$HighFat)
```

```
##    
##        0    1
##   0 3529 1355
##   1 1378  712
```

```r
tapply(entity_df$Iron, entity_df$HighProtein, mean, na.rm=TRUE)
```

```
##        0        1 
## 2.558945 3.197294
```

```r
tapply(entity_df$VitaminC, entity_df$HighCarbs, max, na.rm=TRUE)
```

```
##      0      1 
## 1677.6 2400.0
```

```r
tapply(entity_df$VitaminC, entity_df$HighCarbs, summary)
```

```
## $`0`
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
##    0.000    0.000    0.000    6.364    2.800 1678.000      248 
## 
## $`1`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    0.00    0.00    0.20   16.31    4.50 2400.00      83
```

```r
# script_df <- rbind(script_df, 
#                    data.frame(chunk_label="extract_features", 
#                               chunk_step_major=max(script_df$chunk_step_major)+1, 
#                               chunk_step_minor=0))
print(script_df)
```

```
##        chunk_label chunk_step_major chunk_step_minor
## 1      import_data                1                0
## 2     inspect_data                2                1
## 3 extract_features                3                0
```

Null Hypothesis ($\sf{H_{0}}$): mpg is not impacted by am_fctr.  
The variance by am_fctr appears to be independent. 

```r
# print(t.test(subset(cars_df, am_fctr == "automatic")$mpg, 
#              subset(cars_df, am_fctr == "manual")$mpg, 
#              var.equal=FALSE)$conf)
```
We reject the null hypothesis i.e. we have evidence to conclude that am_fctr impacts mpg (95% confidence). Manual transmission is better for miles per gallon versus automatic transmission.

## remove nearZeroVar features (not much variance)
#require(reshape)
#var_features_df <- melt(summaryBy(. ~ factor(0), data=entity_df[, features_lst], 
#                             FUN=var, keep.names=TRUE), 
#                             variable_name=c("feature"))
#names(var_features_df)[2] <- "var"
#print(var_features_df[order(var_features_df$var), ])
# summaryBy ignores factors whereas nearZeroVar inspects factors

# k_fold <- 5
# entity_df[order(entity_df$classe, 
#                   entity_df$user_name, 
#                   entity_df$my.rnorm),"my.cv_ix"] <- 
#     rep(1:k_fold, length.out=nrow(entity_df))
# summaryBy(X ~ my.cv_ix, data=entity_df, FUN=length)
# tapply(entity_df$X, list(entity_df$classe, entity_df$user_name, 
#                            entity_df$my.cv_ix), length)

#require(DAAG)
#entity_df$classe.proper <- as.numeric(entity_df$classe == "A")
#rnorm.glm <- glm(classe.proper ~ rnorm, family=binomial, data=entity_df)
#cv.binary(rnorm.glm, nfolds=k_fold, print.details=TRUE)
#result <- cv.lm(df=entity_df, form.lm=formula(classe ~ rnorm), 
#                    m=k_fold, seed=12345, printit=TRUE)

#plot(mdl_1$finalModel, uniform=TRUE, main="base")
#text(mdl_1$finalModel, use.n=TRUE, all=TRUE, cex=0.8)




```
## R version 3.1.2 (2014-10-31)
## Platform: x86_64-apple-darwin13.4.0 (64-bit)
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] plyr_1.8.1      sos_1.3-8       brew_1.0-6      doBy_4.5-13    
## [5] survival_2.38-1 ggplot2_1.0.0  
## 
## loaded via a namespace (and not attached):
##  [1] codetools_0.2-10 colorspace_1.2-5 digest_0.6.8     evaluate_0.5.5  
##  [5] formatR_1.0      grid_3.1.2       gtable_0.1.2     htmltools_0.2.6 
##  [9] knitr_1.9        lattice_0.20-30  MASS_7.3-39      Matrix_1.1-5    
## [13] munsell_0.4.2    proto_0.3-10     Rcpp_0.11.4      reshape2_1.4.1  
## [17] rmarkdown_0.5.1  scales_0.2.4     splines_3.1.2    stringr_0.6.2   
## [21] tcltk_3.1.2      tools_3.1.2      yaml_2.1.13
```
