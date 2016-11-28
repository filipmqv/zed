# Raport - śledzie
FW  
`r format(Sys.time(), '%d %B, %Y')`  



# Wstęp

// TODO 
Ponadto raport powinien zaczynać się od rozdziału podsumowującego całą analizę, streszczającego najważniejsze spostrzeżenia analityka.

# Wstępne przetwarzanie danych

## Kod wyliczający wykorzystane biblioteki.

Wykorzystano następujące biblioteki: 


```r
library(dplyr)
library(tidyr)
library(ggplot2)
library(plotly)
library(zoo)
library(reshape2)
```


## Kod pozwalający wczytać dane z pliku.





Dane zawierają 52582 próbek (wierszy) oraz 16 atrybutów (kolumn).

## Kod zapewniający powtarzalność wyników przy każdym uruchomieniu raportu na tych samych danych.


## Kod przetwarzający brakujące dane.




## Sekcję podsumowującą rozmiar zbioru i podstawowe statystyki.

W poniższej tabeli przedstawiono kilka pierwszych wierszy z tabeli: 


```r
knitr::kable(head(herring))
```



  X   length     cfin1     cfin2     chel1      chel2     lcop1      lcop2    fbar     recr        cumf     totaln        sst        sal   xmonth   nao
---  -------  --------  --------  --------  ---------  --------  ---------  ------  -------  ----------  ---------  ---------  ---------  -------  ----
  1     22.5   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8
  2     25.0   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8
  3     25.5   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8
  4     24.0   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8
  5     22.0   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8
  6     24.0   0.02778   0.27785   2.46875   21.43548   2.54787   26.35881   0.356   482831   0.3059879   267380.8   14.30693   35.51234        7   2.8

Oznaczenia kolumn:

* X: numer badania (chronologicznie);
* length: długość złowionego śledzia [cm];
* cfin1: dostępność planktonu [zagęszczenie Calanus finmarchicus gat. 1];
* cfin2: dostępność planktonu [zagęszczenie Calanus finmarchicus gat. 2];
* chel1: dostępność planktonu [zagęszczenie Calanus helgolandicus gat. 1];
* chel2: dostępność planktonu [zagęszczenie Calanus helgolandicus gat. 2];
* lcop1: dostępność planktonu [zagęszczenie widłonogów gat. 1];
* lcop2: dostępność planktonu [zagęszczenie widłonogów gat. 2];
* fbar: natężenie połowów w regionie [ułamek pozostawionego narybku];
* recr: roczny narybek [liczba śledzi];
* cumf: łączne roczne natężenie połowów w regionie [ułamek pozostawionego narybku];
* totaln: łączna liczba ryb złowionych w ramach połowu [liczba śledzi];
* sst: temperatura przy powierzchni wody [°C];
* sal: poziom zasolenia wody [Knudsen ppt];
* xmonth: miesiąc połowu [numer miesiąca];
* nao: oscylacja północnoatlantycka [mb].

Poniżej przedstawiono statystyki poszczególnych kolumn:


```r
knitr::kable(summary(herring))
```

           X             length         cfin1             cfin2             chel1            chel2            lcop1              lcop2             fbar             recr              cumf             totaln             sst             sal            xmonth            nao         
---  --------------  -------------  ----------------  ----------------  ---------------  ---------------  -----------------  ---------------  ---------------  ----------------  ----------------  ----------------  --------------  --------------  ---------------  -----------------
     Min.   :    1   Min.   :19.0   Min.   : 0.0000   Min.   : 0.0000   Min.   : 0.000   Min.   : 5.238   Min.   :  0.3074   Min.   : 7.849   Min.   :0.0680   Min.   : 140515   Min.   :0.06833   Min.   : 144137   Min.   :12.77   Min.   :35.40   Min.   : 1.000   Min.   :-4.89000 
     1st Qu.:13146   1st Qu.:24.0   1st Qu.: 0.0000   1st Qu.: 0.2778   1st Qu.: 2.469   1st Qu.:13.427   1st Qu.:  2.5479   1st Qu.:17.808   1st Qu.:0.2270   1st Qu.: 360061   1st Qu.:0.14809   1st Qu.: 306068   1st Qu.:13.60   1st Qu.:35.51   1st Qu.: 5.000   1st Qu.:-1.89000 
     Median :26291   Median :25.5   Median : 0.1111   Median : 0.7012   Median : 5.750   Median :21.435   Median :  7.0000   Median :24.859   Median :0.3320   Median : 421391   Median :0.23191   Median : 539558   Median :13.86   Median :35.51   Median : 8.000   Median : 0.20000 
     Mean   :26291   Mean   :25.3   Mean   : 0.4463   Mean   : 2.0255   Mean   :10.004   Mean   :21.218   Mean   : 12.8029   Mean   :28.423   Mean   :0.3304   Mean   : 520367   Mean   :0.22981   Mean   : 514978   Mean   :13.87   Mean   :35.51   Mean   : 7.258   Mean   :-0.09241 
     3rd Qu.:39436   3rd Qu.:26.5   3rd Qu.: 0.3333   3rd Qu.: 1.7936   3rd Qu.:11.500   3rd Qu.:27.193   3rd Qu.: 21.2315   3rd Qu.:37.232   3rd Qu.:0.4560   3rd Qu.: 724151   3rd Qu.:0.29803   3rd Qu.: 730351   3rd Qu.:14.16   3rd Qu.:35.52   3rd Qu.: 9.000   3rd Qu.: 1.63000 
     Max.   :52581   Max.   :32.5   Max.   :37.6667   Max.   :19.3958   Max.   :75.000   Max.   :57.706   Max.   :115.5833   Max.   :68.736   Max.   :0.8490   Max.   :1565890   Max.   :0.39801   Max.   :1015595   Max.   :14.73   Max.   :35.61   Max.   :12.000   Max.   : 5.08000 

# Analiza danych

## Szczegółową analizę wartości atrybutów (np. poprzez prezentację rozkładów wartości).


```r
ggplot(herring, aes(x=X, y=length)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/length(X)-1.png)<!-- -->


```r
ggplot(herring, aes(x=X, y=length)) + geom_point() + geom_smooth() + theme_bw() + ylim(min(herring$length),max(herring$length)) + facet_wrap(~xmonth)
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/length(X)_by_months-1.png)<!-- -->


```r
ggplot(herring, aes(x=X, y=totaln)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/totaln(X)-1.png)<!-- -->


```r
ggplot(herring, aes(x=X, y=recr)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/recr(X)-1.png)<!-- -->


```r
food <- herring %>% gather(type, amount, cfin1:lcop2)
ggplot(food, aes(x = X, y = amount, colour = type)) + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/food(X)-1.png)<!-- -->

Natężenie połowów w trakcie pomiaru oraz łączne roczne natężęnie połowów:


```r
fbar_cumf <- herring %>% gather(type, amount, c(fbar, cumf))
ggplot(fbar_cumf, aes(x = X, y = amount, colour = type)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/fbar+cumf(X)-1.png)<!-- -->





Inne czynniki środowiskowe:


```r
ggplot(herring, aes(x=X, y=sst)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/sst(X)-1.png)<!-- -->


```r
ggplot(herring, aes(x=X, y=sal)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/sal(X)-1.png)<!-- -->


```r
ggplot(herring, aes(x=X, y=nao)) + geom_point() + geom_smooth() + theme_bw()
```

```
## `geom_smooth()` using method = 'gam'
```

![](Raport-sledzie_files/figure-html/nao(X)-1.png)<!-- -->

## Sekcję sprawdzającą korelacje między zmiennymi; sekcja ta powinna zawierać jakąś formę graficznej prezentacji korelacji.


```r
# get correlation matrix
cor_matrix <- round(cor(herring, method="pearson"), 2)
# get lower triangle
cor_matrix[lower.tri(cor_matrix)] <- NA
# use reshape2 library to melt matrix for plotting
melted_cor_matrix <- melt(cor_matrix, na.rm = TRUE)
ggplot(data = melted_cor_matrix, aes(x=Var2, y=Var1, fill=value)) + geom_tile() + scale_fill_gradient2(low = "red", high = "green", mid = "gray", midpoint = 0, limit = c(-1,1), space = "Lab") + geom_text(aes(Var2, Var1, label = value), color = "black", size = 4) + theme_bw()
```

![](Raport-sledzie_files/figure-html/correlation-1.png)<!-- -->



## Interaktywny wykres lub animację prezentującą zmianę rozmiaru śledzi w czasie.


# Regresor

## Sekcję próbującą stworzyć regresor przewidujący rozmiar śledzia (w tej sekcji należy wykorzystać wiedzę z pozostałych punktów oraz wykonać dodatkowe czynności, które mogą poprawić trafność predykcji); dobór parametrów modelu oraz oszacowanie jego skuteczności powinny zostać wykonane za pomocą techniki podziału zbioru na dane uczące, walidujące i testowe; trafność regresji powinna zostać oszacowana na podstawie miar R2 i RMSE.


## Analizę ważności atrybutów najlepszego znalezionego modelu regresji. Analiza ważności atrybutów powinna stanowić próbę odpowiedzi na pytanie: co sprawia, że rozmiar śledzi zaczął w pewnym momencie maleć.


## Jeśli analityk uzna to za stosowne, powyższe punkty mogę być wykonane w innej kolejności. Analityk nie musi, a nawet nie powinien, ograniczać się do powyższych punktów. Wszelkie dodatkowe techniki analizy danych, wizualizacje, spostrzeżenia będą pozytwnie wpływały na ocenę.
