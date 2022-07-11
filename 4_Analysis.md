``` r
library(tidyverse)
```

The code below demonstrates the last step of the research project–From
Sequence to Gene Expression. (previous steps available at our [Github
page](https://github.com/NickuFeng/Research-Project)) **This script was
originally written by our project mentor, professor Hae Kyung Im.** This
is a new version developed based on her work.

# Set up

We import two files we generated from the previous steps of the project.

``` r
CEU = read_csv(glue::glue("Data/Pre_Enf_CEU_correlation_table.csv"))
colnames(CEU)[1]<-'Gene'
YRI = read_csv(glue::glue("Data/Pre_Enf_YRI_correlation_table.csv"))
colnames(YRI)[1]<-'Gene'
library(knitr)
kable(CEU[1:5,],caption='A Glance at The CEU Table')
```

<table>
<caption>A Glance at The CEU Table</caption>
<thead>
<tr class="header">
<th style="text-align: left;">Gene</th>
<th style="text-align: right;">Predixcan</th>
<th style="text-align: right;">Enformer</th>
<th style="text-align: left;">strand</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">SERPINB1</td>
<td style="text-align: right;">0.4846456</td>
<td style="text-align: right;">-0.1337553</td>
<td style="text-align: left;">[‘-’]</td>
</tr>
<tr class="even">
<td style="text-align: left;">KBTBD2</td>
<td style="text-align: right;">0.1482625</td>
<td style="text-align: right;">-0.1406811</td>
<td style="text-align: left;">[‘-’]</td>
</tr>
<tr class="odd">
<td style="text-align: left;">GCDH</td>
<td style="text-align: right;">0.2177962</td>
<td style="text-align: right;">0.1975878</td>
<td style="text-align: left;">[‘+’]</td>
</tr>
<tr class="even">
<td style="text-align: left;">ZNF493</td>
<td style="text-align: right;">0.2374523</td>
<td style="text-align: right;">0.1055328</td>
<td style="text-align: left;">[‘+’]</td>
</tr>
<tr class="odd">
<td style="text-align: left;">MANBA</td>
<td style="text-align: right;">0.2165370</td>
<td style="text-align: right;">0.1174089</td>
<td style="text-align: left;">[‘-’]</td>
</tr>
</tbody>
</table>

A Glance at The CEU Table

For each table, the first column is the target gene, the second and
third columns are the correlation coefficients. They are the **model’s
prediction across sample group on the target gene** correlated with
**the true observed value from Geuvadis Data set**. The forth column is
the strand information of the gene.

# Analysis

## Enformer: CEU vs YRI

We first take a look at how does Enformer performs across population.

``` r
plot(CEU$Enformer,YRI$Enformer); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-3-1.png)

``` r
summary(CEU$Enformer)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
    ## -0.44002 -0.10597  0.05238  0.02481  0.13789  0.44571        1

``` r
summary(YRI$Enformer)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
    ## -0.29366 -0.06436  0.04685  0.05442  0.19410  0.44971        1

It looks like Enformer has a steady performance regardless the input’s
origin ancestry.

## PrediXcan: CEU vs YRI

Then we check on Predixcan’s performance across population.

``` r
plot(CEU$Predixcan,YRI$Predixcan); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-4-1.png)

``` r
summary(CEU$Predixcan)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -0.22731  0.04063  0.15713  0.17553  0.29781  0.75288

``` r
summary(YRI$Predixcan)
```

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -0.207979  0.001836  0.126608  0.127161  0.246127  0.625439

Looking at the plot and the summary, we notice Predixcan generally make
better predictions when its input data has an European ancestry. It
loses some accuracy when its input is non-European, which in this case
is African ancestry.

## Histogram of each variable

We make some histograms to gain some understanding about how is the data
distributed.

``` r
par(mfrow=c(2,2))
rango=c(-1,1)
hist(CEU$Enformer,xlim=rango)
hist(YRI$Enformer,xlim=rango)
hist(CEU$Predixcan,xlim=rango)
hist(YRI$Predixcan,xlim=rango)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-5-1.png)

``` r
par(mfrow=c(1,1)) #reset
```

## CEU: Enformer vs. Predixcan

We compare two model’s performance when predicting the group with
European ancestry

``` r
plot(CEU$Enformer,CEU$Predixcan); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-6-1.png)

## YRI: Enformer vs. Predixcan

We compare two model’s performance when predicting the group with
African ancestry

``` r
plot(YRI$Enformer,YRI$Predixcan); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-7-1.png)

We notice there is a sign-flip issue occurring with Enformer, but we
decide to ignore the problem for now. Instead, we will make our
comparison with either absolute value or the squared value of the
corr.coef.

## CEU: Enformer vs. Predixcan

``` r
plot(abs(CEU$Enformer),abs(CEU$Predixcan)); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-8-1.png)

## YRI: Enformer vs. Predixcan

``` r
plot(abs(YRI$Enformer),abs(YRI$Predixcan)); abline(0,1)
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-9-1.png)

# Prelimary Findings

## 1. PrediXcan outperforms Enformer in BOTH EUR and YRI

``` r
## CEU: Enformer vs. Predixcan
qqplot((CEU$Enformer)^2,(CEU$Predixcan)^2); abline(0,1)
title("PrediXcan outperforms Enformer in CEU")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-10-1.png)

``` r
## YRI: Enformer vs. Predixcan
qqplot((YRI$Enformer)^2,(YRI$Predixcan)^2); abline(0,1)
title("PrediXcan outperforms Enformer in YRI")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-11-1.png)

Looking at these two plots, it is clear that PrediXcan outperforms
Enformer in **BOTH** European and African ancestry sample groups.

## 2. PrediXcan performance in YRI worse than in EUR

``` r
## PrediXcan: CEU vs YRI
qqplot((CEU$Predixcan)^2,(YRI$Predixcan)^2);abline(0,1)
title("PrediXcan performance in YRI worse than in CEU")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-12-1.png)

However, PrediXcan suffers a lost in prediction accuracy when its input
data comes from a non-European individual.

## 3. Enformer’s performance does not change between EUR and YRI

``` r
## Enformer: CEU vs YRI
qqplot((CEU$Enformer)^2,(YRI$Enformer)^2);abline(0,1)
title("Enformer's performance does not change between EUR and YRI")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Enformer’s performance does not change between EUR and YRI. This is a
exciting finding because it implies that Enformer’s algorithm has the
potential to overcome PrediXcan’s limitations, which is a model trained
mainly on data with European ancestry.

## 4. Relative performance: Portability of Enformer better than Predixcan

``` r
epsi=0.1
portability_Enformer = ( abs(YRI$Enformer) + epsi )  / ( abs(CEU$Enformer)  + epsi )
portability_Predixcan = ( abs(YRI$Predixcan) + epsi )/ ( abs(CEU$Predixcan) + epsi )
 
qqplot(portability_Enformer,portability_Predixcan);abline(0,1)
title("Portability of Enformer better than Predixcan")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-14-1.png)

``` r
summary(portability_Enformer)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##  0.2691  0.6810  0.9368  1.0966  1.2542  3.6703       1

``` r
summary(portability_Predixcan)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.2293  0.5711  0.9375  0.9813  1.2809  3.0142

Following the thread of the previous finding, we also want to test the
portability of the two models. We found that Enformer has a better
portability then PrediXcan does.

## 5. Violin Plot

``` r
tibble(Enformer=portability_Enformer,Predixcan=portability_Predixcan )  %>% pivot_longer(cols = c(Enformer,Predixcan),names_to="type",values_to="portability") %>% ggplot(aes(type,portability)) + geom_violin() + geom_boxplot(width=0.3) + geom_jitter(size=2,col='gray') + theme_bw(base_size = 15) + ggtitle("Enformer Portability to YRI Higher than Predixcan")
```

![](4_Analysis_files/figure-markdown_strict/unnamed-chunk-17-1.png)

This Violin Plot further proves the finding \#4.
