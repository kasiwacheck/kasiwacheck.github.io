---
title:  "Pandas_Dataframe_시계열 데이터"
date: 2022-07-12 13:30:00
categories: pandas
---
Pandas: chapter 5 데이터 사전 처리
==========
6.시계열 데이터
----------
### 시계열 데이터란
```
#시계열 데이터
#타임 스탬프와 피리어드; 타임 스탬프는 특정 시점, 피리어드는 두 시점 사이의 일정기간
```
### to_datetime( ) : 문자열 -> timestamp
```
#문자열을 timestamp로 변환 to_datetime()함수 사용

import pandas as pd

df = pd.read_csv('C:/Users/taesa/Desktop/AI섭렵하기/part5/stock-data.csv')

print(df.head())
print('\n')
print(df.info())

df['new_date'] = pd.to_datetime(df['Date'])

print(df.head())
print('\n')
print(df.info())
print('\n')
print(type(df['new_date'][0]))

#새로운 행 인덱스 지정, 기존 Date열은 삭제
df.set_index('new_date', inplace=True)
df.drop('Date', axis=1, inplace=True)

print(df.head())
print('\n')
print(df.info())
```
### to_period( ) : timestamp -> period
```
#timestamp를 period로 변환 to_period 함수 사용
import pandas as pd

date = ['2019-01-01', '2019-03-01', '2019-06-01']


#문자열을 타임스탬프로 변환
ts_date = pd.to_datetime(date)
print(ts_date)
print('\n')

#타임스탬프를 피리어드로 변환
pr_day = ts_date.to_period(freq='D')
print(pr_day)
pr_month = ts_date.to_period(freq='M')
print(pr_month)
pr_year = ts_date.to_period(freq='A')
print(pr_year)


```
### data_range( ) 함수
```
#date_range() 함수를 사용하여 타임스탬프가 들어있는 배열 형태의 시계열 데이터를 만들 수 있다.
#timestamp 배열
import pandas as pd

#타임스탬프 배열 만들기
ts_ms = pd.date_range(start='2019-01-01',
                           end=None,
                           periods=6,
                           freq='MS',
                           tz='Asia/Seoul')
print(ts_ms)

ts_ms = pd.date_range(start='2019-01-01',
                           end=None,
                           periods=6,
                           freq='M',
                           tz='Asia/Seoul')
print(ts_ms)

ts_ms = pd.date_range(start='2019-01-01',
                           end=None,
                           periods=6,
                           freq='3M',
                           tz='Asia/Seoul')
print(ts_ms)

```
### period_range( ) 함수
```
#피리어드 배열 만들기
pr_m = pd.period_range(start = '2019-01-01',
                       end = None,
                       periods = 3,
                       freq = 'M')

print(pr_m)
print('\n')

pr_h = pd.period_range(start = '2019-01-01',
                       end = None,
                       periods = 3,
                       freq = 'H')

print(pr_h)
print('\n')

pr_2h = pd.period_range(start = '2019-01-01',
                       end = None,
                       periods = 3,
                       freq = '2H')

print(pr_2h)
```