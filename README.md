dplyr In A Nutshell
===

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

### Chaining

`%.%`

# Relating the Functions

### Speedy Table 

`tbl_df` works like similar to `data.table` 

### Relating the 5 Guys + 1 to base R

List of dplyr functions and the base functions they're related to:

Base Function    | dplyr Function(s) | Special Powers
-----------------|-------------------|-----------------------------
subset           |  filter & select  | filter rows & select columns
transform        |  mutate           | operate with columns not yet created
split            |  group_by         | splits without cutting
lapply + do.call |  summarise        | apply and join in a single bound
order + with     |  arrange          | "I only have to specify dataframe once?"

### Chaining

`%*%`... Do you know ggplot2's `+`?  Same idea.  

![](chain.png)

*Basically previous input in chain supplied as argument 1 to function on right side.*

# Demos


```r
library(dplyr)

mtcars2 <- tbl_df(mtcars)

mtcars2 %.%
    group_by(cyl) %.%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp))
```

```
Source: local data frame [3 x 4]

  cyl    md     mh   mdh
1   4 105.1  82.64 187.8
2   8 353.1 209.21 562.3
3   6 183.3 122.29 305.6
```

```r
mtcars2 %.%
    group_by(cyl, gear) %.%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp)) %.%
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
## Use `%.%` with base functions too!!!
mtcars2 %.%
    group_by(cyl, gear) %.%
    summarise(md=mean(disp), mh=mean(hp), mdh=mean(disp + hp)) %.%
    arrange(-cyl, -gear) %.%
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
mtcars2 %.%
    group_by(cyl) %.%
    summarise(max(disp), hp[1])
```

```
Source: local data frame [3 x 3]

  cyl max(disp) hp[1]
1   4     146.7    93
2   8     472.0   175
3   6     258.0   110
```

```r
mtcars2 %.%
    group_by(cyl) %.%
    summarise(n = n()) 
```

```
Source: local data frame [3 x 2]

  cyl  n
1   4 11
2   8 14
3   6  7
```

```r
table(mtcars$cyl) 
```

```

 4  6  8 
11  7 14 
```
