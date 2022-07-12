---
title:  "Pandas_Dataframe_시계열 데이터 활용"
date: 2022-07-12 14:00:00
categories: pandas
---
Pandas: chapter 5 데이터 사전 처리
==========
6.시계열 데이터 활용
----------
### 날짜 데이터 분리
```
import pandas as pd

df = pd.read_csv('C:/Users/taesa/Desktop/AI섭렵하기/part5/stock-data.csv')

#문자열인 date를 to_datetime을 이용해 timestamp 형식인 new_date열을 생성
df['new_date'] = pd.to_datetime(df['Date'])

print(df.head())
print('\n')

#dt를 이용해 각 값만 추출해서 새로운 열 만듬
df['Year'] = df['new_date'].dt.year
df['Month'] = df['new_date'].dt.month
df['Day'] = df['new_date'].dt.day

print(df.head())

#to_period를 사용해 timestamp를 period로 변환하고 열에 추가함
df['date_yr'] = df['new_date'].dt.to_period(freq='A')#년만 나타냄
df['date_m'] = df['new_date'].dt.to_period(freq='M')#년-월 나타냄

print(df.head())

#date_m 열을 df의 새로운 행 인덱스로 지정할 수 있음
df.set_index('date_m', inplace=True)
print(df.head())

```
### 날짜 인덱스 활용
```

#timestamp로 구성된 열을 행 인덱스로 지정하면 datetimeindex 라는 고유값을 가짐
#period 도 마찬가지로 periodindex 라는 속성을 갖는다.

import pandas as pd

df = pd.read_csv('C:/Users/taesa/Desktop/AI섭렵하기/part5/stock-data.csv')

df['new_date'] = pd.to_datetime(df['Date'])
df.set_index('new_date', inplace=True)


print(df.head())
print('\n')
print(df.index)


#포함하는거 출력하기

df_y=df['2018']
print(df_y)
print('\n')
df_ym=df['2018-07']
print(df_ym)
print('\n')
df_ym_cols=df.loc['2018-07', 'Start':'High']
print(df_ym_cols)
print('\n')
df_ymd=df['2018-07-02']
print(df_ymd)
print('\n')
df_ymd_range=df['2018-06-20':'2018-06-25']
print(df_ymd_range)

#timestamp로 두 날짜 사이의 시간 간격 구하기
#시간 간격 계산, 최근 180-189일 사이의 값들만 선택하기

today = pd.to_datetime('2018-12-25')
df['time_delta'] = today - df.index
df.set_index('time_delta', inplace=True)
df_180 = df['180 days':'189 days']
print(df_180)

```




