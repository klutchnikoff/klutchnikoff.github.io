Le package `dplyr`
================
Nicolas Klutchnikoff
02 février 2017

Introduction
============

Le package `dplyr` est un package qui permet la manipulation des données de façon simple et rapide. Il définit une **grammaire** pour interagir avec les données. Il est basé sur une *variante* des `data.frame` appelée `tibble`. Nous avons déjà rencontré des `tibble`lors de l'utilisation du package `readr`. En effet la commande `read_delim()`et ses dérivées créent des objets dont la classe est `tbl`. Regardons un exemple simple :

``` r
library(dplyr)
library(readr)
```

``` r
path <- file.path(dirname(getwd()), "data", "piscines.csv")
piscines <- read_csv(path)
```

    ## Parsed with column specification:
    ## cols(
    ##   Name = col_character(),
    ##   Address = col_character(),
    ##   Latitude = col_double(),
    ##   Longitude = col_double()
    ## )

``` r
class(piscines)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

Nous reviendrons plus tard précisément sur la signification précise des classes `tbl` et `tbl_df`. On remarque tout de même que la variable `piscines`est un `data.frame`. Toutes les manipulations classiques sur les `data.frame`sont donc possibles. Comme d'habitude, on suppose que le `tibble` sur lequel on travaille est formaté de sorte que les colonnes représentent des variables et que les lignes représentent des individus[1].

Visualiser les `tibble`
=======================

Les commandes usuelles pour visualiser les `data.frame`fonctionnent. Elles donnent toutefois des résultats légèrement différents sur les `tibble` comme on peut d-le vérifier simplement dans la console de `R` (dans le notebook les résultats sont ne sont pas les mêmes que dans la console). Voici les commandes de base :

``` r
head(piscines, n=5)
```

    ## # A tibble: 5 × 4
    ##                          Name                                    Address
    ##                         <chr>                                      <chr>
    ## 1 Acacia Ridge Leisure Centre         1391 Beaudesert Road, Acacia Ridge
    ## 2             Bellbowrie Pool               Sugarwood Street, Bellbowrie
    ## 3                 Carole Park Cnr Boundary Road and Waterford Road Wacol
    ## 4 Centenary Pool (inner City)           400 Gregory Terrace, Spring Hill
    ## 5              Chermside Pool               375 Hamilton Road, Chermside
    ## # ... with 2 more variables: Latitude <dbl>, Longitude <dbl>

``` r
glimpse(piscines)
```

    ## Observations: 20
    ## Variables: 4
    ## $ Name      <chr> "Acacia Ridge Leisure Centre", "Bellbowrie Pool", "C...
    ## $ Address   <chr> "1391 Beaudesert Road, Acacia Ridge", "Sugarwood Str...
    ## $ Latitude  <dbl> -27.58616, -27.56547, -27.60744, -27.45537, -27.3858...
    ## $ Longitude <dbl> 153.0264, 152.8911, 152.9315, 153.0251, 153.0351, 15...

Notons que pour les `tibble`on préfère utiliser la commande `glimpse()`à la commande `str()` qui permet de décrire la structure des `data.frame`. Mais on peut tout de même utiliser cette fonction :

``` r
str(piscines)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    20 obs. of  4 variables:
    ##  $ Name     : chr  "Acacia Ridge Leisure Centre" "Bellbowrie Pool" "Carole Park" "Centenary Pool (inner City)" ...
    ##  $ Address  : chr  "1391 Beaudesert Road, Acacia Ridge" "Sugarwood Street, Bellbowrie" "Cnr Boundary Road and Waterford Road Wacol" "400 Gregory Terrace, Spring Hill" ...
    ##  $ Latitude : num  -27.6 -27.6 -27.6 -27.5 -27.4 ...
    ##  $ Longitude: num  153 153 153 153 153 ...
    ##  - attr(*, "spec")=List of 2
    ##   ..$ cols   :List of 4
    ##   .. ..$ Name     : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_character" "collector"
    ##   .. ..$ Address  : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_character" "collector"
    ##   .. ..$ Latitude : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ Longitude: list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   ..$ default: list()
    ##   .. ..- attr(*, "class")= chr  "collector_guess" "collector"
    ##   ..- attr(*, "class")= chr "col_spec"

On peut également utiliser la fonction `summary()`.

``` r
summary(piscines)
```

    ##      Name             Address             Latitude        Longitude    
    ##  Length:20          Length:20          Min.   :-27.61   Min.   :152.9  
    ##  Class :character   Class :character   1st Qu.:-27.55   1st Qu.:153.0  
    ##  Mode  :character   Mode  :character   Median :-27.49   Median :153.0  
    ##                                        Mean   :-27.49   Mean   :153.0  
    ##                                        3rd Qu.:-27.45   3rd Qu.:153.1  
    ##                                        Max.   :-27.31   Max.   :153.2

Les verbes de la grammaire `dplyr`
==================================

La grammaire de *base* `dplyr` est basée sur cinq **verbes**[2] qui permettent la manipulation des données. En voici une description rapide :

1.  `select()` permet d'extraire des variables (colonnes) d'un `tibble`.
2.  `mutate()` permet d'ajouter une variable à un `tibble`.
3.  `filter()` permet de sélectionner des individus (lignes) qui vérifient certains critères.
4.  `arrange()` permet permuter les individus pour présenter un `tibble`d'une manière différente.
5.  `summarise()` permet d'extraire des informations contenues dans un `tibble`.

Dans la suite de cette section nous allons voir de façon plus précise comment utiliser ces verbes.

Le verbe `select()`
-------------------

[1] On pourra utiliser les packages `tidyr` et `reshape2` si les données recueillis ne sont pas dans ce format. On en parlera éventuellement un peu plus loin.

[2] Ce sont des fonctions qui ont une syntaxe particulière
