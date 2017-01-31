TP 1
================
Nicolas Klutchnikoff
10 janvier 2017

``` r
library(readr)
library(data.table)
```

Écrire des rapports de TP
-------------------------

L'objectif de ce premier TP est d'écrire un rapport sur l'utilisation des outils de base d'importation des données avec `R`.

Pour réaliser cet objectif, on va utiliser la syntaxe **RMarkdown** et le logiciel *RStudio*.

Avant de commencer, nous donnons les éléments de base de la syntaxe **RMarkdown** puis nous rentrerons dans le vif du sujet.

### L'entête

    ---
    title: "TP 1"
    author: "Nicolas Klutchnikoff"
    date: "10 janvier 2017"
    output: html_document
    ---

L'entête ci-dessus contient les informations de base sur le document :

-   Le titre ;
-   L'auteur ;
-   La date ;
-   Le type de sortie désiré (html, pdf, fichier *Word*, présentation).

### Les titres

On peut simplement sectionner le rapport à l'aide de titres avec différents niveaux. Par exemple :

    # Titre principal
    ## Sous-titre
    ### Sous-sous-titre

C'est très simple.

### Les commandes de mise en valeur

Il est possible de mettre en valeur certaines parties du texte :

-   `*italique*` pour la mise en *italique*,

-   `**gras**` pour la mise en **gras**.

-   `` `typewriter` `` pour les caractères de type machine à écrire `typewriter`.

### Les listes

Ci dessus on a fabriqué une liste en codant :

    * `*italique*` pour la mise en *italique*, 

    * `**gras**` pour la mise en **gras**. 

    * `` `typewriter` `` pour les caractères de type machine à écrire `typewriter`

Il ne faut pas oublier de sauter une ligne avant chaque entrée (ça fait partie de la syntaxe).

On peut aussi faire des listes énumérées :

    1. Toto

    2. Tata.

On obtient alors :

1.  Toto

2.  Tata.

### Les liens

On peut insérer des liens. Par exemple vers le logiciel [RStudio](https://www.rstudio.com) avec la syntaxe : `[RStudio](https://www.rstudio.com)`.

### Les maths

Pour terminer avec les choses simples, on peut inclure des formules mathématiques en utilisant la syntaxe de LaTeX. Par exemple

    $$e^{i\pi} + 1 = 0$$

donne la célèbre formule :
*e*<sup>*i**π*</sup> + 1 = 0

### Du **R**

Enfin, **RMarkdown** est surtout utile pour insérer du code *R* à l'intérieur du rapport. Ce code sera exécuté. Donnons un exemple très simple :

``` r
x <- runif(100)
y <- 2*x+1 + rnorm(100, 0, 0.05)
plot(x,y)
```

![](tp-1_files/figure-markdown_github/unnamed-chunk-2-1.png)

Il y a beaucoup d'options d'affichage que nous ne détaillerons pas pour l'instant. Ni sans doute ultérieurement. On peut également exécuter d'autres langages que *R*, par exemple du *Python*, du *Bash*, etc.

### A garder sous la main

Pour se rappeler les éléments de syntaxe de **Rmarkdown**, le plus simple est de consulter ce [document](https://www.rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf). D'autres *antisèches* sont disponibles à l'adresse [suivante](https://www.rstudio.com/resources/cheatsheets/).

Importation des données (1)
---------------------------

### Introduction

Les données peuvent se présenter sous différents formats. Les formats qu'on trouve le plus souvent sont les suivants :

-   Fichiers `.csv`

-   Fichiers Excel `.xls` ou `.xlsx` pour les plus récents

-   Base de données relationnelles (MySQL)

-   Base de données NoSQL. Par exemple `.json`.

-   Fichiers provenant d'autres logiciels statistiques (SAS, STATA, etc.)

Dans ce premier TP, on va se concentrer sur les deux premiers formats, les fichiers `.csv` et leurs *dérivés* dans un premier temps puis les fichiers *Excel* dans un second temps. Les 3 fichiers sur lesquels nous allons travailler sont disponibles dans **Cursus**.

### Le packae de base `Utils`

#### La fonction `read.csv()`

Par défaut, *R* propose une fonction pour lire les fichiers `.csv`. La syntaxe est relativement simple :

``` r
path <- file.path("data", "piscines.csv")
piscines <- read.csv(path)
```

On peut afficher les données à l'aide de

``` r
head(piscines, n=10)
```

    ##                                         Name
    ## 1                Acacia Ridge Leisure Centre
    ## 2                            Bellbowrie Pool
    ## 3                                Carole Park
    ## 4                Centenary Pool (inner City)
    ## 5                             Chermside Pool
    ## 6                Colmslie Pool (Morningside)
    ## 7             Spring Hill Baths (inner City)
    ## 8                 Dunlop Park Pool (Corinda)
    ## 9                      Fortitude Valley Pool
    ## 10 Hibiscus Sports Complex (upper MtGravatt)
    ##                                       Address  Latitude Longitude
    ## 1          1391 Beaudesert Road, Acacia Ridge -27.58616  153.0264
    ## 2                Sugarwood Street, Bellbowrie -27.56547  152.8911
    ## 3  Cnr Boundary Road and Waterford Road Wacol -27.60744  152.9315
    ## 4            400 Gregory Terrace, Spring Hill -27.45537  153.0251
    ## 5                375 Hamilton Road, Chermside -27.38583  153.0351
    ## 6                400 Lytton Road, Morningside -27.45516  153.0789
    ## 7            14 Torrington Street, Springhill -27.45960  153.0215
    ## 8                     794 Oxley Road, Corinda -27.54652  152.9806
    ## 9        432 Wickham Street, Fortitude Valley -27.45390  153.0368
    ## 10        90 Klumpp Road, Upper Mount Gravatt -27.55183  153.0735

La variable `piscines` obtenue est un `data.frame` comme on peut s'en rendre compte simplement :

``` r
class(piscines)
```

    ## [1] "data.frame"

Regardons de plus près la variable `Name` (on remarque au passage que le fichier originel contenait, à la première ligne, le nom des variables. *read.csv()* a donc récupéré ces noms).

``` r
print(piscines$Name)
```

    ##  [1] Acacia Ridge Leisure Centre              
    ##  [2] Bellbowrie Pool                          
    ##  [3] Carole Park                              
    ##  [4] Centenary Pool (inner City)              
    ##  [5] Chermside Pool                           
    ##  [6] Colmslie Pool (Morningside)              
    ##  [7] Spring Hill Baths (inner City)           
    ##  [8] Dunlop Park Pool (Corinda)               
    ##  [9] Fortitude Valley Pool                    
    ## [10] Hibiscus Sports Complex (upper MtGravatt)
    ## [11] Ithaca Pool ( Paddington)                
    ## [12] Jindalee Pool                            
    ## [13] Manly Pool                               
    ## [14] Mt Gravatt East Aquatic Centre           
    ## [15] Musgrave Park Pool (South Brisbane)      
    ## [16] Newmarket Pool                           
    ## [17] Runcorn Pool                             
    ## [18] Sandgate Pool                            
    ## [19] Langlands Parks Pool (Stones Corner)     
    ## [20] Yeronga Park Pool                        
    ## 20 Levels: Acacia Ridge Leisure Centre Bellbowrie Pool ... Yeronga Park Pool

On remarque que cette variable est considérée comme un facteur. Lors de l'importation les chaînes de caractères sont traitées par défaut comme des facteurs. Ici, ce n'est **pas** souhaitable.

L'importation correcte est donc

``` r
piscines_OK <- read.csv(path, stringsAsFactors = FALSE)
print(piscines_OK$Name)
```

    ##  [1] "Acacia Ridge Leisure Centre"              
    ##  [2] "Bellbowrie Pool"                          
    ##  [3] "Carole Park"                              
    ##  [4] "Centenary Pool (inner City)"              
    ##  [5] "Chermside Pool"                           
    ##  [6] "Colmslie Pool (Morningside)"              
    ##  [7] "Spring Hill Baths (inner City)"           
    ##  [8] "Dunlop Park Pool (Corinda)"               
    ##  [9] "Fortitude Valley Pool"                    
    ## [10] "Hibiscus Sports Complex (upper MtGravatt)"
    ## [11] "Ithaca Pool ( Paddington)"                
    ## [12] "Jindalee Pool"                            
    ## [13] "Manly Pool"                               
    ## [14] "Mt Gravatt East Aquatic Centre"           
    ## [15] "Musgrave Park Pool (South Brisbane)"      
    ## [16] "Newmarket Pool"                           
    ## [17] "Runcorn Pool"                             
    ## [18] "Sandgate Pool"                            
    ## [19] "Langlands Parks Pool (Stones Corner)"     
    ## [20] "Yeronga Park Pool"

On comprend donc que la fonction `read.csv()`a le comportament suivant par défaut : le nom des variables se trouve sur la première ligne, les chaînes de carctères sont converties en facteur.

Tout ceci est bien entendu **paramétrable**. En particulier la fonction `read.csv()` utilise en interne la fonction plus générale `read.table()`.

#### La fonction `read.table()`

Regardons maintenant le fichier `hotdogs.tsv`. Ce fichier est dans un format texte similaire quoique différent. Les variables sont séparées par des tabulations (représentées par la chaîne `"\t"` dans *R*). De plus, le nom des variables n'est pas inscrit à la première ligne.

On peut toutefois importer encore simplement ce fichier :

``` r
path <- file.path("data","hotdogs.tsv")
hotdogs <- read.table(path,
                      sep="\t",
                      header=FALSE,
                      col.names=c("type", "kcal", "sodium"),
                      stringsAsFactors = TRUE)
head(hotdogs, n=5)
```

    ##   type kcal sodium
    ## 1 Beef  186    495
    ## 2 Beef  181    477
    ## 3 Beef  176    425
    ## 4 Beef  149    322
    ## 5 Beef  184    482

**Remarque :** ici il semble plus intéressant de considérer le type comme un facteur ! Attention toutefois, s'il y a plusieurs variables de type *chaîne de caractère*, il est préférable, à mon avis de toujours passer `FALSE` à l'argument `stringsAsFactors` et de déclarer les facteurs pour les variables concernées (on reviendra plus loin là -dessus).

**Remarque :** dans le cas des fichiers délimités par des tabulations, il existe une fonction spécifique basée sur `read.table()`et qui propose ces propres arguments par défaut. C'est la fonction `read.delim()`.

**Remarque :** penser à utiliser l'aide de *R* pour voir les options de toutes ces fonctions. Par exemple, dans une console :

``` r
?read.table
```

Les fonctions `read.csv()` et `read.delim` possèdent les mêmes arguments que `read.table()`. On peut regarder plus particulièrement les argument `skip`et `nrows`et *oublier* les autres dans un premier temps !

### Le package `readr`

#### Présentation

Ce package est un package récent à installer. Il se veut plus rapide que les outils de base pour l'import de grande base de données. Sa syntaxe se veut aussi plus simple.

``` r
install.packages("readr")
library(readr)
```

Ce package propose principalement trois fonctions :

-   `read_csv()` : même objectif que `read.csv()`

-   `read_tsv()` : même objectif que `read.delim()`

-   `read_delim()` : fonction de base dont les deux fonctions précédentes ne sont que des *wrappers* à l'instar de `read.table()`.

#### La fonction `read_csv()`

Commençons simplement

``` r
path <- file.path("data", "piscines.csv")
piscines <- read_csv(path)
```

    ## Parsed with column specification:
    ## cols(
    ##   Name = col_character(),
    ##   Address = col_character(),
    ##   Latitude = col_double(),
    ##   Longitude = col_double()
    ## )

Il s'est passé quelque chose et nous y reviendrons. Pour simplifier, disons que la fonction a detecté que les variables `Name`et `Address` sont des chaînes de caractères et que `Longitude` et `Latitude`sont des doubles. En particulier les chaînes de caractères n'ont pas été converties en facteurs !

On peut regarder

``` r
class(piscines)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

ou encore

``` r
print(piscines)
```

    ## # A tibble: 20 × 4
    ##                                         Name
    ##                                        <chr>
    ## 1                Acacia Ridge Leisure Centre
    ## 2                            Bellbowrie Pool
    ## 3                                Carole Park
    ## 4                Centenary Pool (inner City)
    ## 5                             Chermside Pool
    ## 6                Colmslie Pool (Morningside)
    ## 7             Spring Hill Baths (inner City)
    ## 8                 Dunlop Park Pool (Corinda)
    ## 9                      Fortitude Valley Pool
    ## 10 Hibiscus Sports Complex (upper MtGravatt)
    ## 11                 Ithaca Pool ( Paddington)
    ## 12                             Jindalee Pool
    ## 13                                Manly Pool
    ## 14            Mt Gravatt East Aquatic Centre
    ## 15       Musgrave Park Pool (South Brisbane)
    ## 16                            Newmarket Pool
    ## 17                              Runcorn Pool
    ## 18                             Sandgate Pool
    ## 19      Langlands Parks Pool (Stones Corner)
    ## 20                         Yeronga Park Pool
    ## # ... with 3 more variables: Address <chr>, Latitude <dbl>,
    ## #   Longitude <dbl>

ou, pour finir :

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

**Remarque :** la fonction `read_csv()` ne renvoie pas simplement un `data.frame`. C'est un objet plus complexe appelé *Tibble*. On retrouvera ces objets lors de l'utilisation du package `dplyr`. Il faut y penser comme à un `data.frame` avec un meilleur affichage et quelques fonctions de *subset* plus efficientes.

#### La fonction `read_delim()`

Entrons plus en détail dans la fonction de base `read_delim()` en nous exerçant sur les données contenues dans `hotdogs.tsv`.

**Remarque :** on pourrait utiliser la fonction `read_tsv()`mais on ne le fera que plus tard...

``` r
path <- file.path("data", "hotdogs.tsv")
read_delim(path,
           delim = "\t"
)
```

    ## Parsed with column specification:
    ## cols(
    ##   Beef = col_character(),
    ##   `186` = col_integer(),
    ##   `495` = col_integer()
    ## )

    ## # A tibble: 53 × 3
    ##     Beef `186` `495`
    ##    <chr> <int> <int>
    ## 1   Beef   181   477
    ## 2   Beef   176   425
    ## 3   Beef   149   322
    ## 4   Beef   184   482
    ## 5   Beef   190   587
    ## 6   Beef   158   370
    ## 7   Beef   139   322
    ## 8   Beef   175   479
    ## 9   Beef   148   375
    ## 10  Beef   152   330
    ## # ... with 43 more rows

On voit qu'il y a un soucis avec le nom des variables : il n'est pas présent à la première ligne comme supposé par défaut par la fonction.

``` r
read_delim(path,
           delim = "\t",
           col_names = c("type", "kcal", "sodium")
)
```

    ## Parsed with column specification:
    ## cols(
    ##   type = col_character(),
    ##   kcal = col_integer(),
    ##   sodium = col_integer()
    ## )

    ## # A tibble: 54 × 3
    ##     type  kcal sodium
    ##    <chr> <int>  <int>
    ## 1   Beef   186    495
    ## 2   Beef   181    477
    ## 3   Beef   176    425
    ## 4   Beef   149    322
    ## 5   Beef   184    482
    ## 6   Beef   190    587
    ## 7   Beef   158    370
    ## 8   Beef   139    322
    ## 9   Beef   175    479
    ## 10  Beef   148    375
    ## # ... with 44 more rows

C'est mieux ! On voit que le type de chaque variable est écrit dans la représentation !

Regardons d'autres arguments assez simples :

``` r
read_delim(path,
           delim = "\t",
           col_names = c("type", "kcal", "sodium"),
           skip = 3,
           n_max = 5
)
```

    ## Parsed with column specification:
    ## cols(
    ##   type = col_character(),
    ##   kcal = col_integer(),
    ##   sodium = col_integer()
    ## )

    ## # A tibble: 5 × 3
    ##    type  kcal sodium
    ##   <chr> <int>  <int>
    ## 1  Beef   149    322
    ## 2  Beef   184    482
    ## 3  Beef   190    587
    ## 4  Beef   158    370
    ## 5  Beef   139    322

Enfin, on peut spécifier le type de chaque de plusieurs façons :

``` r
read_delim(path,
           delim = "\t",
           col_names = c("type", "kcal", "sodium"),
           col_types = cols(
               "type" = "c",
               "kcal" = "c",
               "sodium" = "c"
           )
)
```

    ## # A tibble: 54 × 3
    ##     type  kcal sodium
    ##    <chr> <chr>  <chr>
    ## 1   Beef   186    495
    ## 2   Beef   181    477
    ## 3   Beef   176    425
    ## 4   Beef   149    322
    ## 5   Beef   184    482
    ## 6   Beef   190    587
    ## 7   Beef   158    370
    ## 8   Beef   139    322
    ## 9   Beef   175    479
    ## 10  Beef   148    375
    ## # ... with 44 more rows

On peut aussi faire :

``` r
read_delim(path,
           delim = "\t",
           col_names = FALSE
)
```

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_character(),
    ##   X2 = col_integer(),
    ##   X3 = col_integer()
    ## )

    ## # A tibble: 54 × 3
    ##       X1    X2    X3
    ##    <chr> <int> <int>
    ## 1   Beef   186   495
    ## 2   Beef   181   477
    ## 3   Beef   176   425
    ## 4   Beef   149   322
    ## 5   Beef   184   482
    ## 6   Beef   190   587
    ## 7   Beef   158   370
    ## 8   Beef   139   322
    ## 9   Beef   175   479
    ## 10  Beef   148   375
    ## # ... with 44 more rows

Ou

``` r
read_delim(path,
           delim = "\t",
           col_names = c("type", "kcal", "sodium"),
           col_types = cols(
               "type" = col_factor(c("Beef", "Meat", "Poultry"), ordered=FALSE),
               "kcal" = col_double(),
               "sodium" = col_double()
           )
)
```

    ## # A tibble: 54 × 3
    ##      type  kcal sodium
    ##    <fctr> <dbl>  <dbl>
    ## 1    Beef   186    495
    ## 2    Beef   181    477
    ## 3    Beef   176    425
    ## 4    Beef   149    322
    ## 5    Beef   184    482
    ## 6    Beef   190    587
    ## 7    Beef   158    370
    ## 8    Beef   139    322
    ## 9    Beef   175    479
    ## 10   Beef   148    375
    ## # ... with 44 more rows

Il faut passer un peu de temps pour bien tout comprendre... Tout est dans l'aide et dans les *vignettes*.

### Le package `data.table` et sa fonction `fread()`
