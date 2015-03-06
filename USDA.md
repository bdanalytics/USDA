# USDA: <Predicted attribute> regression
bdanalytics  

**  **    
**Date: (Fri) Mar 06, 2015**    

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

#print(summary(entity_df))
#pairs(subset(entity_df, select=-c(col_symbol)))

# Create new features that help diagnostics
#   Convert factors to dummy variables
#   Build splines   require(splines); bsBasis <- bs(training$age, df=3)
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
## [1] sos_1.3-8       brew_1.0-6      doBy_4.5-13     survival_2.38-1
## [5] ggplot2_1.0.0  
## 
## loaded via a namespace (and not attached):
##  [1] codetools_0.2-10 colorspace_1.2-5 digest_0.6.8     evaluate_0.5.5  
##  [5] formatR_1.0      grid_3.1.2       gtable_0.1.2     htmltools_0.2.6 
##  [9] knitr_1.9        lattice_0.20-30  MASS_7.3-39      Matrix_1.1-5    
## [13] munsell_0.4.2    plyr_1.8.1       proto_0.3-10     Rcpp_0.11.4     
## [17] reshape2_1.4.1   rmarkdown_0.5.1  scales_0.2.4     splines_3.1.2   
## [21] stringr_0.6.2    tcltk_3.1.2      tools_3.1.2      yaml_2.1.13
```
