나이와 월급의 관계
================
오유리
July 30, 2020

## 3\. 나이와 월급의 관계

### 분석 절차

#### 1\. 변수 검토하기

나이와 월급의 관계를 분석하려면 나이 변수가 있어야 한다. 태어난 연도 변수만 있기 때문에 연도를 이용해서 나이 변수를 만들어야
한다.

``` r
class(welfare$birth)
```

    ## [1] "numeric"

``` r
summary(welfare$birth)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1907    1946    1966    1968    1988    2014

``` r
qplot(welfare$birth)
```

![](welfare03_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

#### 2\. 전처리

태어난 연도: 1900\~2014 모름/무응답:9999

``` r
# 이상치 확인
summary(welfare$birth)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1907    1946    1966    1968    1988    2014

``` r
# 결측치 확인
table(is.na(welfare$birth))
```

    ## 
    ## FALSE 
    ## 16664

출력된 결과엔 이상치와 결측치가 없으므로 파생변수를 만드는 단계로 넘어간다.

만약 이상치가 발견된다면

``` r
welfare$birth <- ifelse(welfare$birth == 9999, NA, welfare$birth)
table(is.na(welfare$birth))
```

    ## 
    ## FALSE 
    ## 16664

#### 3\. 파생변수 만들기 - 나이

2015년에 조사가 시작됐으니 2015를 뺀 후 1일 더해 나이를 구하면 된다. 변수를 만들고 summary(), qplot()을
이용해 특징을 살펴본다.

``` r
welfare$age <- 2015 - welfare$birth + 1
summary(welfare$age)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    2.00   28.00   50.00   48.43   70.00  109.00

``` r
qplot(welfare$age)
```

![](welfare03_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### 나이와 월급의 관계 분석하기

#### 1\. 나이에 따른 월급 평균표 만들기

``` r
age_income <- welfare %>% 
  filter(!is.na(income)) %>% 
  group_by(age) %>% 
  summarise(mean_income = mean(income))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
head(age_income)
```

    ## # A tibble: 6 x 2
    ##     age mean_income
    ##   <dbl>       <dbl>
    ## 1    20        121.
    ## 2    21        106.
    ## 3    22        130.
    ## 4    23        142.
    ## 5    24        134.
    ## 6    25        145.

#### 2\. 그래프 만들기

x축은 나이, y축은 월급

``` r
ggplot(data=age_income, aes(x=age,y=mean_income)) + geom_line()
```

![](welfare03_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
