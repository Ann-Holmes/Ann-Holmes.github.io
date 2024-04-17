---
title: Tidyverse 中的 tidy evaluation
toc: true
date: 2024-04-18 00:21:03
tags:
categories: [知识碎片,R]
---

Tidyverse 是 R 中用于数据分析的一套工具包. 由 R 大神, 现任 Rstudio 的首席科学家, Hadley Wickham 编写. 据他的[个人网站](https://hadley.nz/)介绍, 他现在和他的丈夫还有狗狗生活在休斯顿. 

Tidyverse 中的 `dplyr` 和 `tidyr` 是数据分析中最常用到的两个包. `dplyr` 主要用于操作数据, `tdiyr` 则主要是为了将数据整理成  [Tidy Data](https://tidyr.tidyverse.org/index.html). 

所谓的 [Tidy Data](https://tidyr.tidyverse.org/index.html) 是指符合下面三条标准的表格数据:
- 变量即是列, 列即是变量;
- 观测即是行, 行即是观测; 
- 单元格是值, 值就是单元格;

总之, 不管是 `dplyr` 还是 `tidyr` 都会涉及到操作表格数据. 

在操作表格数据的过程中, 不可避免的会涉及到对**列**进行选择, 从而对不同的**列**应用不同的统计计算操作. 在这个步骤中, 就涉及到如何选定**列**的问题.

<!--more-->

# Tidy Evaluation

在 Tidyverse 中, 几乎所有的操作都会经过 Tidy Evaluation 来选择**列**进行对应的操作. Tidy Evaluation 有两种形式: 
- Data-masking 
- Tidy-selection

## 变量的含义

在 Tidyverse 中很容易模糊 **变量(Variable)** 一词的含义. 由于 Tidyverse 常用于数据分析, 在数据分析中, 变量还可以是数学函数的中变量, 也可以是统计学中的变量. 而 Tidyverse 又是 R 语言的一个工具包, 所以变量还可以是编程中的变量. 因此, 在正式介绍两种形式的 Tidy Evaluation 之前有必要澄清一下变量的含义. 

在 Tidyverse 或者 R 语言的使用环境中, 常常有两类 **变量(Variable)**:
- **env-variable**(环境变量): 这里指的是编程意义上的变量. 由赋值符号 `<-` 创建. 
- **data-variable**(数据变量): 这里指的数学或者统计意义上的变量. 比如: 线性回归中的自变量, 因变量等等. 在表格数据中, 这些变量指的就是表格中的一列. 

> 注意:
> "环境变量" 在这里不是 Linux 中的系统环境变量. 因为 R 中有一种对象类型为 `env`. `env` 是一种特殊的类型, 每个 R 进程创建时, 会创建一个 `env` 对象存储该进程中定义的变量. 因此, 这里的 **env-variable** 指的是在 R 进程中的某个 `env` 下的变量. 

本文后续会分别使用 env-variable 和 data-variable 来区别两种变量的不同. 

## Data-masking

Data-masking 允许在一些 Tidyverse 操作中, 直接使用 data-varaible 来选定要操作的列. 比如: `my_variable` 而不是 `df$my_variable`. 

常见的使用 Data-masking 的函数有: [`arrange()`](https://dplyr.tidyverse.org/reference/arrange.html), [`count()`](https://dplyr.tidyverse.org/reference/count.html), [`filter()`](https://dplyr.tidyverse.org/reference/filter.html), [`group_by()`](https://dplyr.tidyverse.org/reference/group_by.html), [`mutate()`](https://dplyr.tidyverse.org/reference/mutate.html), [`summarise()`](https://dplyr.tidyverse.org/reference/summarise.html), [`expand()`](https://tidyr.tidyverse.org/reference/expand.html), [`crossing()`](https://tidyr.tidyverse.org/reference/expand.html), [`nesting()`](https://tidyr.tidyverse.org/reference/expand.html)

这些函数的第一个参数往往是一个 DataFrame, 这样在这些函数内部可以直接使用 DataFrame 中的 data-variable 而不需要指定这个 data-variable 来自哪个 DataFrame. 

 以 `mtcars` 为例, 如果要按照 `cyl` 和 `mpg` 排序, 可以这样写: 
 
```R
arrange(mtcars, cyl, mpg)
```

而不是

```R
arrange(mtcars, mtcars$cyl, mtcars$mpg)
```

也就是说通过 Data-masking, 我们可以方便的使用 Tidyverse 中的操作函数. 

## Tidy-selection

Tidy-selection 不仅可以直接使用 data-variable 来选定**列**, 还可以根据**列**的位置, 名字和数据类型来选定**列**. 

常见的使用 Tidy-selection 的函数有:  [`drop_na()`](https://tidyr.tidyverse.org/reference/drop_na.html), [`fill()`](https://tidyr.tidyverse.org/reference/fill.html), [`pivot_longer()`](https://tidyr.tidyverse.org/reference/pivot_longer.html), [`pivot_wider()`](https://tidyr.tidyverse.org/reference/pivot_wider.html), [`nest()`](https://tidyr.tidyverse.org/reference/nest.html), [`unnest()`](https://tidyr.tidyverse.org/reference/unnest.html), [`separate()`](https://tidyr.tidyverse.org/reference/separate.html), [`extract()`](https://tidyr.tidyverse.org/reference/extract.html), [`unite()`](https://tidyr.tidyverse.org/reference/unite.html)` `[`across()`](https://dplyr.tidyverse.org/reference/across.html), [`relocate()`](https://dplyr.tidyverse.org/reference/relocate.html), [`rename()`](https://dplyr.tidyverse.org/reference/rename.html), [`select()`](https://dplyr.tidyverse.org/reference/select.html) 和 [`pull()`](https://dplyr.tidyverse.org/reference/pull.html) 

Tidy-selection 有自己的一套 DSL(Domain Specific Language), 详情可以参考: [?tidyr_tidy_select](https://tidyr.tidyverse.org/reference/tidyr_tidy_select.html). 

这里举一个简单的列子来说 Tidy-selection 的方便之处. 

`starwars` 是一个 87 行 x 14 列的表格, 可以在 R 中用 `?dplyr::starwars` 查看详细的说明. 前五行如下:

```R
head(starwars, 5)

# A tibble: 5 × 14
#>   name           height  mass hair_color skin_color  eye_color birth_year sex    gender    homeworld species films     vehicles  starships
#>   <chr>           <int> <dbl> <chr>      <chr>       <chr>          <dbl> <chr>  <chr>     <chr>     <chr>   <list>    <list>    <list>   
#> 1 Luke Skywalker    172    77 blond      fair        blue            19   male   masculine Tatooine  Human   <chr [5]> <chr [2]> <chr [2]>
#> 2 C-3PO             167    75 NA         gold        yellow         112   none   masculine Tatooine  Droid   <chr [6]> <chr [0]> <chr [0]>
#> 3 R2-D2              96    32 NA         white, blue red             33   none   masculine Naboo     Droid   <chr [7]> <chr [0]> <chr [0]>
#> 4 Darth Vader       202   136 none       white       yellow          41.9 male   masculine Tatooine  Human   <chr [4]> <chr [0]> <chr [1]>
#> 5 Leia Organa       150    49 brown      light       brown           19   female feminine  Alderaan  Human   <chr [5]> <chr [1]> <chr [0]>
```


假设现在我想把 `starwars` 中列明以 "h" 开头, 并且数据类型为数字的列以及 "name" 和 "species" 列一并筛选出来, 就可以用 Tidy-selection:

```R
select(starwars, name, species, starts_with("h") & where(is.numeric))

#>    name               species height
#>    <chr>              <chr>    <int>
#>  1 Luke Skywalker     Human      172
#>  2 C-3PO              Droid      167
#>  3 R2-D2              Droid       96
#>  4 Darth Vader        Human      202
#>  5 Leia Organa        Human      150
```

## 在自己编写的函数中使用 Data-masking 和 Tidy-selection

虽然在交互式的使用 Data-masking 和 Tidy-selection 做数据分析非常方便, 但是在编写函数时直接使用 Data-masking 和 Tidy-selection 可能会导致错误或者函数没有按照预期去操作数据. 这是因为在编写函数的时候很容易会混淆 data-variable 和 env-variable.

假设我想写一个做分组统计的函数, 还是以上面的 `starwars` 数据集作为例子. 最直接的想法可能是这样的:
```R
group_statistic <- function(df, group_var, value_var) {
  df %>%
    group_by(group_var) %>%
    summarise(
      n = n(),
      mean = mean(value_var, na.rm = TRUE),
      sd = sd(value_var, na.rm = TRUE)
    )
}
```

然而, 上面的代码直接应用到 `starwars` 数据集上会有问题:
```R
group_statistic(starwars, species, height)

#> Error in `group_by()`:
#> ! Must group by variables found in `.data`.
#> ✖ Column `group_var` is not found.
#> Run `rlang::last_trace()` to see where the error occurred.
```

这是因为 Data-masking 机制, `group_by()` 和 `summarise()` 会将 `group_var` 和 `value_var` 当作 `df` 中的 data-variable, 然而 `starwars` 并没有名为 `group_var` 和 `value_var` 的列, 就会报错. 

### 拥抱变量

如果像让 `group_by()` 和 `summarise()` 将 `group_var` 和 `value_var` 作为参数, 可以使用 `{{ var }}` 的形式来 embrace(拥抱) 变量, 如下:

```R
group_statistic <- function(df, group_var, value_var) {
  df %>%
    group_by({{ group_var }}) %>%
    summarise(
      n = n(),
      mean = mean({{ value_var }}, na.rm = TRUE),
      sd = sd({{ value_var }}, na.rm = TRUE)
    )
}

group_statistic(starwars, species, height) %>% head()

#> # A tibble: 6 × 4
#>   species      n  mean    sd
#>   <chr>    <int> <dbl> <dbl>
#> 1 Aleena       1   79   NA  
#> 2 Besalisk     1  198   NA  
#> 3 Cerean       1  198   NA  
#> 4 Chagrian     1  196   NA  
#> 5 Clawdite     1  168   NA  
#> 6 Droid        6  131.  49.1
```

这样函数就可以按照预期执行了, 同样的方法对于 Tidy-selection 也是一样的. 

这样, 编写出来的函数可以在参数传递 data-variable, 也就是说我们自己编写的函数也具有 Data-masking 或者 Tidy-selection 的能力. 

### Data-masking 的字符串参数

除了上面这种情况, 有时我们编写的函数希望接收字符串参数. 这种情况下可以用 `.data` 变量来定位 data-variable. 

```R
group_statistic <- function(df, group_var, value_var) {
  df %>%
    group_by(.data[[group_var]]) %>%
    summarise(
      n = n(),
      mean = mean(.data[[value_var]], na.rm = TRUE),
      sd = sd(.data[[value_var]], na.rm = TRUE)
    )
}

group_statistic(starwars, "species", "height") %>% head()
#> # A tibble: 6 × 4
#>   species      n  mean    sd
#>   <chr>    <int> <dbl> <dbl>
#> 1 Aleena       1   79   NA  
#> 2 Besalisk     1  198   NA  
#> 3 Cerean       1  198   NA  
#> 4 Chagrian     1  196   NA  
#> 5 Clawdite     1  168   NA  
#> 6 Droid        6  131.  49.1
```

`.data` 变量是一个特殊的 `env` 对象, 和 `.env` 类似可以通过 `$` 或者 `[[` 来获取相应的变量, `.data` 获取的特指 data-variable.

### Tidy-selection 的字符串参数

在 Tidy-selection 支持的函数中, 有一对特殊的函数 `any_of()` 和 `all_of()` 可以从字符串的变量名来获取对应的 data-variable. 如下: 

```R
select_names <- function(df, vars) {
  df %>%
    select(any_of(vars))
}

select_names(starwars, c("species", "height")) %>% head()
#> # A tibble: 6 × 2
#>   species height
#>   <chr>    <int>
#> 1 Human      172
#> 2 Droid      167
#> 3 Droid       96
#> 4 Human      202
#> 5 Human      150
#> 6 Human      178
```

而 `any_of()` 和 `all_of()` 的区别就在于如果有不在指定 DataFrame 中的变量时是否会报错. 

总之, 在自己编写的函数中使用 Data-masking 和 Tidy-selection 会导致错误, 可以使用 `{{ var }}` 的方式来将 data-masking 和 Tidy-selection 的能力应用到自己编写的函数中. 
