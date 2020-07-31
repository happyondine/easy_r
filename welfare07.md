성별 직업 빈도
================
오유리
July 30, 2020

## 7\. 성별 직업 빈도

### 분석 절차

검토 및 전처리 변수: 성별, 직업

### 성별 직업 빈도 분석하기

#### 1\. 성별 직업 빈도표 만들기

상위10 성별 직업별 빈도를 구한다.

남성 상위 10 직업

``` r
job_male <- welfare %>% 
  filter(!is.na(job) & sex == "male") %>% 
  group_by(job) %>% 
  summarise(n = n()) %>% 
  arrange(desc(n)) %>% 
  head(10)

job_male
```

여성 상위 10 직업

``` r
job_female <- welfare %>% 
  filter(!is.na(job) & sex == "female") %>% 
  group_by(job) %>% 
  summarise(n = n()) %>% 
  arrange(desc(n)) %>% 
  head(10)

job_female
```

#### 2\. 그래프 만들기

``` r
# 남성 직업 빈도 상위 10
ggplot(data=job_male, aes(x=reorder(job,n),y=n)) + geom_col() +coord_flip()
```

![](welfare07_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
# 여성 직업 빈도 상위 10
ggplot(data=job_female, aes(x=reorder(job,n), y = n)) + geom_col() + coord_flip()
```

![](welfare07_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->
