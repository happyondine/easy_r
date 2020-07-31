연령대 및 성별 월급 차이
================
오유리
July 30, 2020

## 5\. 연령대 및 성별 월급 차이

### 분석 절차

연력대, 성별, 월급 변수를 검토 및 전처리한다. 연령대 및 성별 월급 평균표와 그래프를 만든다.

### 연령대 및 성별 월급 차이 분석하기

#### 1\. 연령대 및 성별 월급 평균표 만들기

각 연령대에서 성별에 따른 월급에 차이가 있는지 알아보기 위해 연령대 및 성별에 따른 월급 평균표를 만든다.

``` r
sex_income <- welfare %>% 
  filter(!is.na(income)) %>% 
  group_by(ageg,sex) %>% 
  summarise(mean_income = mean(income))
```

    ## `summarise()` regrouping output by 'ageg' (override with `.groups` argument)

``` r
sex_income
```

#### 2\. 그래프 만들기

연령대별로 표현되도록 x축에 ageg를 지정한다. 막대가 성별에 따라 다른 색으로 표현되도록 fill 에 sex를 지정하고 연령대
순으로 설정한다.

``` r
ggplot(data = sex_income, aes(x = ageg, y = mean_income, fill = sex)) + geom_col() + scale_x_discrete(limits = c("young","middle","old"))
```

![](welfare05_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> \#\#\#\#
성별 막대 분리 위 그래프는 차이를 비교하기 어렵기 때문에 막대를 분리한다.

중년에 월급하지아 크게 벌어져 남성이 166만 원 가량 더 많다. 노년에는 차이가 줄지만 여전히 92만원 가량 남성이 더 많다.
남성의 경우 노년과 초년간 월급하지가 크지 않다.

``` r
ggplot(data = sex_income, aes(x = ageg, y = mean_income, fill = sex)) + geom_col(position = "dodge") + scale_x_discrete(limits = c("young","middle","old"))
```

![](welfare05_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### 나이 및 성별 월급 차이 분석하기

#### 그래프 만들기

연령대로 구분하지 않고, 나이 및 성별 월급 평균표와 그래프를 만든다. 그래프는 선그래프로 만들고, 월급 평균 선이 성별에 따라
다른색으로 표현되도록 한다.

``` r
sex_age <- welfare %>% 
  filter(!is.na(income)) %>% 
  group_by(age,sex) %>% 
  summarise(mean_income = mean(income))
```

    ## `summarise()` regrouping output by 'age' (override with `.groups` argument)

``` r
head(sex_age)

ggplot(data=sex_age, aes(x=age, y=mean_income, col=sex)) + geom_line()
```

![](welfare05_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
