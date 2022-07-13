The code below demonstrates the last step of the research project–From
Sequence to Gene Expression. (previous steps available at our [Github
page](https://github.com/NickuFeng/Research-Project)) **This script was
originally written by our project mentor, professor Hae Kyung Im.** This
is a new version developed based on her work.

# Set up

We import two files we generated from the previous steps of the project.

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

## Histogram of each variable

We make some histograms to gain some understanding about how is the data
distributed.

<p align="center">
<img src="/Data/Images/Histogram-1.png" style="height: 450; width:450;"> 
</p>

## Evaluating Model’s Performance

<p align="center">
<img src="/Data/Images/Model Performance-1.png" style="height: 450; width:450;"> 
</p>


### Enformer: CEU vs YRI

We first take a look at how does Enformer performs across population
(upper left), and it looks like Enformer has a steady performance
regardless the input’s origin ancestry because the dots are
approximately equally distributed around the identity line.

    ## [1] "The summary of PrediXcan on CEU group"

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
    ## -0.44002 -0.10597  0.05238  0.02481  0.13789  0.44571        1

    ## [2] "The summary of PrediXcan on YRI group"

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
    ## -0.29366 -0.06436  0.04685  0.05442  0.19410  0.44971        1

### PrediXcan: CEU vs YRI

Then we check on PrediXcan’s performance across population (upper
right). Combining the plot and the summary, we notice PrediXcan
generally make better predictions when its input data has an European
ancestry because there are more dots located below the identity line. It
loses some accuracy when its input is non-European, which in this case
is African ancestry.

    ## [1] "The summary of PrediXcan on CEU group"

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -0.22731  0.04063  0.15713  0.17553  0.29781  0.75288

    ## [2] "The summary of PrediXcan on YRI group"

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -0.207979  0.001836  0.126608  0.127161  0.246127  0.625439

### Enformer vs. PrediXcan on CEU and YRI group\*

Then, we compare two model’s performance when making predictions on the
group with European ancestry and the group with African ancestry (The
bottom two graphs).

*Enformer vs. PrediXcan on CEU and YRI group Extended*

We notice there is a sign-flip issue occurring with Enformer, but we
decide to ignore the problem for now. Instead, we will make our
comparison with either absolute value or the squared value of the
corr.coef.

<p align="center">
<img src="/Data/Images/Absolute_Enf_YRI-1.png" style="height: 450; width:450;"> 
</p>


# Prelimary Findings

## 1. PrediXcan outperforms Enformer in BOTH EUR and YRI

``` r
## CEU: Enformer vs. Predixcan
qqplot((CEU$Enformer)^2,(CEU$Predixcan)^2, xlab = 'The squared value of Enformer in CEU', ylab = 'The squared value of PrediXcan in CEU', frame=FALSE); abline(0,1)
title("PrediXcan outperforms Enformer in CEU")
```

<p align="center">
<img src="/Data/Images/Squared_CEU-1.png" style="height: 450; width:450;"> 
</p>

``` r
## YRI: Enformer vs. Predixcan
qqplot((YRI$Enformer)^2,(YRI$Predixcan)^2, xlab = 'The squared value of Enformer in YRI', ylab = 'The squared value of PrediXcan in YRI'); abline(0,1)
title("PrediXcan outperforms Enformer in YRI")
```

<p align="center">
<img src="/Data/Images/Squared_YRI-1.png" style="height: 450; width:450;"> 
</p>

Looking at these two plots, it is clear that PrediXcan outperforms
Enformer in **BOTH** European and African ancestry sample groups.

## 2. PrediXcan performance in YRI worse than in EUR

``` r
## PrediXcan: CEU vs YRI
qqplot((CEU$Predixcan)^2,(YRI$Predixcan)^2, xlab = 'The squared value of PrediXcan in CEU', ylab = 'The squared value of PrediXcan in YRI');abline(0,1)
title("PrediXcan performance in YRI worse than in CEU")
```

<p align="center">
<img src="/Data/Images/Squared_PrediXcan-1.png" style="height: 450; width:450;"> 
</p>

However, PrediXcan suffers a lost in prediction accuracy when its input
data comes from a non-European individual.

## 3. Enformer’s performance does not change between EUR and YRI

``` r
## Enformer: CEU vs YRI
qqplot((CEU$Enformer)^2,(YRI$Enformer)^2, xlab = 'The squared value of Enformer in CEU group', ylab = 'The squared value of Enformer in YRI group');abline(0,1)
title("Enformer's performance does not change between EUR and YRI")
```

<p align="center">
<img src="/Data/Images/Squared_Enformer-1.png" style="height: 450; width:450;"> 
</p>

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

<p align="center">
<img src="/Data/Images/Portability-1.png" style="height: 450; width:450;"> 
</p>

    ## [1] "portability of Enformer"

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##  0.2691  0.6810  0.9368  1.0966  1.2542  3.6703       1

    ## [2] "portability of PrediXcan"

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.2293  0.5711  0.9375  0.9813  1.2809  3.0142

Following the thread of the previous finding, we also want to test the
portability of the two models. We found that Enformer has a better
portability then PrediXcan does.

## 5. Violin Plot

``` r
tibble(Enformer=portability_Enformer,Predixcan=portability_Predixcan )  %>% pivot_longer(cols = c(Enformer,Predixcan),names_to="type",values_to="portability") %>% ggplot(aes(type,portability)) + geom_violin() + geom_boxplot(width=0.3) + geom_jitter(size=2,col='gray') + theme_bw(base_size = 15) + ggtitle("Enformer Portability to YRI Higher than Predixcan")
```
<p align="center">
<img src="/Data/Images/Violin-1.png" style="height: 450; width:450;"> 
</p>

This Violin Plot further proves the finding \#4.
