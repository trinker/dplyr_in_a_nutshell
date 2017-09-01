dplyr In a Nutshell
===

This is a minimal guide, mostly for myself, to remind me of the most import dplyr functions and how they relate to base R functions that I'm familiar with. Also check out [tidyr In a Nutshell](https://github.com/trinker/tidyr_in_a_nutshell).



# 8 dplyr Functions to Rule the World

### Speedy Table

`tbl_df`


### The 5 Guys + 1

1. `filter`
2. `select`
3. `mutate`
4. `group_by`
5. `summarise`
6. `arrange`

### Chaining (pronounced "then")

`%>%`

# Relating the Functions

### Speedy Table 

`tbl_df` works similar to `data.table` in that it prints sensibly.

### Relating the 5 Guys + 1 to base R

List of dplyr functions and the base functions they're related to:

Base Function    | dplyr Function(s) | Special Powers
-----------------|-------------------|-----------------------------
subset           |  filter & select  | filter rows & select columns
transform        |  mutate           | operate with columns not yet created
split            |  group_by         | splits without cutting
lapply + do.call |  summarise        | apply and bind in a single bound
order + with     |  arrange          | "I only have to specify dataframe once?"

### Chaining

`%>%`... Do you know ggplot2's `+`?  Same idea.  

![](chain.png)

*Basically previous input in chain supplied as argument 1 to function on right side.*

# Demos
### Speedy Table

```r
library(dplyr)
mtcars2 <- tbl_df(mtcars)
```

### The 5 Guys

```r
filter(mtcars2[1:10, ], cyl == 8)
```

```
Source: local data frame [2 x 11]

   mpg cyl disp  hp drat   wt  qsec vs am gear carb
1 18.7   8  360 175 3.15 3.44 17.02  0  0    3    2
2 14.3   8  360 245 3.21 3.57 15.84  0  0    3    4
```

```r
select(mtcars2[1:10, ], mpg, cyl, hp:vs)
```

```
Source: local data frame [10 x 7]

                   mpg cyl  hp drat    wt  qsec vs
Mazda RX4         21.0   6 110 3.90 2.620 16.46  0
Mazda RX4 Wag     21.0   6 110 3.90 2.875 17.02  0
Datsun 710        22.8   4  93 3.85 2.320 18.61  1
Hornet 4 Drive    21.4   6 110 3.08 3.215 19.44  1
Hornet Sportabout 18.7   8 175 3.15 3.440 17.02  0
Valiant           18.1   6 105 2.76 3.460 20.22  1
Duster 360        14.3   8 245 3.21 3.570 15.84  0
Merc 240D         24.4   4  62 3.69 3.190 20.00  1
Merc 230          22.8   4  95 3.92 3.150 22.90  1
Merc 280          19.2   6 123 3.92 3.440 18.30  1
```

```r
arrange(mtcars2[1:10, ], cyl, disp)
```

```
Source: local data frame [10 x 11]

    mpg cyl  disp  hp drat    wt  qsec vs am gear carb
1  22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
2  22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
3  24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
4  21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
5  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
6  19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
7  18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
8  21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
9  18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
10 14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
```

```r
mutate(mtcars2[1:10, ], displ_l = disp / 61.0237, displ_l_add1 = displ_l + 1)
```

```
Source: local data frame [10 x 13]

    mpg cyl  disp  hp drat    wt  qsec vs am gear carb displ_l
1  21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4   2.622
2  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4   2.622
3  22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1   1.770
4  21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1   4.228
5  18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2   5.899
6  18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1   3.687
7  14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4   5.899
8  24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2   2.404
9  22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2   2.307
10 19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4   2.746
Variables not shown: displ_l_add1 (dbl)
```

```r
summarise(mtcars, mean(disp))
```

```
  mean(disp)
1      230.7
```

### Chaining


```r
mtcars2 %>%
    group_by(cyl) %>%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp))
```

```
Source: local data frame [3 x 4]

  cyl    md     mh   mdh
1   4 105.1  82.64 187.8
2   6 183.3 122.29 305.6
3   8 353.1 209.21 562.3
```

```r
mtcars2 %>%
    group_by(cyl, gear) %>%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp)) %>%
    arrange(-cyl, -gear)
```

```
Source: local data frame [8 x 5]
Groups: cyl

  cyl gear    md    mh   mdh
1   8    5 326.0 299.5 625.5
2   8    3 357.6 194.2 551.8
3   6    5 145.0 175.0 320.0
4   6    4 163.8 116.5 280.3
5   6    3 241.5 107.5 349.0
6   4    5 107.7 102.0 209.7
7   4    4 102.6  76.0 178.6
8   4    3 120.1  97.0 217.1
```

```r
## Use `%>%` with base functions too!!!
mtcars2 %>%
    group_by(cyl, gear) %>%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp)) %>%
    arrange(-cyl, -gear) %>%
	head()
```

```
Source: local data frame [6 x 5]
Groups: cyl

  cyl gear    md    mh   mdh
1   8    5 326.0 299.5 625.5
2   8    3 357.6 194.2 551.8
3   6    5 145.0 175.0 320.0
4   6    4 163.8 116.5 280.3
5   6    3 241.5 107.5 349.0
6   4    5 107.7 102.0 209.7
```

```r
mtcars2 %>%
    group_by(cyl) %>%
    summarise(max(disp), hp[1])
```

```
Source: local data frame [3 x 3]

  cyl max(disp) hp[1]
1   4     146.7    93
2   6     258.0   110
3   8     472.0   175
```

```r
mtcars2 %>%
    group_by(cyl) %>%
    summarise(n = n()) 
```

```
Source: local data frame [3 x 2]

  cyl  n
1   4 11
2   6  7
3   8 14
```

```r
table(mtcars$cyl) 
```

```

 4  6  8 
11  7 14 
```


