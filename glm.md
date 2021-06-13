---
title: 'dtc: GLM in R'
author: "Andi"
date: "Last updated: 13 June, 2021"
output: 
  html_document: 
    keep_md: yes
---






```r
#```{r, echo = FALSE, results='hide'}
# if we used both 'echo=TRUE' and 'results=hide' the pipe would not work properly
# if we used 'echo = FALSE' and 'results=hide' we would have only messages (i.e. attaching package) If we don't want them we set 'error = FALSE', 'warning = FALSE', and 'message = FALSE'.
```

## Refresher on fitting linear models




```r
# Fit a lm()
lm(formula = weight ~ Diet, data = chick_weight_end)
```

```
## 
## Call:
## lm(formula = weight ~ Diet, data = chick_weight_end)
## 
## Coefficients:
## (Intercept)        Diet2        Diet3        Diet4  
##      177.75        36.95        92.55        60.81
```

```r
# Fit a glm()
glm(formula = weight ~ Diet , data = chick_weight_end, family = 'gaussian')
```

```
## 
## Call:  glm(formula = weight ~ Diet, family = "gaussian", data = chick_weight_end)
## 
## Coefficients:
## (Intercept)        Diet2        Diet3        Diet4  
##      177.75        36.95        92.55        60.81  
## 
## Degrees of Freedom: 44 Total (i.e. Null);  41 Residual
## Null Deviance:	    225000 
## Residual Deviance: 167800 	AIC: 507.8
```

## Fitting a Poisson regression in R




```r
# fit y predicted by x with data.frame dat using the poisson family
poisson_out <- glm(count ~ time, family = "poisson", data = dat)

# print the output
summary(poisson_out)
```

```
## 
## Call:
## glm(formula = count ~ time, family = "poisson", data = dat)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.6547  -0.9666  -0.7226   0.3830   2.3022  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)  
## (Intercept) -1.43036    0.59004  -2.424   0.0153 *
## time         0.05815    0.02779   2.093   0.0364 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 35.627  on 29  degrees of freedom
## Residual deviance: 30.918  on 28  degrees of freedom
## AIC: 66.024
## 
## Number of Fisher Scoring iterations: 5
```

## Comparing linear and Poisson regression


```r
# Fit a glm with count predicted by time using data.frame dat and gaussian family
lm_out <- glm(count ~ time, data = dat, family = "gaussian")

summary(lm_out)
```

```
## 
## Call:
## glm(formula = count ~ time, family = "gaussian", data = dat)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.2022  -0.5190  -0.1497   0.2595   3.0194  
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept)  0.09425    0.32891   0.287   0.7766  
## time         0.03693    0.01853   1.993   0.0561 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7714815)
## 
##     Null deviance: 24.667  on 29  degrees of freedom
## Residual deviance: 21.601  on 28  degrees of freedom
## AIC: 81.283
## 
## Number of Fisher Scoring iterations: 2
```

```r
summary(poisson_out)
```

```
## 
## Call:
## glm(formula = count ~ time, family = "poisson", data = dat)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.6547  -0.9666  -0.7226   0.3830   2.3022  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)  
## (Intercept) -1.43036    0.59004  -2.424   0.0153 *
## time         0.05815    0.02779   2.093   0.0364 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 35.627  on 29  degrees of freedom
## Residual deviance: 30.918  on 28  degrees of freedom
## AIC: 66.024
## 
## Number of Fisher Scoring iterations: 5
```

## Intercepts-comparisons versus means




```r
# Fit a glm() that estimates the difference between players
summary(glm(goal ~ player, data = scores, family = 'poisson'))
```

```
## 
## Call:
## glm(formula = goal ~ player, family = "poisson", data = scores)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.0000  -0.6325  -0.6325   0.4934   1.2724  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)  
## (Intercept)  -1.6094     0.9999  -1.610   0.1075  
## playerSam     2.3026     1.0487   2.196   0.0281 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 18.3578  on 9  degrees of freedom
## Residual deviance:  9.8105  on 8  degrees of freedom
## AIC: 26.682
## 
## Number of Fisher Scoring iterations: 5
```

```r
# Fit a glm() that estimates an intercept for each player 
summary(glm(goal ~ player - 1, data = scores, family = 'poisson'))
```

```
## 
## Call:
## glm(formula = goal ~ player - 1, family = "poisson", data = scores)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.0000  -0.6325  -0.6325   0.4934   1.2724  
## 
## Coefficients:
##           Estimate Std. Error z value Pr(>|z|)  
## playerLou  -1.6094     0.9999  -1.610   0.1075  
## playerSam   0.6931     0.3162   2.192   0.0284 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 18.4546  on 10  degrees of freedom
## Residual deviance:  9.8105  on  8  degrees of freedom
## AIC: 26.682
## 
## Number of Fisher Scoring iterations: 5
```

Note that the intercept from model 1, (Intercept), is the same as Lou's coefficient from model 2, playerLou. R allows for two different types of intercepts to be estimated. The first and default setting estimated the number of goals scored by Sam and the difference in goals scored by Lou. The second setting, using - 1, estimated the number of goals scored by Sam AND the number of goals scored by Lou. More generally, the default formula in R uses the first factor level and contrasts all other levels to it. Reording a factor allows you to change the reference level.

## Applying summary(), print(), and tidy() to glm




```r
# Build your linear and Poisson regression models
lm_out <- lm(Number ~ Month, data = dat)
poisson_out <- glm(Number ~ Month, data = dat, family = 'poisson')

# Examine the outputs using print()
print(lm_out)
```

```
## 
## Call:
## lm(formula = Number ~ Month, data = dat)
## 
## Coefficients:
## (Intercept)       Month2       Month3       Month4       Month5       Month6  
##    0.129477    -0.038031    -0.078401    -0.057254    -0.032702    -0.043365  
##      Month7       Month8       Month9      Month10      Month11      Month12  
##   -0.005821    -0.051520    -0.023921    -0.054208    -0.023921    -0.022919
```

```r
print(poisson_out)
```

```
## 
## Call:  glm(formula = Number ~ Month, family = "poisson", data = dat)
## 
## Coefficients:
## (Intercept)       Month2       Month3       Month4       Month5       Month6  
##     -2.0443      -0.3478      -0.9302      -0.5838      -0.2911      -0.4079  
##      Month7       Month8       Month9      Month10      Month11      Month12  
##     -0.0460      -0.5073      -0.2043      -0.5424      -0.2043      -0.1948  
## 
## Degrees of Freedom: 4367 Total (i.e. Null);  4356 Residual
## Null Deviance:	    2325 
## Residual Deviance: 2303 	AIC: 2976
```

```r
# Examine the outputs using summary()
summary(lm_out)
```

```
## 
## Call:
## lm(formula = Number ~ Month, data = dat)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.1295 -0.1056 -0.0914 -0.0753  8.8763 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  0.129477   0.022770   5.686 1.38e-08 ***
## Month2      -0.038031   0.032767  -1.161   0.2458    
## Month3      -0.078401   0.032007  -2.450   0.0143 *  
## Month4      -0.057254   0.032269  -1.774   0.0761 .  
## Month5      -0.032702   0.032007  -1.022   0.3070    
## Month6      -0.043365   0.032269  -1.344   0.1791    
## Month7      -0.005821   0.032007  -0.182   0.8557    
## Month8      -0.051520   0.032007  -1.610   0.1075    
## Month9      -0.023921   0.032269  -0.741   0.4586    
## Month10     -0.054208   0.032007  -1.694   0.0904 .  
## Month11     -0.023921   0.032269  -0.741   0.4586    
## Month12     -0.022919   0.032136  -0.713   0.4758    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4338 on 4356 degrees of freedom
## Multiple R-squared:  0.00249,	Adjusted R-squared:  -2.927e-05 
## F-statistic: 0.9884 on 11 and 4356 DF,  p-value: 0.4542
```

```r
summary(poisson_out)
```

```
## 
## Call:
## glm(formula = Number ~ Month, family = "poisson", data = dat)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -0.5089  -0.4595  -0.4277  -0.3880   7.7086  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)  -2.0443     0.1459 -14.015  < 2e-16 ***
## Month2       -0.3478     0.2314  -1.503 0.132839    
## Month3       -0.9302     0.2719  -3.422 0.000623 ***
## Month4       -0.5837     0.2444  -2.388 0.016923 *  
## Month5       -0.2911     0.2215  -1.314 0.188706    
## Month6       -0.4079     0.2314  -1.763 0.077939 .  
## Month7       -0.0460     0.2074  -0.222 0.824486    
## Month8       -0.5073     0.2361  -2.149 0.031671 *  
## Month9       -0.2043     0.2182  -0.936 0.349112    
## Month10      -0.5424     0.2387  -2.272 0.023075 *  
## Month11      -0.2043     0.2182  -0.936 0.349112    
## Month12      -0.1948     0.2166  -0.899 0.368434    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 2325.3  on 4367  degrees of freedom
## Residual deviance: 2302.7  on 4356  degrees of freedom
## AIC: 2975.6
## 
## Number of Fisher Scoring iterations: 6
```

```r
# Examine the outputs using tidy()
library(broom)
tidy(lm_out)
```

```
## # A tibble: 12 x 5
##    term        estimate std.error statistic      p.value
##    <chr>          <dbl>     <dbl>     <dbl>        <dbl>
##  1 (Intercept)  0.129      0.0228     5.69  0.0000000138
##  2 Month2      -0.0380     0.0328    -1.16  0.246       
##  3 Month3      -0.0784     0.0320    -2.45  0.0143      
##  4 Month4      -0.0573     0.0323    -1.77  0.0761      
##  5 Month5      -0.0327     0.0320    -1.02  0.307       
##  6 Month6      -0.0434     0.0323    -1.34  0.179       
##  7 Month7      -0.00582    0.0320    -0.182 0.856       
##  8 Month8      -0.0515     0.0320    -1.61  0.108       
##  9 Month9      -0.0239     0.0323    -0.741 0.459       
## 10 Month10     -0.0542     0.0320    -1.69  0.0904      
## 11 Month11     -0.0239     0.0323    -0.741 0.459       
## 12 Month12     -0.0229     0.0321    -0.713 0.476
```

```r
tidy(poisson_out)
```

```
## # A tibble: 12 x 5
##    term        estimate std.error statistic  p.value
##    <chr>          <dbl>     <dbl>     <dbl>    <dbl>
##  1 (Intercept)  -2.04       0.146   -14.0   1.27e-44
##  2 Month2       -0.348      0.231    -1.50  1.33e- 1
##  3 Month3       -0.930      0.272    -3.42  6.23e- 4
##  4 Month4       -0.584      0.244    -2.39  1.69e- 2
##  5 Month5       -0.291      0.221    -1.31  1.89e- 1
##  6 Month6       -0.408      0.231    -1.76  7.79e- 2
##  7 Month7       -0.0460     0.207    -0.222 8.24e- 1
##  8 Month8       -0.507      0.236    -2.15  3.17e- 2
##  9 Month9       -0.204      0.218    -0.936 3.49e- 1
## 10 Month10      -0.542      0.239    -2.27  2.31e- 2
## 11 Month11      -0.204      0.218    -0.936 3.49e- 1
## 12 Month12      -0.195      0.217    -0.899 3.68e- 1
```

## Extracting coefficients from glm()


```r
# Extract the regression coefficients
coef(poisson_out)
```

```
## (Intercept)      Month2      Month3      Month4      Month5      Month6 
## -2.04425523 -0.34775767 -0.93019964 -0.58375226 -0.29111968 -0.40786159 
##      Month7      Month8      Month9     Month10     Month11     Month12 
## -0.04599723 -0.50734279 -0.20426264 -0.54243411 -0.20426264 -0.19481645
```

```r
# Extract the confidence intervals
confint(poisson_out)
```

```
## Waiting for profiling to be done...
```

```
##                  2.5 %      97.5 %
## (Intercept) -2.3444432 -1.77136313
## Month2      -0.8103027  0.10063404
## Month3      -1.4866061 -0.41424128
## Month4      -1.0762364 -0.11342457
## Month5      -0.7311289  0.14051326
## Month6      -0.8704066  0.04053012
## Month7      -0.4542037  0.36161360
## Month8      -0.9807831 -0.05092540
## Month9      -0.6367321  0.22171492
## Month10     -1.0218277 -0.08165226
## Month11     -0.6367321  0.22171492
## Month12     -0.6237730  0.22851779
```

## Predicting with glm()





```r
# print the new input months
print(new_dat)
```

```
##   Month
## 1     6
## 2     7
## 3     8
```

```r
# use the model to predict with new data 
pred_out <- predict(object = poisson_out, newdata = new_dat, type = "response")

# print the predictions
print(pred_out)
```

```
##          1          2          3 
## 0.08611111 0.12365591 0.07795699
```

