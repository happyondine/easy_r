성별에 따른 월급 차이
================
오유리
July 30, 2020

## 2\. 성별에 따른 월급 차이

### 분석 절차

변수 검토 및 전처리 (성별, 월급)-\> 변수 간 관계 분석(성별 월급 평균표 만들기, 그래프 만들기)

### 성별 변수 검토 및 전처리

#### 1\. 변수 검토하기

class()로 sex변수의 타입을 파악하고 table()로 각 범주에 몇명이 있는지 알아본다. 출력 결과를 보면 sex는
numeric이고 1,2로 구성된다. 1은 7578, 2는 9086명이 존재 한다.

``` r
class(welfare$sex)

table(welfare$sex)
```

#### 2\. 전처리

1은 남자,2는 여자, 9는 무응답이다. 이상치(9포함)는 NA값을 부여한다.

``` r
table(welfare$sex)
```

    ## 
    ##    1    2 
    ## 7578 9086

``` r
#이상치 결측 처리
welfare$sex <- ifelse(welfare$sex == 9, NA, welfare$sex)


#결측치확인
table(is.na(welfare$sex))
```

    ## 
    ## FALSE 
    ## 16664

``` r
#성별 항목 이름 부여
welfare$sex <- ifelse(welfare$sex ==1, "male", "female")
table(welfare$sex)
```

    ## 
    ## female   male 
    ##   9086   7578

``` r
qplot(welfare$sex)
```

![](welfare02_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### 월급 변수 검토 및 전처리

#### 1\. 변수 검토하기

월급: 일한달의 월 평균 임금(1만원 단위) income변수를 검토하고 qplot()으로 분포를 확인 성별 변수는 범주 변수이기
때문에 table()로 각 범주의 빈도를 확인하면 특징으 ㄹ파악할 수 있다. 하지만 월급 변수는 연속 변수 이기 때문에
table()을 이용하면 너무 많은 항목이 출력되서 summary()로 요약 통계량을 확인한다.

``` r
class(welfare$income)
```

    ## [1] "numeric"

``` r
summary(welfare$income)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0   122.0   192.5   241.6   316.6  2400.0   12030

``` r
qplot(welfare$income)
```

![](welfare02_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

income 은 numeric타입이고 0\~2400만 원 사이의 값을 지니며 122\~316만 원 사이에 가장 많이 분포한다.
평균은 241.6만원, 중앙값은 평균보다 작은 192.5만 원으로 전반적으로 낮은 값 쪽으로 치우쳐 있다.

qplot은 최댓값까지 표현하도록 기본값이 설정되어 있다. 그래프를 보면 x축이 2500까지 있어서 대다수를 차지하는
0\~1000사이의 데이터가 잘 표현되지 않는다. xlim을 이용해 0\~1000까지 설정한다.

0\~250사이에 가장 많은 사람이 분포한다.

``` r
qplot(welfare$income) +xlim(0,1000)
```

![](welfare02_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

#### 2\. 전처리

월급은 1\~9998이고 무응답은 9999이다.

``` r
summary(welfare$income)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0   122.0   192.5   241.6   316.6  2400.0   12030

직업이 없어서 월급을 받지 않는 응답자가 있어서 결측치가 존재. 값이 0이나 9999면 결측 처리하고 이를 제외한 후 분석 한다.

``` r
welfare$income <- ifelse(welfare$income %in% c(0,9999), NA, welfare$income)

#결측치 확인
table(is.na(welfare$income))
```

    ## 
    ## FALSE  TRUE 
    ##  4620 12044

### 성별에 따른 월급 차이 분석하기

#### 1\. 성별 월급 평균표 만들기

변수 간 관계를 분석한다. 성별 월급 평균표를 만든다.

``` r
sex_income <- welfare %>%  
  filter(!is.na(income)) %>% 
  group_by(sex) %>% 
  summarise(mean_income = mean(income))

sex_income
```

    ## # A tibble: 2 x 2
    ##   sex    mean_income
    ##   <chr>        <dbl>
    ## 1 female        163.
    ## 2 male          312.

#### 2\. 그래프 만들기

막대 그래프를 만든다. 남성이 여성의 두배 가까이 많다는 걸 알 수 있다.

``` r
ggplot(data= sex_income, aes(x=sex, y= mean_income)) + geom_col()
```

![](welfare02_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
