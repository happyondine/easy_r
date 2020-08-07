건강상태에 따른 소득차이
================
오유리
02/08/2020

### 분석 절차

### 연령대 변수 검토 및 전처리하기

#### 1\. 월급 변수 및 결측치 확인

월급이 0\~9999 사이가 아니면 결측치를 NA로 처리한다.

``` r
class(welfare$income)
table(welfare$income)
welfare$income <- ifelse(welfare$income %in% c(0,9999), NA, welfare$income)
```

#### 2\. 건강상태 변수 및 결측치 확인

결측치는 없지만 변수를 범주형태로 바꿔야 하는것을 알 수 있다.

``` r
class(welfare$med_cond)
table(welfare$med_cond)
```

#### 3\. 건강상태 변수 타입을 레벨로 바꾼다.

``` r
welfare$med_cond <- as.factor(welfare$med_cond)
class(welfare$med_cond)
levels(welfare$med_cond)
```

### 건강상태별 평균 월급표

#### 건강상태별로 평균 월급을 구한다

``` r
med_income <- welfare %>% 
  filter(!is.na(income) & !is.na(med_cond)) %>% 
  group_by(med_cond) %>% 
  summarise(mean_income = mean(income))

med_income
```

### 소득과 건강상태 그래프

#### 1\. 1\~5변수로 된 건강상태를 알아보기 쉽게 바꾼다.

``` r
med_income <- med_income %>% 
  mutate(healthy = ifelse(med_cond == "1", "Very Healthy", ifelse(med_cond == "2",  "Healthy", ifelse(med_cond == "3", "Normal", ifelse(med_cond == "4", "Not Healthy", ifelse(med_cond == "5", "Very Not Healthy", NA))))))

med_income
```

#### 2\. 소득이 많은 순으로 그래프를 그린다

``` r
ggplot(med_income,aes(reorder(healthy,-mean_income),mean_income))+geom_point()  + labs(x="Health Condition",y="Income", title= "Relationship between Health Condition and Income") 
```

![](welfare10_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

#### 결론: 가장 건강이 안좋은 사람들의 소득 평균이 가장 높다. 하지만 가장 건강한 사람들은 근소한 차이로 평균 소득이 그 다음으로 높다. 소득과 건강상태는 비례하지만, 적절한 선의 소득에 만족하는 사람들이 건강을 잘 챙길 수 있다고 해석될 수도 있겠다.
